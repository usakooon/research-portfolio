# 2026-04-04 ワークフロー　分析＋Result

> **Summary (EN):** Consolidated the full workflow in one place: road-inclusion rules and filter expressions (incl. Nagareyama-specific `barrier=kerb`, `service=driveway`), ground/elevated/steps layer cleaning, 10 m-segment grade calculation (values >30% set to NULL), aggregation of mean grade onto Space Syntax segments, and QNEAT3 isochrones (300/600/900 s). Results compare walking speeds (2.8 vs 2.5 km/h at ≤5%: p3/p1 66.7% vs 67.7%) and slope thresholds at 2.8 km/h (5.1% vs 8.0%: p3/p1 rises from 66.7% to 86.8%, while p3/p2 drops from 26.5% to 23.5%), showing the threshold choice strongly shapes the measured centrality–walkability gap. Stairs and pedestrian decks were excluded from DEM-based grade evaluation and noted as a limitation to be checked in the field.
>
> **Phase:** 4. Method revision & conference prep (手法見直し・学会準備) · **Tools/Data:** QGIS, Space Syntax Toolkit, QNEAT3, OpenStreetMap, 基盤地図情報 DEM

[← 作業ログ一覧 / Work log index](README.md)

---

# 1．主解析に入れる道路

- デッキ上の主通路
- デッキから駅へ入る通路
- デッキから街へ出る通路
- デッキと地上を結ぶ階段・EV・スロープ
- 地上の外周歩道
- 横断歩道
- バス停へ行く最低限のアクセス歩道
- 幹線道路、一般道路（2車線以上）の歩道は残し、それ以外は車道を採用

  **主要道路**→ 歩道を別線で残す

  - 主要道路と普通の道路を接続する短いラインはSplit with linesで削除

  **それ以外**

  → 歩道は基本削除して、**交差点で車道ネットワークに吸収（歩道と車道が並行しないように）**

  ```javascript
  "highway"='tertiary' or "highway"='primary'
  ```

  ```javascript
  "service"='parking_aisle' or "indoor"='yes' or "access"='private'
  ```

  流山case

  ```javascript
  "barrier"='kerb'
  "service"='driveway'
  ```
- 敷地内道路（一般歩行者が通らない）
- 駅前デッキは残す。（連絡通路なため）
- パーキングに入る道は削除

その他

- 基本横切れるのは横断歩道があるところだけ
- 一車線の場所は横断歩道関係なく扱う

# 2．QGIS標準機能で代用する場合（推奨ワークフロー）

駅前の立体交差（デッキ）を正確に扱うために、以下の手順で行うのが最も確実です。

### ステップA：データの仕分け（OSM属性の活用）

OSMから抽出したデータを2つのレイヤに分けます。

1. **A：地上レイヤ:** **`layer=0`** またはタグなし（普通の道路・歩道）* 反転させて
   ```javascript
   "layer"=1 or "bridge"='yes' or "tunnel"='yes' or "highway"='steps'
   ```
2. **B：立体レイヤ:** **`layer=1`** 以上、**`bridge=yes`**、**`tunnel=yes`**（ペデストリアンデッキ、地下道）
   ```javascript
   "layer"=1 or "bridge"='yes' or "tunnel"='yes'
   ```
3. C：highway=steps

### ステップB：地上レイヤのトポロジークリーニング

Aのレイヤを使用

1. **重複の削除:** プロセシング ＞ 「重複ジオメトリを削除 (Delete duplicate geometries)」
2. **交差点での分割:** プロセシング ＞ **「split with lines」** (ツールボックス内)。**＊道路整備の段階で完了済みだからやらなくて良い**
3. **スナップ処理:** プロセシング ＞ 「ジオメトリのスナップ (Snap geometries to layer)」。
   - 自分自身のレイヤを指定し、許容値を **`0.5m`** にして実行。
   - 最近点を優先するが、頂点は追加しない

### ステップC：単純化（Douglas-Peucker）

**A地上レイヤとB立体レイヤ**の両方に適用します。

1. プロセシング ＞ **「簡略化 (Simplify)」**。
   - 手法: **Douglas-Peucker**。
   - 許容値: 日本の狭い道なら **`1.0m`** （建物に被らない程度）。

### ステップD：駅前デッキの結合と階段の作成

ここで「立体レイヤ」を戻します。

1. **マージ:** 「ベクタレイヤのマージ」でA,B,Cを1つにします。

## ステップE：ジオメトリクリーニング

- 重複削除：「重複ジオメトリを削除」
- 無効ジオメトリ修正 : 「ジオメトリを修復」
  - ラインワーク

highway_completed_clip

## ステップF：verify layerで不必要なものを削除

- island, orphanなど

# 3．道路に grade(%)（縦断勾配）を付与する作業フロー（簡潔版）

---

## 0) 前提

- 入力：道路ライン（OSM等）、DEM（標高ラスタ）
  - [基盤地図情報ダウンロードサービス](https://service.gsi.go.jp/kiban/)
- 目的：各道路セグメントの **grade(%) = |Z_end − Z_start| / length × 100** を付与
- 分割長：**最大10m**（※分母は固定10ではなく各線分の実長）

---

## 1) 道路ラインを最大10mで分割

- ツール：**最大長で線を切断（Split lines by maximum length）**
- 最大長：10 m

  → 出力：10m以下の短い道路セグメント（最後は10m未満あり）
- 分割後セグメントに一意IDを付与（例：`seg_id`）

> [!NOTE]
> 💡 ※田奈駅ではfidが出力されなかったため、新規フィールド作成で、`$id`で新規idを作った

---

## 2) 分割後セグメントから「始点点」と「終点点」を作る

- ツール：**線の始点（Extract specific vertices：vertex=0）**
  - 入力：分割後セグメント：10ｍ
  - 出力：start_points
- ツール：**線の終点（Extract specific vertices：vertex=-1）**
  - 出力：end_points

---

## 3) DEMから標高を点に付与

- ツール：**ベクタレイヤにラスタ値を付加（Sample raster values）**
  - 入力：start_points → 出力：start_points_z（Zstart列）
  - 入力：end_points → 出力：end_points_z（Zend列）

---

## 4) 点の標高を「分割後セグメント」に戻す（IDで結合）

- start_points_z と end_points_z にも同じ ***fid*** が残る前提で進める
- ツール：属性テーブルで結合(table join)
  - セグメント ← start_points_z を ***fid*** で結合（Zstartを持たせる）
  - セグメント ← end_points_z を ***fid*** で結合（Zendを持たせる）

※「空間結合」でも可だが、点が線端にあるので誤結合を避けるなら **ID結合が安定**

Footway_slope

---

## 5) grade(%) を計算して列を作る

- フィールド計算機で新規列 `grade_pct`（実数）
- Length: `$length`
- 式：
  ```javascript
  abs("Zend" - "Zstart") / $length * 100
  ```

  - [傾きの割合と傾き角を求める | アメリカ地質調査所](https://www.usgs.gov/educational-resources/determine-percent-slope-and-angle-slope)

ポイント：

- `$length` は **分割後の各セグメント長**（10m未満も反映）
- 分母を10固定にはしない

---

## 6) 橋の処理（原則 0%）　保留

- OSM属性などで `bridge` 判定できるならフラグ `is_bridge`
- `grade_pct_final` を作って：
  - `CASE WHEN "is_bridge" = 1 THEN 0 ELSE "grade_pct" END`

（湾曲橋は「湾曲」自体ではなくDEMが床版を表さない問題なので、0%扱いが一番壊れにくい）

これの代わりに、grade_pct_finalフィールドを作成し、下のコードを実行

```javascript
CASE WHEN grade_pct > 30 THEN NULL ELSE grade_pct END
```

---

## 7) 出力

- 分割済み道路セグメントに `grade_pct_final` が付いた状態

  → このまま「低勾配（≤8%）」判定や集計に使用

# 4．10m grade → SSセグメントに集約（QGISフロー）

## 前提レイヤ

- **A：road_10m_grade**（10m分割道路。`grade_clean` が入ってる）
- **B：ss_segments**（Space Syntax Toolkitの交差点分割セグメント。Integration/NAIN/NACH/segLENなど）

---

## STEP 1：10m側の異常値処理（もうやってればOK）

10mレイヤ（A）に `grade_clean` を作る

- `CASE WHEN "grade_pct" > 30 THEN NULL ELSE "grade_pct" END`

※道路フィーチャは消さず、値だけNULLにするのが基本。

---

## STEP 2：SSセグメントに grade を“空間結合（集計付き）”で付ける

プロセシングツール：

**「属性の空間結合（集計付き）」**（Join attributes by location (summary)）

設定：

- **入力レイヤ（結合先）**：B（ss_segments）
- **比較対象（結合元）**：A（road_10m_grade）
- 空間関係：**交差する (intersects)**
- 集計するフィールド：`grade_pct_final`
- 集計関数：まずは **mean**（平均）

  （余裕があれば max も一緒に作ると後で比較できる）
- 出力：`ss_segments_with_grade`

結果：

- SSセグメントに `grade_clean_mean`（＋必要なら`grade_clean_max`）が追加される

# 4．QNEAT3（ローカルネットワークで等時間圏“ポリゴン”まで作れる）

**できること：等時間圏（Iso-Area）を“ポリゴン”で出しやすい。OD行列も可。**([root676.github.io](https://root676.github.io/))

### 手順（いちばん実務的）

1. プラグイン **QNEAT3** を入れる ([plugins.qgis.org](https://plugins.qgis.org/plugins/QNEAT3/))
2. Processingで

   **QNEAT3 → Iso-Area (as Polygon) (from Point / from Layer)** を選ぶ

   - 1点なら from Point
   - 複数点なら from Layer
3. パラメータの要点
   - **Network layer**：道路
   - **Start point(s)**：駅点
   - **Cost**：時間コスト（900秒など）
   - **Default speed**：徒歩速度（m/s換算で入れる流儀もある）
   - countor interval: 300, 600, 900
   - default directions: both directions
4. 出力：**等時間圏ポリゴン**（＋中間成果として到達道路）

### QNEAT3が強い理由（本研究向け）

- **道路属性を自分で作ってコストにできる**ので、

  ここに「勾配補正の歩行時間」を入れれば、本研究の狙い（坂の効き）を実装できる。([root676.github.io](https://root676.github.io/))

5. 準備完了した道路データにクリップ

# 5．Result

## 2.5km/h ＋勾配5.0％以下

all

<!-- image: download failed -->

p1

<!-- image: download failed -->

p2

<!-- image: download failed -->

p3

<!-- image: download failed -->

| length_m | Sum (m) | Sum (km) | all比（%） |
| --- | --- | --- | --- |
| p1 | 6193.221 | 6.193 | 20.755 |
| p2 | 16620.597 | 16.620 | 55.700 |
| p3 | 4193.931 | 4.193 | 14.055 |
| all | 29838.988 | 29.838 | 100.000 |

| 区分 | 意味 | 延長 (km) | 割合 |
| --- | --- | --- | --- |
| p1 | 高統合（Integration 上位20%） | 6.193 | 20.755% |
| p2 | 低勾配（grade_pct_final_mean <= 8.0） | 16.620 | 55.700% |
| p3 | 高統合 ∩ 低勾配 | 4.193 | 14.055% |
| all | 全道路 | 29.838 | 100.000% |

# 勾配5.0%以下＋2.5km/hと2.8km/hの比較表

| 区分 | 意味 | 2.8km/h版 延長(km) | 2.8km/h版 割合 | 2.5km/h版 延長(km) | 2.5km/h版 割合 |
| --- | --- | --- | --- | --- | --- |
| p1 | 高統合（Integration 上位20%） | 7.694 | 21.194% | 6.193 | 20.755% |
| p2 | 低勾配 | 19.387 | 53.402% | 16.620 | 55.700% |
| p3 | 高統合 ∩ 低勾配 | 5.135 | 14.146% | 4.193 | 14.055% |
| all | 全道路 | 36.305 | 100.000% | 29.838 | 100.000% |

| 指標 | 意味 | 2.8km/h版 | 2.5km/h版 |
| --- | --- | --- | --- |
| p3 / p1 | 高統合道路のうち低勾配な割合 | 66.745% | 67.718% |
| p3 / p2 | 低勾配道路のうち高統合な割合 | 26.489% | 25.233% |

# 勾配8.0%以下＋2.8km/h

p1

<!-- image: download failed -->

p2

<!-- image: download failed -->

p3

<!-- image: download failed -->

all

<!-- image: download failed -->

| 区分 | 意味 | 延長 (km) | 割合 |
| --- | --- | --- | --- |
| p1 | 高統合（Integration 上位20%） | 7.694 | 21.194% |
| p2 | 低勾配（勾配8.0%以下） | 28.432 | 78.316% |
| p3 | 高統合 ∩ 低勾配 | 6.678 | 18.396% |
| all | 全道路 | 36.305 | 100.000% |

| 指標 | 意味 | 値 |
| --- | --- | --- |
| p3 / p1 | 高統合道路のうち低勾配な割合 | 86.800% |
| p3 / p2 | 低勾配道路のうち高統合な割合 | 23.490% |

# 2.8km/h 条件における勾配閾値比較

### 1. 延長比較表

| 区分 | 意味 | 5.1%版 延長 (km) | 5.1%版 割合 | 8.0%版 延長 (km) | 8.0%版 割合 |
| --- | --- | --- | --- | --- | --- |
| p1 | 高統合（Integration 上位20%） | 7.694 | 21.194% | 7.694 | 21.194% |
| p2 | 低勾配 | 19.387 | 53.402% | 28.432 | 78.316% |
| p3 | 高統合 ∩ 低勾配 | 5.135 | 14.146% | 6.678 | 18.396% |
| all | 全道路 | 36.305 | 100.000% | 36.305 | 100.000% |

---

### 2. 派生指標の比較

| 指標 | 意味 | 5.1%版 | 8.0%版 |
| --- | --- | --- | --- |
| p3 / p1 | 高統合道路のうち低勾配な割合 | 66.745% | 86.800% |
| p3 / p2 | 低勾配道路のうち高統合な割合 | 26.489% | 23.490% |

---

### 3. 差分整理

| 項目 | 変化量 |
| --- | --- |
| p2 延長 | +9.044 km |
| p3 延長 | +1.543 km |
| p2 の all比 | +24.914 pt |
| p3 の all比 | +4.250 pt |
| p3 / p1 | +20.055 pt |
| p3 / p2 | -2.999 pt |

---

### 4. 結果の要点

- 低勾配道路（p2）は、5.1%条件では 19.387 km だったのに対し、8.0%条件では 28.432 km まで増加した。
- また、高統合かつ低勾配な道路（p3）も、5.135 km から 6.678 km に増加した。
- 特に、高統合道路のうち低勾配道路が占める割合（p3/p1）は、66.745% から 86.800% に上昇した。
- その一方で、低勾配道路全体の中で高統合道路が占める割合（p3/p2）は、26.489% から 23.490% にやや低下した。

---

### 6. 解釈メモ

この結果は、勾配条件を厳しく設定した場合には、高統合道路の中でも実際に歩きやすい道路がかなり絞られることを示している。一方、勾配閾値を 8.0% に緩和すると、高統合道路の大部分が低勾配道路として含まれるようになる。したがって、勾配閾値の設定は、「中心道路」と「歩行しやすい道路」とのギャップをどの程度強く捉えるかに大きく影響すると考えられる。

---

# 限界

本研究では、道路ネットワーク上の地表歩行空間を対象として、DEM に基づく地形由来の縦断勾配を評価した。一方、階段やペデストリアンデッキ等の立体歩行施設については、DEM による勾配表現が適切でなく、また本研究の分析手法では十分に扱うことが難しいため、本分析では勾配評価の対象外とした。これらの要素については、実地調査および地図上で位置を確認し、歩行評価の際に補足的に検討する。
