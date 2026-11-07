# Angular Segment Analysis (ASA): Algorithm Learning Notes

**学習日:** 2026-11-07  
**目的:** 自身のWalkability研究で利用したAngular Segment Analysisについて、計算の仕組みを数理的に説明できるようにする。  
**位置づけ:** 原理の学習メモ。depthmapXのソースコードを解析・再実装した記録ではない。

## 1. ASAの全体像

道路ネットワークをセグメント単位で表し、道路同士を移る際の**方向転換角度（angular change）**をコストとして最小角度経路を求める。その結果から、到達性（Integration）と通過性（Choice）を評価する。

```text
Pedestrian network / Street centre lines
      ↓
Segment map（交差点等で分割）
      ↓
Graph representation（道路セグメントをノード、接続をエッジとして概念化）
      ↓
Angular cost（方向転換角度をコスト化）
      ↓
Least angular paths（最小角度経路の探索）
      ↓
Angular Total Depth / Angular Choice
      ↓
NAIN / NACH（正規化）
```

**注意:** 図はアルゴリズムの理解のための概念図。実際のdepthmapXの内部データ構造や探索アルゴリズムを直接検証したものではない。

## 2. Step 1 — 道路ネットワークのグラフ化

- ノード: 道路セグメント
- エッジ: 接続して移動できるセグメントの関係
- エッジの重み: セグメント間の方向転換コスト

道路形状と接続関係を区別する。交差点が地図上で交差していても、立体交差などで実際には行き来できない場合には接続しない。

## 3. Step 2 — 方向転換角度と角度コスト

2つの進行方向ベクトルを **a**, **b** とすると、

```math
cos(θ) = (a · b) / (|a| |b|)
θ = arccos((a · b) / (|a| |b|))
```

（数式は読みやすい表記として以下にも示す。）

$$
\cos\theta=\frac{\mathbf{a}\cdot\mathbf{b}}{\lVert\mathbf{a}\rVert\lVert\mathbf{b}\rVert}
$$

- `cos θ` は角度コストそのものではなく、角度を求める途中の値。
- ASAの考え方では方向転換が少ないほどコストを小さくする。
- 前の学習で用いた「角度を180°で割る」ような値は**理解用のスケーリング例**。depthmapX内部の角度単位・Tulip bins・重みの扱いと同一だと断定しない。

## 4. Step 3 — 最小角度経路と探索アルゴリズム

複数の経路のうち、**経路全体の方向転換コストの総和が最小**になる経路が least angular path。

例（下記の数値は学習用に仮定した重み）:

| 経路 | 合計コスト |
|---|---:|
| A → C | 0.6 |
| A → B → C | 0.2 + 0.1 = 0.3 |

したがって A → B → C が最小角度経路。

**Dijkstra法の学習上の役割:**
1. 出発ノードを0、ほかを∞に初期化する。
2. 現時点で最小の暫定コストのノードを選ぶ。
3. そのノードから接続するノードへのコストを比較し、小さければ更新する。
4. 繰り返して各ノードへの最小コストを確定する。

Dijkstra法は、非負コストを持つグラフの最短経路を求める一般的な手法。**これはASAの最小コスト探索を理解するための説明であり、depthmapXの特定バージョンが内部でDijkstra法を使うという検証結果ではない。**

## 5. Step 4 — IntegrationとChoice

### Angular Total Depth（ATD）

出発セグメント \(i\) から、対象範囲内の各セグメント \(j\) への最小角度コスト \(d_\theta(i,j)\) の合計:

$$
ATD(i)=\sum_{j\ne i} d_\theta(i,j)
$$

例: AからB,C,Dへの最小コストがそれぞれ0.5, 0.75, 1.25なら、

$$
ATD(A)=0.5+0.75+1.25=2.5
$$

同一条件ならATDが小さいほど、一般にIntegration（到達性）は高い。

### Angular Choice（ACH）

異なる出発地・目的地の組に対して、対象セグメントが**最小角度経路の途中**に含まれる程度を集計する。出発セグメントと到着セグメントを途中通過として数えないのが基礎的な説明。

※同率の最小経路が複数ある場合の配分や計算範囲は実装上の重要な詳細。Choiceは実測通行人数ではなくネットワーク上の構造的な通過ポテンシャル。

## 6. Step 5 — NAINとNACHの正規化

### NAIN (Normalised Angular Integration)

depthmapX公式説明に掲載された式:

$$
\boxed{NAIN=\frac{NC^{1.2}}{ATD}}
$$

- NC = Node Count
- ATD = Angular Total Depth

例: NC=10, ATD=20なら、

$$
NAIN=\frac{10^{1.2}}{20}\approx0.792
$$

**解釈:** NCが同一なら、ATDが小さいほどNAINは高い。

### NACH (Normalised Angular Choice)

depthmapX公式説明に掲載された式:

$$
\boxed{NACH=\frac{\log(ACH+1)}{\log(ATD+3)}}
$$

- ACH = Angular Choice
- ATD = Angular Total Depth
- logは大きい値の差を圧縮する。分子・分母で同じ対数の底を使用する。

例: ACH=9, ATD=97なら、

$$
NACH=\frac{\log10}{\log100}=0.5
$$

**補足:** 正規化は道路数やネットワーク規模の影響を調整するが、道路データの粒度、接続性、分析半径などの差を完全に取り除くものではない。NAINとNACHは必ずしも0〜1に収まらない。

**計算式の一次的な確認先:** depthmapX公式ガイド「Calculate NACh and NAIn」およびHillier, Yang & Turner (2012)。

## 7. 自分のWalkability研究との接続

出典: Terado (2026), *How Does Walkability for Elderly People Differ between Hilly and Flat Urban Areas?*（論文本体のMethods節）。

- 対象: 新百合ヶ丘駅と流山おおたかの森駅
- 徒歩圏: 47 m/min × 15 min = **705 m**
- ASAのMetric Radius: **705 m**（論文本体の記述に基づく）
- 歩行可能ネットワーク: OpenStreetMapをベースにQGISで整備
- 計算: Space Syntax Toolkit と depthmapXnet によるNAIN/NACH
- 独立した勾配評価: 5 m DEMから道路を10 m以下に区切り縦断勾配を算出し、5%・8%の条件と重ね合わせる

**重要:** この研究では、勾配をASAのangular costそのものに組み込んだのではない。ASAで計算した中心性と勾配条件を、後段で重ね合わせて比較した。

## 8. 5分発表用の構成（2026-11-10）

| 時間 | 話すこと | 図・式 |
|---|---|---|
| 0:00–0:35 | なぜASAを理解する必要があるか | 研究の道路ネットワーク |
| 0:35–1:20 | セグメント・グラフ、角度コスト | 2本の道路の折れ角 |
| 1:20–2:05 | 最小角度経路とコスト比較 | 2経路、合計コスト |
| 2:05–3:10 | ATD / Integration と Choice | 4ノードの例 |
| 3:10–4:10 | NAIN/NACHの正規化 | 2つの数式 |
| 4:10–5:00 | 自分の研究への適用と限界 | ASA + DEM勾配の模式図 |

**発表の核心:** 「道路の接続・方向転換を数値化 → 最小角度経路を探索 → 到達性と通過性を集計 → NAIN/NACHで比較可能性を高める」。

## 9. 参考文献・学習資料

1. Turner, A. (2000). *Angular Analysis: A Method for the Quantification of Space*. CASA Working Paper 23, University College London. https://www.ucl.ac.uk/bartlett/publications/2000/jun/casa-working-paper-23  
   — 方向転換を使う解析の考え方、数学的定義とアルゴリズム。
2. Turner, A. (2007). *From Axial to Road-Centre Lines: A New Representation for Space Syntax and a New Model of Route Choice for Transport Network Analysis*. *Environment and Planning B*, 34(3). https://doi.org/10.1068/b32067  
   — 街路中心線、セグメント、ルート選択モデル。
3. Hillier, B., Yang, T., & Turner, A. (2012). *Normalising Least Angle Choice in Depthmap and How It Opens Up New Perspectives on the Global and Local Analysis of City Space*. *Journal of Space Syntax*, 3(2), 155–193. https://discovery.ucl.ac.uk/id/eprint/1389938/  
   — NAIN・NACHの正規化。
4. Hillier, B., & Iida, S. (2005). *Network and Psychological Effects in Urban Movement*. COSIT 2005, LNCS 3693, 475–490. https://doi.org/10.1007/11556114_30  
   — 角度と移動経路の関係。
5. Kolovou, I., Gil, J., Karimi, K., Law, S., & Versluis, L. (2017). *Road Centre Line Simplification Principles for Angular Segment Analysis*. Proceedings of the 11th Space Syntax Symposium. https://discovery.ucl.ac.uk/id/eprint/10042788/  
   — 道路中心線の前処理と結果への影響。
6. depthmapX (official). *How do I? — Calculate NACh and NAIn*. https://github.com/SpaceGroupUCL/depthmapX/blob/master/docs/howdoi.md  
   — 計算式の実装上のガイド。
7. Terado, H. (2026). *How Does Walkability for Elderly People Differ between Hilly and Flat Urban Areas? A Case Study of Shin-Yurigaoka Station and Nagareyama-Otakanomori Station*. ISPRS Archives, L-4/W1-2026, 311–318. https://doi.org/10.5194/isprs-archives-L-4-W1-2026-311-2026

## 10. 次回学習する点

- Metric RadiusとAngular Distanceの役割の違い
- NAIN/NACH計算時の分析範囲・NCの解釈・重みと同率最小経路の扱い
- depthmapX実装／入力データを用いた手計算と出力の照合
- 研究条件（705m、歩道接続、道路中心線の簡略化）の感度と再現性
