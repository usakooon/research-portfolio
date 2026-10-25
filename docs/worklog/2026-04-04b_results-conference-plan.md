# 2026-04-04 Result、学会方針

> **Summary (EN):** Measured how much the high-integration streets (top 20% Integration) and the low-slope streets overlap inside the walking-area network. This was done for two elderly walking speeds (2.5 km/h and 2.8 km/h) and two slope thresholds (5% and 8%). At the 5% threshold about 67% of high-integration streets are also low-slope. At 8% this rises to 86.8%, so the threshold choice strongly shapes the "central street vs. walkable street" gap. Conference plan decided: FOSS4G centres on Integration with 5% as the main threshold (8% supplementary), and the City Planning Institute paper adds Choice and improvement-priority proposals.
>
> **Phase:** 4. Method revision & conference prep (手法見直し・学会準備) · **Tools/Data:** QGIS, Space Syntax (Integration / Choice), DEM-based slope, road network, Mapillary

[← 作業ログ一覧 / Work log index](README.md)

---

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
| p2 | 低勾配（`grade_pct_final_mean <= 8.0`） | 16.620 | 55.700% |
| p3 | 高統合 ∩ 低勾配 | 4.193 | 14.055% |
| all | 全道路 | 29.838 | 100.000% |

## 勾配5.0%以下＋2.5km/hと2.8km/hの比較表

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

## 勾配8.0%以下＋2.8km/h

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

## 2.8km/h 条件における勾配閾値比較

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

## 限界

本研究では、道路ネットワーク上の地表歩行空間を対象として、DEM に基づく地形由来の縦断勾配を評価した。一方、階段やペデストリアンデッキ等の立体歩行施設については、DEM による勾配表現が適切でなく、また本研究の分析手法では十分に扱うことが難しいため、本分析では勾配評価の対象外とした。これらの要素については、実地調査および地図上で位置を確認し、歩行評価の際に補足的に検討する。

## 今後の学会方針

### FOSS4G

- R705とR627 (65〜74歳と75歳以上でグループ分け)
- Integration 主体
- 勾配⇒5.0%が主体＋8.0%(2.8km/hだけ)は補助
- Mapillary / 現地確認⇒階段、現地の歩道状況を確認
- **評価・ギャップ可視化まで**

### 都市計画学会

- R705 と R627
- Integration + Choice
- 勾配⇒5.0%だけ
- Choice: 簡易現地観察(できれば)
- **改善優先地点の提案まで**
