# メモ / Notes

> **Summary (EN):** Undated notes page with two sub-notes. The first, dated 2/21, is an early simplification of the design: walking speed fixed at 1.0 m/s, slope handled as a separate terrain indicator (mean slope or share of ≥8% slope area) rather than as a speed correction, a 65+ population share by areal interpolation, and Tana/Eda as candidate comparison stations against Shin-Yurigaoka. The second is a literature-based justification for using Integration (to-movement) as the main indicator and Choice only as a supplementary one. It draws on Hacar et al. 2020, Lerman et al. 2014, Nag et al. 2022 and Koohsari et al. 2016.
>
> **Phase:** 4. Method revision & conference prep (手法見直し・学会準備) · **Tools/Data:** Network isochrones, DEM slope, census small-area population (areal interpolation), Space Syntax literature

[← 作業ログ一覧 / Work log index](README.md)

---

- [付録: 2/21 今後の流れ](#付録-221-今後の流れ)
- [付録: choiceを入れない言い訳](#付録-choiceを入れない言い訳)

---

## 付録: 2/21 今後の流れ

その判断でOKです。締切がある状況で **勾配ごとに速度を変えるモデル**に手を出すと、実装も説明も一気に重くなって、研究の芯（駅間比較）がぼやけます。

代わりに、**速度は固定（1.0 m/s）**のまま、勾配は「別の指標」として扱えば、手間を増やさずに地形要素もちゃんと入ります。

---

### 最短で“研究として成立”させる設計

#### 方針

- 歩行速度：**1.0 m/s固定**
- 15分徒歩圏：ネットワーク等時間圏（作成可能な方法でOK）
- 勾配：徒歩圏内の **地形指標として別途計測**
- 高齢化：徒歩圏内の **65+割合（面積按分）**

#### 勾配は「max」ではなく、次のどれか1つで十分

maxは崖1セルで跳ねるので、発表や要旨では避けた方が安全です。

- **平均勾配（mean）**（すでにやってる）
- **急勾配エリア比率**：徒歩圏内で `slope% ≥ 8%` の面積割合  
  → ラスタの閾値処理→ゾーン統計だけで出せて、説明も簡単で強いです。

これなら「勾配補正（速度変化）」をしなくても、**坂が多い圏かどうか**は定量で言えます。

---

### 明日の作業を軽くするための“成果物セット”

#### 1) 比較駅セット（もう決め打ちでOK）

これまでの結果から、比較の筋が良いのはこれ：

- 新百合ヶ丘（基準）
- **田奈**（平坦寄り＋高齢化率が新百合に近い側）
- **江田**（高齢化率が新百合に最も近い側）
- 予備：市が尾（平坦×高齢化率高めを見せたい時）

#### 2) 岡部先生面談で見せる「最低限の図表」

- 表1：駅別まとめ（これ1枚で説明できる）
  - 駅名
  - 平均勾配（%）
  - 15分徒歩圏の面積（または到達距離・到達施設数）
  - 65+割合（徒歩圏内）
- 図1：15分徒歩圏ポリゴン（新百合 vs 田奈）
- 図2：散布図（例）
  - x：平均勾配
  - y：15分徒歩圏面積（or 到達施設数）
  - 点のラベル：駅名

---

### 面談での説明（1分テンプレ）

> 多摩丘陵圏の郊外駅を対象に、徒歩15分圏（歩行速度1.0 m/s固定）をネットワークで推定し、徒歩圏内の地形（平均勾配／急勾配比率）と、徒歩圏内の高齢化率（町丁人口の面積按分）を同一手順で算出しました。新百合ヶ丘を基準に、青葉区では平坦寄りかつ高齢化率が近い田奈・江田を候補として比較します。勾配を速度補正に直接入れるのは今回は避け、地形指標として別に評価し、比較の明確さと実装の再現性を優先しています。

---

### FOSS4G（academic）要旨の骨組み（そのまま埋める形式）

- 背景：丘陵地×高齢化で徒歩アクセシビリティが課題
- 目的：駅別に15分徒歩圏・地形・高齢化を同一手法で比較
- データ：道路ネットワーク、DEM、町丁年齢人口
- 方法：1.0 m/s固定の等時間圏、勾配指標、面積按分で65+割合
- 結果：新百合と田奈/江田で徒歩圏形状と勾配負荷・高齢化の組合せが異なる（←数値を2〜3個）
- 意義：地形条件と人口構成を同時に見た駅圏比較の枠組み
- 限界：速度固定、人口均一仮定（面積按分）など

---

ここまでを最短で整えるなら、次の1点だけ決めたい：

**面談（2/23）で一番見せたい「主結果」はどれにするか？**

A) 15分徒歩圏の“広がり/面積”の差

B) 15分徒歩圏の“高齢化率”の差

C) “勾配指標（平均 or 急勾配比率）”の差

---

## 付録: choiceを入れない言い訳

該当部分を、**論文を明示しながら**判断材料になる形で整理する。

### まず、前提の意味

Space Syntax では、一般に **Integration は to-movement（行きやすさ・到達性）**、**Choice は through-movement（通り抜けられやすさ）** を表す指標として扱われます。つまり、Integration は「その道へ向かいやすいか」、Choice は「その道が経路として通されやすいか」を見るものです。 ([Springer Link](https://link.springer.com/chapter/10.1007/978-3-030-59140-3_2))

### 1. 「Integration と Choice を両方使う」こと自体は既存研究にある

これは、すでにかなりあります。

Hacar et al. (2020, **OA**) は先行研究レビューの中で、**Lerman and Omer のテルアビブ研究**や**Athens の研究**などを挙げ、**pedestrian movement を説明するために integration と choice を併用した研究がある**と整理しています。つまり、**両方出すこと自体は新規性にはなりにくい**です。 ([MDPI](https://www.mdpi.com/2220-9964/9/10/589))

さらに、Sharmin & Kamruzzaman (2018) のメタ分析では、**Integration と Choice の両方が pedestrian movement と関係する主要指標**として扱われ、特に **angular approach で算出した choice と integration の効果が強い**とまとめられています。ただし同時に、**結果のばらつきは大きい**とも述べています。つまり、文献全体としては「どちらも重要だが、どちらが効くかは文脈依存」です。なお、このメタ分析は検索結果上で **Cited by 138** と表示されています。 ([ScienceDirect](https://www.sciencedirect.com/org/science/article/abs/pii/S0144164722001775))

### 2. 「Integration は効くが、Choice は効かない」例は本当にある

ここは、かなり直接的な論文があります。

Hacar, Gülgen & Bilgi (2020, **OA**) は、大学キャンパスで pedestrian density を目的変数にして、**15種類の Space Syntax 指標**を ordinal logistic regression で比較しました。その結果、**axial integration は有意**で、**choice は pedestrian density に有意な効果を持たなかった**と明記しています。論文中でも、最終的に **integration が最も有効な指標**だと述べています。これは、「Integration の方が効いて、Choice が有意でないこともある」の直接根拠です。 ([MDPI](https://www.mdpi.com/2220-9964/9/10/589))

要するに、この論文が言っているのは、

- 歩行者密度を説明したい場面でも
- Choice を入れれば必ず効くわけではなく
- その場所では Integration の方が説明力が高かった

ということです。 ([MDPI](https://www.mdpi.com/2220-9964/9/10/589))

### 3. 高被引用の実務・計画系研究でも、「Choice は試すが残らない」ことがある

Lerman, Rofè & Omer (2014) の Bat Yam 研究は、検索結果上で **158 Scopus citations** と表示されている、かなり被引用のある歩行モデリング研究です。検索で見える全文コピーでは、彼らは **global/local integration, global/local choice, normalized angular choice など複数の syntactic variables** を候補に入れてモデル化しています。ところが、提示されている最終モデルでは **intensity・commercial fronts・angular mean depth** が残っており、**choice が最終説明変数として採用されていません**。 ([Tel Aviv University](https://cris.tau.ac.il/en/publications/using-space-syntax-to-model-pedestrian-movement-in-urban-transpor/))

この論文から読み取れるのは、

**高水準の歩行モデルでも、Choice は「候補」ではあっても「必須」ではない**

ということです。

つまり、査読で「なぜ Choice を入れなかったのか」と聞かれたとしても、

**“既存研究でも Choice は常に決定的とは限らず、研究目的に応じて指標を絞るのは妥当”**

とは言えます。 ([MDPI](https://www.mdpi.com/2220-9964/9/10/589))

### 4. 逆に、「Choice も効く」例はある

一方で、Choice がまったく不要というわけでもありません。

Nag, Sen & Goswami (2022, **OA**) は、297リンクの pedestrian volume と 45 個の SS 指標の相関を見て、**Integration が最も高い相関**を示し、**Choice も有意な正の相関**を持つと報告しています。数値としては、Integration が **r = 0.60**、Choice が **r = 0.51** でした。つまり、**両方効くが、Integration の方が強い**というタイプです。 ([PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC9299754/))

これはかなり実務的な含意があります。

**Choice を入れると「通り抜けられやすい道」を補足できるが、歩行量や到達性の説明では Integration の方が主役になりやすい場合がある**、ということです。 ([PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC9299754/))

### 5. では、本研究では Choice を入れるべきか

ここが判断の核心です。

#### 入れなくてもよい場合

本研究の主目的が

**「駅を中心とした15分徒歩圏のアクセス構造を見る」**

なら、Integration 中心でも十分に筋は通ります。

実際、Koohsari et al. (2016, **OA**) は、walking for transport を説明するための **space syntax walkability** として **integration を採用**しており、Choice は使っていません。つまり、**“歩いて行きやすい近隣構造”を見たい研究では、Integration 一本でも文献的に防御可能**です。 ([PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC5122155/))

#### 入れた方が意味が出る場合

逆に、

**「駅へ向かう主要路」と「駅周辺を通り抜ける道」が同じなのか違うのか**

まで見たいなら、Choice を入れる意味があります。

そのときの新しさは、**Choice を入れたこと自体**ではなく、

**“丘陵地の駅周辺で、Integration と Choice のズレを、勾配や階段と結びつけて読むこと”**

にあります。

たとえば、

- **高 Integration・低 Choice** なら「駅アクセスには重要だが通り抜け路ではない」
- **低 Integration・高 Choice** なら「抜け道としては重要だが、駅アクセス骨格ではない」
- そこに **高勾配** が重なるなら「通されやすいが高齢者には負担の大きい道」

という読みができます。これは本研究の文脈には合っています。Integration と Choice の役割差自体は既知ですが、**それを勾配条件と合わせる**ところに意味が出ます。 ([Springer Link](https://link.springer.com/chapter/10.1007/978-3-030-59140-3_2))

### 6. 判断を一言で言うと

かなり率直に言うと、こうです。

**Choice を入れるだけでは新規性は弱い。**

ただし、

**「駅アクセスの中心路」と「通り抜け路」の違いを、勾配や現地歩行環境と結びつけて読む**

なら、入れる意味があります。 ([MDPI](https://www.mdpi.com/2220-9964/9/10/589))

### 7. 実務的な結論

FOSS4G の段階での方針：

- **本筋を守るなら**  
  Integration × 勾配 × 現地確認  
  に集中する。これは十分に成立します。 ([PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC5122155/))
- **Choice を入れるなら**  
  主分析ではなく、**補助分析**として入れる。  
  つまり、「高 Integration 道路」と「高 Choice 道路」がどの程度重なるかだけを簡潔に見て、必要なら勾配の違いを一枚図で示す。  
  これなら重くなりすぎません。 ([ScienceDirect](https://www.sciencedirect.com/org/science/article/abs/pii/S0144164722001775))
- **Choice を入れないときの言い訳**  
  「本研究の主眼は、駅を中心とした徒歩15分圏の到達性・アクセス構造の把握であり、to-movement を表す Integration を主指標とした。Choice は through-movement の補足指標として有用だが、先行研究でも常に有意・必須とは限らないため、本稿では主分析からは外した。」  
  これは文献的に十分 defend できます。 ([Springer Link](https://link.springer.com/chapter/10.1007/978-3-030-59140-3_2))
