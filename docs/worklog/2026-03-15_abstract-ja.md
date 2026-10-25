# 2026-03-15 Abstract

> **Summary (EN):** Wrote the Japanese conference abstract for the Shin-Yurigaoka case study (gradient-aware network analysis: OSM + 5 m DEM, 10 m segments, ASA R900 Integration, 15-minute catchment at 1.0 m/s). Key numbers: high-integration roads 23.9%, low-gradient (≤8.0%) roads 84.7%, both 21.6%; 90.8% of central roads are low-gradient but only 25.6% of low-gradient roads are central. A structured outline of the abstract and a machine-translated (DeepL) English version were kept on the same page.
>
> **Phase:** 4. Method revision & conference prep (手法見直し・学会準備) · **Tools/Data:** QGIS, DepthmapX, Space Syntax Toolkit, OpenStreetMap, DEM 5m, DeepL

[← 作業ログ一覧 / Work log index](README.md)

---

## 勾配考慮型ネットワーク分析を用いた高齢者にとっての歩きやすさと実空間のギャップ解析ー新百合ヶ丘駅周辺を事例に

本研究は、丘陵地に立地する新百合ヶ丘駅周辺を対象に、高齢者にとっての歩きやすさを街路ネットワーク全体から評価することを目的とする。対象地である川崎市麻生区は、令和2年市区町村別生命表で男女ともに平均寿命全国1位となった長寿地域であり、さらに市の調査では高齢者の多くが「15分くらい続けて歩ける、歩いている」と回答している。こうした背景から、坂の多い丘陵地において、高齢者の歩行と街路空間の関係を捉える事例として新百合ヶ丘駅周辺を取り上げる。

分析では、OpenStreetMap の道路データと 5m DEM を用い、高齢者の想定歩行速度 1.0m/s に基づく徒歩15分圏を対象範囲とした。QGIS 上で道路を10m単位に分割し、始点終点の標高差から道路縦断勾配を算出したうえで、DepthmapX および Space Syntax Toolkit により求めた Angular Segment Analysis（R900）の Integration 値を各道路セグメントに付与した。これにより、ネットワーク上の中心性と勾配条件を統合した「勾配考慮型ネットワーク分析」を行った。

その結果、新百合ヶ丘駅周辺では、高統合道路（Integration 上位20%）が 23.9%、低勾配道路（8.0%以下）が 84.7%、両条件を同時に満たす道路は 21.6% であった。高統合道路のうち 90.8% が低勾配条件を満たしており、ネットワーク上で中心的な道路の多くは比較的歩きやすい勾配条件にあることが示された。一方で、低勾配道路のうち高統合道路が占める割合は 25.6% にとどまり、勾配が緩やかな道路が広く存在していても、それらがそのまま街路ネットワークの中心を形成するわけではないことが明らかになった。

以上より、歩きやすさは、勾配条件だけでなく、その道路が街路ネットワークの中でどれだけ主要な役割を担っているかとあわせて評価する必要がある。今後は、OSMおよびQGIS上で道路データをさらに実際の歩道に近づける形で整備し、30分徒歩圏への拡張や実空間との照合を通じて、勾配考慮型ネットワーク分析の妥当性と応用可能性を検討する。

## Abstractまとめ

タイトル：勾配考慮型ネットワーク分析を用いた高齢者にとっての歩きやすさと実空間のギャップ解析ー新百合ヶ丘駅周辺を事例に

対象：高齢者(歩行速度1.0m/s)

範囲：新百合ヶ丘周辺の徒歩15分圏

勾配考慮型ネットワーク分析：Space Syntax,Angular Segment Analysis(R900),DepthmapX, Space Syntax Toolkit) ×勾配

背景：本研究の対象地域は、丘陵地につくられた麻生区（新百合ヶ丘駅が本研究での代表駅）を選択した。その背景として、麻生区は2023年に平均寿命を男女ともに全国一位を獲得した長寿の地域（厚生労働省による令和２年市区町村別生命表を公表）。坂道が多い丘陵地に住む麻生区の高齢者は意識的に歩く人が多い（2023年川崎市によるアンケート結果から約88％以上の人が15分なら歩けるし歩いていると回答）。そのような背景から川崎市北部の副都心として機能する区内の新百合ヶ丘駅を本研究の対象場所とする。そして、丘陵地にできた都市という特徴から、範囲を限定した街路ネットワーク分析から中心となる道路を定量化し、勾配を掛け合わせて算出することで街路ネットワーク全体の空間構造を理解する。

Integrationが歩行者流動と関係することは、古典的に “Natural Movement” などで強く議論されており、Integration/Choice が人の移動と関係するという主張はある。一方で、歩きやすさの主流定義・実証は、目的地近接（destinations）、土地利用混在、接続性に加え、安全・快適・歩行基盤まで含めて説明する枠組みがある。

データ：OpenStreetMapのhighway(駐車場、私道、屋内道路除外)、DEM(５ｍ)

目的：勾配考慮型ネットワーク分析から歩行のしやすさを定量化し、解析結果を実空間で検証し、ギャップを解析する。そして街路ネットワーク全体から歩きやすさを評価する。

方法：QGIS上で道路を10m単位に分割して始点終点の標高差から道路縦断勾配を算出し、交差点で分割した道路セグメント(integration値含む)に勾配値を付与した。

結果：新百合ヶ丘駅周辺では高統合道路(Integration上位20％)は23.9%、低勾配道路（≤8.0% :国土交通省の歩道基準より）は84.7%、両条件を満たす道路は21.6%であった。高統合道路に占める低勾配道路の割合は90.8%で高統合道路の大部分が低勾配条件を満たしていた。一方で、低勾配道路のうち高統合道路が占める割合は25.6%で低勾配道路が広く存在していても、それらがそのまま道路ネットワークの中心を形成するわけではない

今後：実際の歩道に寄せるためにOSMやQGISの道路整備を行う、30分徒歩圏で同様の分析を行い違いをみる、

解析結果の検証（Mapillary）

中心×勾配（＝歩きやすさ）

統合20％（高）×低勾配（8％以下or5％以下）⇒想定通りの歩きやすい道か？

統合20~40%（高）×低勾配（8％以下or5％以下）⇒ポテンシャルがある道か？

統合40~60（低）×低勾配（8％以下or5％以下）⇒代替ルート？どんな特徴をもつ？

統合60%以下（低）× 高勾配(8%以上)⇒範囲を広げたときに使うといいかも

---

### Analysis of the Gap Between Walkability for Older Adults and the Actual Physical Environment Using Gradient-Aware Network Analysis: A Case Study of the Area Surrounding Shin-Yurigaoka Station

This study aims to evaluate walkability for older adults in the area surrounding Shin-Yurigaoka Station, which is located in a hilly region, by analyzing the entire street network. The study area, Asao Ward in Kawasaki City, is a region known for longevity, ranking first nationwide in average life expectancy for both men and women according to the 2020 Municipal Life Tables. Furthermore, a city survey found that many older adults reported being able to walk continuously for about 15 minutes or doing so regularly. Against this backdrop, the area around Shin-Yurigaoka Station is selected as a case study to examine the relationship between older adults’ walking patterns and street space in a hilly area with many slopes.

For the analysis, we used OpenStreetMap road data and a 5-meter DEM, defining the study area as the 15-minute walking radius based on an assumed walking speed of 1.0 m/s for the elderly. In QGIS, roads were divided into 10-meter segments. After calculating the longitudinal gradient of each segment based on the elevation difference between its start and end points, the Integration value from the Angular Segment Analysis (R900)—calculated using DepthmapX and the Space Syntax Toolkit—was assigned to each road segment. This enabled a “gradient-considered network analysis” that integrated centrality within the network with gradient conditions.

The results showed that in the area surrounding Shin-Yurigaoka Station, high-integration roads (top 20% by integration) accounted for 23.9%, low-gradient roads (8.0% or less) accounted for 84.7%, and roads meeting both conditions simultaneously accounted for 21.6%. It was shown that 90.8% of high-integration roads met the low-gradient condition, indicating that many of the central roads in the network have relatively walkable gradient conditions. On the other hand, high-integration roads accounted for only 25.6% of low-gradient roads, revealing that even though roads with gentle gradients are widespread, they do not necessarily form the core of the street network.

Based on the above, walkability must be evaluated not only in terms of gradient conditions but also in relation to the extent to which a road plays a major role within the street network. Moving forward, we will refine road data in OSM and QGIS to better align it with actual sidewalks. Through expansion to the 30-minute walking zone and comparison with the physical environment, we will examine the validity and applicability of gradient-considered network analysis.

*Translated with [DeepL.com](https://www.deepl.com/) (free version)*
