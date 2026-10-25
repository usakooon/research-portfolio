# 2026-03-16 Abstract English ver

> **Summary (EN):** Expanded the abstract into a full English version (~1,000 words) that frames the research question — do structurally central roads in a hilly suburb also work as physically walkable routes for older adults? — and reports the Shin-Yurigaoka results (high-integration 23.9%, low-gradient ≤8.0% 84.7%, both 21.6%; 90.8% vs 25.6% asymmetry). A proofread version with in-text citations (Hillier et al., 1993; Saelens & Handy, 2008; Barnett et al., 2017) and two literature-check notes supporting the Space Syntax and Japanese walkability/aging background claims are attached as appendices.
>
> **Phase:** 4. Method revision & conference prep (手法見直し・学会準備) · **Tools/Data:** Space Syntax literature, walkability/aging literature, Japanese government statistics, Kawasaki City / Asao Ward surveys

[← 作業ログ一覧 / Work log index](README.md)

---

### Analysis of the Gap Between Walkability for Older Adults and the Actual Physical Environment Using Gradient-Aware Network Analysis: A Case Study of the Area Surrounding Shin-Yurigaoka Station

**This study aims to evaluate walkability for older adults in the area surrounding Shin-Yurigaoka Station, located in a hilly suburban urban environment, by analyzing the street network as a whole through a gradient-aware network approach.**

**In Japan, walkability has become an increasingly important issue in the context of rapid population aging. Older adults continue to go out frequently in daily life, and walking remains one of their major travel modes. In this context, the ease of walking is shaped not only by distance or network connectivity, but also by slope-related physical burden. This issue is particularly relevant in Japanese suburban hill areas, where many residential districts were developed on hilly terrain and where access to stations, shops, and everyday services often requires negotiating slopes and stairways.**

**Against this background, this study focuses on the area around Shin-Yurigaoka Station in Asao Ward, Kawasaki City, as a case study for examining how older adults’ walkability can be understood through the combined perspective of street-network structure and slope conditions.**

**Asao Ward is an appropriate study area for two reasons. First, according to the 2020 Municipal Life Tables published in 2023, Asao Ward ranked first in Japan in average life expectancy for both men and women, making it a notable long-living urban area. Second, despite its hilly topography, a survey conducted by Kawasaki City reported that a large proportion of older residents in the ward stated that they were able to walk for about 15 minutes or that they did so in daily life. This suggests that walking remains an important mode of everyday mobility for older adults even in an area with many slopes.**

**This raises a key analytical question: in a hilly urban environment, do structurally central roads in the street network also function as physically walkable routes for older adults? To address this, the study examines the degree of overlap between network centrality and low-slope conditions.**

**Previous studies in Space Syntax have long argued that configurational measures such as Integration are related to pedestrian movement, particularly through the theory of Natural Movement. However, broader walkability research has conceptualized walkability as a multidimensional construct shaped not only by network connectivity, but also by access to destinations, land-use mix, safety, streetscape quality, and pedestrian infrastructure. In this sense, network centrality alone is insufficient to explain practical walkability, especially for older adults. This limitation is more important in hilly urban areas, where slope increases the physical burden of walking and can weaken the relationship between network centrality and actual walkability. Therefore, this study introduces a gradient-aware network analysis that combines street-network centrality with topographic conditions to provide a more realistic evaluation of walkability for older adults.**

**The analysis was conducted using open geospatial data and open-source software. Road network data were obtained from OpenStreetMap, and a 5 m digital elevation model (DEM) was used to represent topography. In order to better focus on walkable public routes, roads classified as parking areas, private roads, and indoor roads were excluded from the analysis. The spatial extent of the study was defined as the 15-minute walking catchment around Shin-Yurigaoka Station, based on an assumed walking speed of 1.0 m/s for older adults. Within QGIS, roads were divided into 10 m segments, and the longitudinal gradient of each segment was calculated from the elevation difference between its start and end points. In parallel, Integration values were derived through Angular Segment Analysis at radius 900 m (R900) using DepthmapX and the Space Syntax Toolkit. These values were then assigned to road segments separated at intersections. By integrating these variables, the study constructed a gradient-aware network analysis framework in which each road segment could be evaluated both in terms of its configurational importance within the overall network and its walking difficulty in terms of slope.**

**The results reveal several important patterns. Within the 15-minute walking catchment of Shin-Yurigaoka Station, high-integration roads, defined as the top 20% of the network by Integration value, accounted for 23.9% of the total road length. Low-gradient roads, defined here as roads with a gradient of 8.0% or less based on Japanese sidewalk design standards, accounted for 84.7%. Roads satisfying both conditions simultaneously—namely, roads that were both highly integrated and low in gradient—accounted for 21.6% of the total. Looking more closely at the overlap between these two conditions, 90.8% of high-integration roads were also low-gradient roads. This indicates that a large proportion of the structurally central roads around the station are also relatively easy to walk in terms of slope. In contrast, only 25.6% of low-gradient roads were classified as high-integration roads. This means that although gentle-slope roads are widely distributed across the study area, they do not necessarily form the core structure of the street network.**

**These findings suggest that walkability for older adults in a hilly urban area should be evaluated not only by the physical ease of slope, but also by the role that a road plays within the overall network. Put differently, roads that are easy to walk are not always the roads that occupy a central or strategic position in everyday movement patterns. This is an important point for understanding mobility in suburban hill areas, where topography may reshape the relationship between urban structure and practical pedestrian accessibility. The study also indicates the value of combining open street data, elevation data, and open-source spatial analysis tools to examine this issue from a reproducible and scalable perspective.**

**At the present stage, this study provides a fundamental quantitative assessment based on network structure and gradient conditions.**

**As a next step, road data in OpenStreetMap and QGIS will be refined so that the analytical network more closely reflects actual pedestrian space, including sidewalks and other walkable links. The analysis will also be extended to a 30-minute walking catchment in order to compare how the relationship between centrality and gradient changes at a broader spatial scale. In addition, comparison with the actual physical environment will be conducted in future work to examine the validity and applicability of gradient-aware network analysis for evaluating walkability for older adults in hilly urban areas.**

---

## 付録: 添削済み

This study aims to evaluate the walkability of the area surrounding Shin-Yurigaoka Station for older adults, which is located in a hilly, suburban environment. This will be achieved by analysing the street network as a whole using a gradient-aware network approach.

In Japan, walkability has become an increasingly important issue in the context of rapid population ageing. Older adults continue to go out frequently in their daily lives and walking remains one of their main modes of travel. In this context, the ease of walking is shaped not only by distance or network connectivity, but also by the physical burden of slopes. This issue is particularly relevant in Japanese suburban hilly areas, where many residential districts have been developed on hilly terrain, often requiring residents to negotiate slopes and stairways to access stations, shops, and everyday services.

Against this background, this study focuses on the area around Shin-Yurigaoka Station in Asao Ward, Kawasaki City, examining how the walkability of older adults can be understood through a combined perspective of street network structure and slope conditions.<br>
Asao Ward is an appropriate study area for two reasons. Firstly, according to the 2020 Municipal Life Tables published in 2023, Asao Ward had the highest average life expectancy in Japan for both men and women, making it a notable area for longevity. Secondly, despite its hilly terrain, a Kawasaki City survey found that a significant proportion of older residents in the ward reported being able to walk for around 15 minutes or doing so in their daily lives. This suggests that walking remains an important mode of everyday mobility for older adults, even in areas with many slopes.

This raises a key analytical question: in hilly urban environments, do roads that are structurally central to the street network also function as physically walkable routes for older adults? To address this question, the study examines the degree of overlap between network centrality and low-slope conditions.

In Space Syntax research, configurational measures such as Integration have been linked to pedestrian movement (Hillier et al., 1993). In contrast, walkability is understood to be a multidimensional concept influenced not only by road connectivity, but also by factors such as access to destinations, land use and safety (Saelens & Handy, 2008). Studies of older adults have also shown that walking and physical activity are influenced by neighbourhood environmental factors, including walkability, access to destinations, and pedestrian infrastructure (Barnett et al., 2017). These findings suggest that street-network centrality alone may not fully explain actual walkability, particularly in hilly areas. Therefore, this study introduces a gradient-aware network analysis that integrates street-network centrality with topographic conditions.

The analysis was conducted using open geospatial data and open-source software. Road network data were obtained from OpenStreetMap and a 5-metre digital elevation model (DEM) was used to represent topography. To better focus on walkable public routes, roads classified as parking areas, private roads and indoor roads were excluded from the analysis. The study area was defined as a 15-minute walking catchment around Shin-Yurigaoka Station, based on an assumed walking speed of 1.0 m/s for older adults. In QGIS, the roads were divided into 10 m segments and the longitudinal gradient of each segment was calculated based on the elevation difference between its start and end points. In parallel, Integration values were derived through Angular Segment Analysis at a radius of 900 m (R900) using DepthmapX and the Space Syntax Toolkit. These values were then assigned to road segments at intersections. Integrating these variables enabled the study to construct a gradient-aware network analysis framework, evaluating each road segment in terms of its configurational importance within the overall network and walking difficulty due to slope.

The results reveal several important patterns. Within the 15-minute walking catchment area of Shin-Yurigaoka Station, high-integration roads — defined as the top 20% of the network in terms of integration value — accounted for 23.9% of the total road length. Low-gradient roads, defined as having a gradient of 8.0% or less based on Japanese sidewalk design standards, accounted for 84.7% of the total. Roads that satisfied both conditions simultaneously — namely, roads that were both highly integrated and low in gradient — accounted for 21.6% of the total. Examining the intersection of these two conditions more closely, it was found that 90.8% of high-integration roads were also low-gradient roads. This suggests that many of the structurally central roads around the station have relatively gentle slopes. In contrast, only 25.6% of low-gradient roads were classified as highly integrated. This suggests that, while gentle-slope roads are widely distributed across the study area, they do not necessarily form the core of the street network.

These findings suggest that the walkability of a hilly urban area for older adults should be evaluated not only in terms of the physical ease of the slope, but also in terms of the role that a road plays within the overall network. In other words, roads that are easy to walk on do not necessarily occupy a central or strategic position in everyday movement patterns. This is an important consideration when it comes to understanding mobility in suburban hilly areas, where the topography can reshape the relationship between urban structure and practical pedestrian accessibility. The study also demonstrates the value of using open street and elevation data alongside open-source spatial analysis tools to examine this issue from a reproducible and scalable perspective.

As a next step, road data in OpenStreetMap and QGIS will be refined to ensure the analytical network more accurately reflects the actual pedestrian environment, including sidewalks and other walkable links. The analysis will also be extended to a 30-minute walking catchment area to compare the relationship between centrality and gradient at a broader spatial scale. Furthermore, future work will validate the gap between the analytical results and the actual physical environment in order to examine the validity and applicability of gradient-aware network analysis for evaluating walkability for older adults in hilly urban areas. (996words)

---

## 付録: Space Syntax先行研究の査読

この段落は、そのままだと**複数の研究系譜を1つに圧縮した要約文**になっています。なので、厳密に言うと、各文にはそれぞれ別の根拠があります。下では、abstract草稿の英文を4つの主張に分解して、**どこまでが既存研究で直接言えることか／どこからが文献を踏まえた推論か**を明確にします。なお、被引用数はデータベースで変動するので、ここではweb上で確認できた値を採用します。 ([ideas.repec.org](https://ideas.repec.org/a/sae/envirb/v20y1993i1p29-66.html))

### 1. 「Space Syntax では Integration と歩行者流動の関係が長く議論されてきた」の根拠

これはかなり強い主張で、古典的な基礎文献があります。

Hillier らの **“Natural Movement”** は、都市の歩行・交通流を、単なる目的地吸引だけでなく、**街路網のコンフィギュレーションそのものが一定の移動パターンを生み出す**という立場で説明しました。そして同論文では、移動パターンと最も強く結びつくグローバルな指標として、**Integration が一貫して重要**だと述べています。つまり、草稿の文の前半――「Integration などのネットワーク指標が歩行者移動と関係する」――は、この系譜にかなり素直に乗っています。さらに Hillier & Iida (2005) でも、**街路網の構成指標と人流の相関は Space Syntax 研究で繰り返し確認されてきた**という整理がされています。 ([ideas.repec.org](https://ideas.repec.org/a/sae/envirb/v20y1993i1p29-66.html))

ただし、ここで注意点があります。

Natural Movement が強く言っているのは、**「街路網の形が移動の基本パターンを強く規定する」ということであって、「歩きやすさの全体」を説明する**とは言っていません。つまり、Integration は**移動の構造的ポテンシャル**には強いが、**高齢者にとっての身体的負荷や歩道環境まで単独で表すわけではない**、というのが厳密な理解です。ここを区別しておくと、abstract はかなり安全になります。 ([ideas.repec.org](https://ideas.repec.org/a/sae/envirb/v20y1993i1p29-66.html))

### 2. 「walkability は connectivity だけではなく、多面的概念である」の根拠

これは Space Syntax とは別系統の、**歩行環境・都市計画・公衆衛生**の文献に基づく主張です。

まず Saelens & Handy (2008) の大規模レビューでは、**交通目的の歩行**は、密度、非居住用途への距離、土地利用混在と一貫した正の関係があり、**ネットワーク接続性、オープンスペース、安全性**なども歩行との関連要素として整理されています。さらに Ewing & Cervero (2010) のメタ分析では、**歩行は land-use diversity、intersection density、徒歩圏内の destinations の数と特に強く関係する**と要約されています。したがって、草稿の文にある「destinations, land-use mix, connectivity」は、かなり標準的な walkability 文献の理解です。 ([PubMed](https://pubmed.ncbi.nlm.nih.gov/18562973/))

そこに加えて、Ewing らの研究は、walkability を単なる粗い指標だけでなく、**imageability, enclosure, human scale, transparency, complexity** のような、街路景観レベルのデザイン特性としても測ろうとしました。これは、草稿の英文にある **“safety, comfort, and pedestrian infrastructure”** のような、よりミクロな歩行体験の側面を入れる理屈に近いです。つまり、walkability は「道がつながっているか」だけではなく、**歩きやすく感じる・実際に歩ける街路空間になっているか**まで含む概念として発展してきた、という理解です。 ([Active Living Research](https://activelivingresearch.org/sites/activelivingresearch.org/files/JPAH_15_Ewing.pdf))

### 3. 「高齢者にとっては、walkability はさらに destination・安全・歩道環境などを含む」の根拠

高齢者に絞ると、この点はさらに明確です。

Barnett らの系統的レビュー・メタ分析では、 older adults の身体活動や歩行と正に関連する環境要素として、**walkability、犯罪安全性、目的地・サービスへのアクセス、レクリエーション施設、公園・オープンスペース、商業施設、緑・景観、歩行フレンドリーなインフラ、公共交通へのアクセス**が挙げられています。つまり、高齢者の歩行は、ネットワークのつながりだけでなく、**安全に休みながら移動できるか、行く先があるか、歩道が歩きやすいか**に強く左右されます。 ([PubMed](https://pubmed.ncbi.nlm.nih.gov/28784183/))

Cerin らの older adults の active travel に関するレビュー・メタ分析でも、**walkability、住宅密度、street connectivity、destination access、land-use mix、shops、public transport、parks、pedestrian-friendly features、benches、streetlights** などが正の関連要因として示されています。特に高齢者では、**ベンチや歩道の歩きやすさ、家から安全に出られること**が重要だと整理されており、これは若年層向けの単純なネットワーク中心性指標だけでは取りこぼしやすい要素です。草稿の英文の「walkability is multidimensional」という文は、older adults 文脈ではむしろ強化されます。 ([Springer](https://link.springer.com/article/10.1186/s12966-017-0471-5))

### 4. 「network centrality alone does not fully explain walkability」は何に基づくか

ここは大事ですが、**単独の1本の論文がそのまま言っているわけではありません**。

これは、上の2系統の文献――

1. Space Syntax が捉える**構造的中心性・構成的アクセシビリティ**、
2. walkability 研究が重視する**目的地・安全・景観・インフラ・休憩可能性**

   ――を重ねたときに導かれる**総合的な解釈**です。

要するに、Integration は「その道がネットワーク上どれだけ中核か」を表すには有効ですが、**その道が高齢者にとって実際に歩きやすいか**には、それ以外の条件も必要です。だから、

**“network centrality alone does not fully explain walkability”**

は、文献上かなり妥当な表現ですが、厳密には**レビュー文献からの推論的総括**として書くのが安全です。断定を強めすぎない方が査読には通りやすいです。 ([PubMed](https://pubmed.ncbi.nlm.nih.gov/18562973/))

### 5. 「丘陵地では slope が歩行負荷を高めるので、この限界はより重大になる」の根拠

これは2段階で支えるのが安全です。

第1に、**勾配が歩行のエネルギーコストを変える**こと自体は、Minetti らの古典的研究でかなり強く示されています。1993年の研究では、上り・平地・下りで代謝コストと機械的仕事が測られ、**歩行の最も経済的な勾配は約 -10.2%**、つまり勾配によって歩行コストが大きく変わることが示されました。2002年の研究でも、**上りではエネルギーコストが増大し、下りでも一定範囲を超えると再びコストが上がる**ことが示されています。したがって、「坂は身体的負荷を変える」という部分は、かなり堅い生理学的根拠があります。 ([PubMed](https://pubmed.ncbi.nlm.nih.gov/8145168/))

第2に、**高齢者では歩行そのもののエネルギー効率が年齢とともに悪化しやすい**ことが知られています。Schrack ら (2012) は、**65歳以降では 1m あたりの歩行エネルギー消費が増加**し、遅い歩行速度はその増大したエネルギーコストを相殺するための代償的戦略かもしれないと述べています。さらに Schrack ら (2016) は、**歩行のエネルギーコストが高い人ほど gait speed decline が進みやすく、slow gait のリスクも高い**ことを示しました。したがって、勾配が歩行コストを上げることと、高齢者が歩行効率低下に敏感であることを組み合わせると、**坂の多い環境では slope が older adults の walkability を実質的に左右する**という推論は十分に妥当です。 ([PubMed](https://pubmed.ncbi.nlm.nih.gov/23035640/))

ただし、ここも厳密には、

**「高引用の古典論文が、丘陵都市の高齢者 street network における slope × centrality の相互作用を直接証明している」わけではありません。**

あるのは、

- 勾配は歩行コストを変える
- 高齢者は歩行コスト上昇に弱い
- walkability は中心性だけでは決まらない

  という3本の文献束です。なので、草稿の英文の最後

  **“slope may alter the relationship between spatial centrality and actual walkability”**

  は、**直接実証の引用というより、既存研究を踏まえた研究仮説・理論的帰結**として置くのが最も正確です。 ([PubMed](https://pubmed.ncbi.nlm.nih.gov/18562973/))

### 6. 段落を、より査読に強い形で言い換えるなら

今の文は方向性は正しいですが、査読を意識するなら少しだけ表現を締めた方がよいです。例えばこうです。

> Previous studies in Space Syntax have long argued that configurational measures such as Integration are related to pedestrian movement, particularly through the theory of Natural Movement. However, broader walkability research has conceptualized walkability as a multidimensional construct shaped not only by network connectivity, but also by access to destinations, land-use mix, safety, streetscape quality, and pedestrian infrastructure. In this sense, network centrality alone is insufficient to explain practical walkability, especially for older adults. This limitation is likely to be more pronounced in hilly urban environments, where slope increases the physical cost of walking and may further mediate the relationship between spatial centrality and experienced walkability. ([ideas.repec.org](https://ideas.repec.org/a/sae/envirb/v20y1993i1p29-66.html))

この書き方の利点は、

- **long argued / conceptualized / is likely to** として断定を少し弱めている
- 「直接証明済み」と「文献から妥当に導ける推論」を分けている
- older adults と hilly context の必然性が自然につながる

  ことです。 ([ideas.repec.org](https://ideas.repec.org/a/sae/envirb/v20y1993i1p29-66.html))

### 7. 参考文献候補（今回の条件を満たすもの中心）

今回の段落の根拠として、まず優先すべき文献はこのあたりです。いずれも確認できた範囲で**被引用100超**です。 ([ideas.repec.org](https://ideas.repec.org/a/sae/envirb/v20y1993i1p29-66.html))

- Hillier, B., Penn, A., Hanson, J., Grajewski, T., & Xu, J. (1993). *Natural Movement: Or, Configuration and Attraction in Urban Pedestrian Movement.*
- Hillier, B., & Iida, S. (2005). *Network and Psychological Effects in Urban Movement.*
- Saelens, B. E., & Handy, S. L. (2008). *Built environment correlates of walking: a review.*
- Ewing, R., & Cervero, R. (2010). *Travel and the Built Environment.*
- Ewing, R., Handy, S., Brownson, R. C., Clemente, O., & Winston, E. (2006). *Identifying and Measuring Urban Design Qualities Related to Walkability.*
- Barnett, D. W., Barnett, A., Nathan, A., Van Cauwenberg, J., & Cerin, E. (2017). *Built environmental correlates of older adults' total physical activity and walking: a systematic review and meta-analysis.*
- Cerin, E., Nathan, A., Van Cauwenberg, J., Barnett, D. W., & Barnett, A. (2017). *The neighbourhood physical environment and active travel in older adults: a systematic review and meta-analysis.*
- Minetti, A. E., Ardigò, L. P., & Saibene, F. (1993). *Mechanical determinants of gradient walking energetics in man.*
- Minetti, A. E., Moia, C., Roi, G. S., Susta, D., & Ferretti, G. (2002). *Energy cost of walking and running at extreme uphill and downhill slopes.*
- Schrack, J. A., Simonsick, E. M., Chaves, P. H. M., & Ferrucci, L. (2012). *The role of energetic cost in the age-related slowing of gait speed.*
- Schrack, J. A., et al. (2016). *Rising Energetic Cost of Walking Predicts Gait Speed Decline With Aging.* ([ideas.repec.org](https://ideas.repec.org/a/sae/envirb/v20y1993i1p29-66.html))

---

## 付録: Walkability研究の重要性　査読

この3文は、日本の文脈に寄せるなら、**「高齢社会」「高齢者の日常外出における徒歩の重要性」「斜面住宅地・丘陵郊外での勾配負荷」**の3本で支えるのがいちばん自然です。以下、文ごとに分解します。

---

### 1. “In recent years, walkability has become an increasingly important topic in aging societies…”

この文の根拠は、日本ではかなり明確です。

日本はすでに超高齢社会で、内閣府の令和7年版高齢社会白書では、**65歳以上人口は3,624万人、高齢化率は29.3%**とされ、今後も高齢化率は上昇し、**2070年には38.7%**に達すると推計されています。総務省統計局の2025年推計でも、**65歳以上人口比率は29.4%で過去最高**です。つまり、日本の都市や生活圏を考えるとき、高齢者の日常移動をどう支えるかは中心的な課題です。 ([内閣府](https://www8.cao.go.jp/kourei/whitepaper/w-2025/html/gaiyou/s1_1.html))

加えて、国土交通省は「居心地が良く歩きたくなる」まちなかづくりを、**人口減少・少子高齢化の進行**を背景に進めており、ウォーカブル政策の対象として**高齢者を含む多様な人々**を明示しています。つまり、日本の政策文脈でも walkability は高齢社会対応の重要テーマとして位置づけられています。 ([国土交通省](https://www.mlit.go.jp/toshi/toshi_machi_tk_000072.html))

さらに、内閣府の高齢社会白書では、**65歳以上の85.5%が「よく外出する」「たまに外出する」**と回答しており、外出時の手段として**徒歩は56.4%**と高い割合を占めています。日本の高齢者にとって、徒歩は特殊な行動ではなく、日常移動の中核の一つです。だから walkability が重要になる、という流れはかなり強く支えられます。 ([内閣府](https://www8.cao.go.jp/kourei/whitepaper/w-2022/zenbun/04pdf_index.html))

---

### 2. “For older adults, the ease of walking is not determined only by distance or route connectivity, but also by the physical burden imposed by slopes.”

ここも日本の研究で支えられます。

まず、都市計画研究では、**斜面住宅地では勾配を考慮すると徒歩移動の抵抗や換算距離が変わる**ことが示されています。溝口・山川（2001）は、日本の斜面住宅地を対象に、都市部への人口集中や地形制約のために丘陵地にも高密な住宅地が形成されてきたことを前提に、**勾配を加味した徒歩移動の評価**を行っています。要するに、同じ距離でも、坂があると“近い＝歩きやすい”とは言えない、ということです。 ([J-STAGE](https://www.jstage.jst.go.jp/article/journalcpij/36/0/36_841/_pdf))

また、縄田ら（2017）は、高齢社会における公共施設配置の研究で、**高齢者が歩行可能な徒歩圏を求める際に、地形距離だけでなく傾斜負荷と高齢者の歩行負担を考慮すべき**だとしています。この研究では、傾斜の激しい地域ではもちろん、**比較的平坦な場所でも高齢者の歩行負担を考慮すると徒歩圏が従来より小さくなる**ことが示されています。これは、older adults の walkability が距離やネットワーク接続性だけでは決まらず、勾配による身体負荷に左右される、という草稿の文をかなり直接に支えます。 ([J-STAGE](https://www.jstage.jst.go.jp/article/journalcpij/52/3/52_1150/_pdf))

川崎市の資料でも、麻生区の地域課題として、**「坂道が多く、高齢者が買い物やちょっとした外出をしにくい」**という認識が住民意見として示されています。これは学術論文ではありませんが、実際の地域感覚として、勾配が高齢者の外出のしやすさに影響していることを裏づけます。 ([川崎市](https://www.city.kawasaki.jp/350/cmsfiles/contents/0000151/151836/asaoku-2.pdf))

---

### 3. “This issue is especially relevant in Japanese suburban hill areas, where residential districts were developed on uneven terrain and where access to stations, shops, and services often involves substantial elevation change.”

この文も、日本の郊外住宅地研究と麻生区の地域資料でかなり支えられます。

まず、日本都市計画学会系の研究では、**日本では都市部への人口集中や地形制約のため、丘陵地などの斜面地に比較的高密度な住宅地が形成されてきた**ことが指摘されています。さらに、地方中核都市郊外の研究でも、**1970年代に丘陵を切り開いて造られた住宅地で高齢化が進んでいる**ことが示されています。つまり、「日本の郊外で、住宅地が起伏のある地形の上に展開してきた」という部分は、十分に一般化可能です。 ([J-STAGE](https://www.jstage.jst.go.jp/article/journalcpij/36/0/36_841/_pdf))

麻生区・新百合ヶ丘周辺については、さらに具体的です。

都市計画論文では、新百合ヶ丘駅周辺地域は**1974年の新百合ヶ丘駅開業以降に開発された住居系土地利用が中心**で、麻生区は**多摩丘陵の自然を残しつつ形成された地域**であり、**区全域が標高約50〜100m超の起伏に富んだ台地の高台**に位置すると記されています。これは、草稿の “developed on uneven terrain” をかなり直接に支えます。 ([cpij.or.jp](https://www.cpij.or.jp/com/ac/reports/19_418.pdf))

加えて、麻生区区民アンケートでは、麻生区のイメージとして**「坂道や階段が多い」**が**49.6%**で上位に挙がっています。住民の半数近くが、地域の特徴として坂や階段の多さを認識しているわけです。 ([川崎市](https://www.city.kawasaki.jp/asao/cmsfiles/contents/0000172/172925/houkokusyo.pdf))

「stations, shops, and services」についても、日本の高齢者の日常外出の目的として妥当です。内閣府調査では、高齢者がよく出かける目的として**近所のスーパーや商店での買物**、**通院**、**散歩**が上位にあり、東京圏の高齢者行動調査でも、**買い物**や**通院**が主要な外出目的で、外出先は**居住地域内が大半**とされています。さらに麻生区の区民アンケートでは、新百合ヶ丘駅の利用目的として**最寄り品の買い物**が最も多く、買い物やレジャーで行く場所として**新百合ヶ丘駅周辺**が突出しています。したがって、郊外丘陵地で「駅・買い物先・サービス施設へのアクセス」が生活上重要だという書き方は妥当です。 ([内閣府](https://www8.cao.go.jp/kourei/ishiki/h17_sougou/19html/2syou-11.html))

ただし、**“often involves substantial elevation change”** は少し強めです。

麻生区や斜面住宅地の文脈では十分もっともらしいですが、全国一律に直接測定した論文をここでは確認できていません。なので、査読に強くするなら、

> where access to stations, shops, and services often requires negotiating slopes and stairways

あるいは

> where access to daily destinations often involves non-negligible slope burdens

くらいにした方が安全です。これは、麻生区の「坂道や階段が多い」という住民認識や、高齢者の買い物・外出のしにくさという地域資料により素直に対応します。 ([川崎市](https://www.city.kawasaki.jp/asao/cmsfiles/contents/0000172/172925/houkokusyo.pdf))

---

### 段落を、日本向け根拠に寄せて少し固く書き直すなら

次の形の方が、根拠との対応がきれいです。

> In Japan, walkability has become an increasingly important issue in the context of rapid population aging. Older adults continue to go out frequently in daily life, and walking remains one of their major travel modes. In this context, the ease of walking is shaped not only by distance or network connectivity, but also by slope-related physical burden. This issue is particularly relevant in Japanese suburban hill areas, where many residential districts were developed on hilly terrain and where access to stations, shops, and everyday services often requires negotiating slopes and stairways.

この版の方が、日本の統計・日本の都市計画研究・麻生区の地域資料にきれいに乗ります。 ([内閣府](https://www8.cao.go.jp/kourei/whitepaper/w-2025/html/gaiyou/s1_1.html))

---

### 参考文献候補（日本フォーカス）

本文に入れやすい順に挙げます。

- 内閣府. 令和7年版高齢社会白書. 高齢化率29.3%、今後の高齢化見通し。 ([内閣府](https://www8.cao.go.jp/kourei/whitepaper/w-2025/html/gaiyou/s1_1.html))
- 総務省統計局. 統計からみた我が国の高齢者（2025）. 高齢化率29.4%、過去最高。 ([総務省統計局](https://www.stat.go.jp/data/topics/pdf/topics146.pdf))
- 国土交通省. 「居心地が良く歩きたくなる」まちなかづくり／ウォーカブルなまちづくり. 少子高齢化の中でのウォーカブル政策。 ([国土交通省](https://www.mlit.go.jp/toshi/toshi_machi_tk_000072.html))
- 溝口秀勝・山川仁（2001）「斜面住宅地における勾配を考慮した徒歩移動に関する研究」. 日本の斜面住宅地における勾配負荷の評価。 ([J-STAGE](https://www.jstage.jst.go.jp/article/journalcpij/36/0/36_841/_pdf))
- 縄田拓哉ほか（2017）「高齢社会における地形条件を考慮した公共施設整備のあり方」. 高齢者徒歩圏における傾斜負荷の重要性。 ([J-STAGE](https://www.jstage.jst.go.jp/article/journalcpij/52/3/52_1150/_pdf))
- 小瀧次郎ほか（2021頃の関連報告）「郊外住宅地居住者の主観的な交通利便性評価に影響を…」および関連する麻生区研究. 新百合ヶ丘周辺の起伏地形・住宅地形成。 ([cpij.or.jp](https://www.cpij.or.jp/com/ac/reports/19_418.pdf))
- 川崎市麻生区. 区民アンケート報告書. 「坂道や階段が多い」という地域認識。 ([川崎市](https://www.city.kawasaki.jp/asao/cmsfiles/contents/0000172/172925/houkokusyo.pdf))
- 川崎市麻生区. 地域意見資料. 「坂道が多く、高齢者が買い物やちょっとした外出をしにくい」。 ([川崎市](https://www.city.kawasaki.jp/350/cmsfiles/contents/0000151/151836/asaoku-2.pdf))
- 川崎市. 麻生区は男女ともに長寿日本一になりました！ 65歳以上の88.2%が「15分程度なら歩くし、歩ける」。 ([川崎市](https://www.city.kawasaki.jp/asao/page/0000151240.html))
