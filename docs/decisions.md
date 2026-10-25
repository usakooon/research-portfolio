# 判断の記録 / Decision Record

分析の主な前提条件を、いつ・なぜ決め、どう変えたかをまとめたものです。根拠は各日の[作業ログ](worklog/README.md)にあります。

This page lists the key analytical choices: what was decided, when, why, and how each one changed before the final paper.

---

## 1. 対象駅 / Study areas

| 時期 | 判断 | 理由 | ログ |
|---|---|---|---|
| 2月18〜19日 | 丘陵地の対象として**新百合ヶ丘駅**を選んだ | 麻生区の駅の中では、駅1km圏の平均勾配が13.90%と中程度で、高齢化率は20.53% | [02-18](worklog/2026-02-18_station-mean-slope.md), [02-19](worklog/2026-02-19_aging-rate-1km.md) |
| 2月19日 | 比較駅を**田奈駅**とし、江田を予備、市が尾を追加候補とした | 高齢化率が新百合ヶ丘と近く（21.95%）、平坦寄り（9.77%）。年齢構成の差を抑え、地形の差を見やすくするため | [02-19](worklog/2026-02-19_aging-rate-1km.md) |
| 5月 | 比較駅を**流山おおたかの森駅**に変更 | 新百合ヶ丘の勾配の激しい地形と対照的な場所として、平坦で比較的新しい街（駅前の計画市街地）を選んだ。地形条件と市街地の形成過程がどちらも異なる駅を比べるため（論文 Introduction: “differs in its topographical condition and urban development process”） | [05-06](worklog/2026-05-06_nagareyama-results.md), 論文 1章 |

## 2. 高齢者の歩行速度と徒歩圏 / Walking speed and catchment

| 時期 | 判断 | 理由 | ログ |
|---|---|---|---|
| 2月21日 | **1.0 m/s**（3.6 km/h）を仮採用 | 歩行者信号の青時間の設計に使われる速度。平均値ではなく基準値として扱った。斉藤・有薗（1986）の高齢者の値（約1.06 m/s）、FHWAの0.9 m/sも参照 | [02-21](worklog/2026-02-21_station-speed-isochrone-roads.md) |
| 4月1日 | **47.0 m/min**（約2.8 km/h）を主、**41.8 m/min**（約2.5 km/h、75歳以上）を補助に変更 | 年齢階級別の歩行速度を使うため | [04-01](worklog/2026-04-01_walking-speed-change.md) |
| 4月〜 | 15分徒歩圏 = **705 m**（補助は627 m）。Space Syntaxの解析半径も R705／R627 に合わせた | 徒歩圏と解析半径の前提をそろえるため（当初はR900） | [04-03](worklog/2026-04-03_slope-length-results.md), [05-08](worklog/2026-05-08_research-refinement.md) |

## 3. 道路ネットワーク / Pedestrian network

| 時期 | 判断 | 理由 | ログ |
|---|---|---|---|
| 2月21日 | motorway・private・indoor・parking_aisle を除外。trunk・primary・secondary は残した | 高齢者が実際に歩ける道に限るため。幹線道路は歩道がある限り残した | [02-21](worklog/2026-02-21_station-speed-isochrone-roads.md) |
| 2月21日 | 2 km範囲で計算し、最後に15分圏で切り出す | 15分徒歩圏より広い範囲でDepthmapXに読み込む方針とした（範囲の端の影響を抑えるため） | [02-21](worklog/2026-02-21_station-speed-isochrone-roads.md) |
| 2月25〜26日 | スナップ0.5 m、Douglas-Peucker簡略化 1 m（高架は0.5 m）。手作業の修正は最小限 | 修正しすぎるとネットワークの接続が壊れるため | [02-25](worklog/2026-02-25_road-preparation.md), [02-26](worklog/2026-02-26_road-preparation-v2.md) |
| 3月24日 | 歩行者デッキ・階段・地下道・主要な歩道を分析に含めた | 駅周辺の実際の歩行経路を表すため | [03-24](worklog/2026-03-24_network-selection.md) |

## 4. 勾配 / Slope

| 時期 | 判断 | 理由 | ログ |
|---|---|---|---|
| 2月16日 | 長い道路の平均値ではなく、**10 m分割**で勾配を出した | 平均すると短い急坂が見えなくなる（過小評価）ため | [02-16](worklog/2026-02-16_slope-pitfalls.md), [03-05](worklog/2026-03-05_slope-to-roads.md) |
| 3月5日 | **30%を超える勾配はNULL**（外れ値）とした | DEMは地表面なので、橋や立体交差で異常な値が出るため | [03-05](worklog/2026-03-05_slope-to-roads.md) |
| 3月〜4月 | 閾値を**8% → 5%（主）＋8%（補助）**に変更 | 国交省の歩道の縦断勾配基準（5%、やむを得ない場合8%）に合わせた。高齢者には厳しめの基準を主にした | [04-03](worklog/2026-04-03_slope-length-results.md), [04-04b](worklog/2026-04-04b_results-conference-plan.md) |
| 4月4日 | 階段・デッキはDEMの勾配評価から外し、現地調査で確認 | DEMでは立体的な構造を表せないため（研究の限界として明記） | [04-04a](worklog/2026-04-04a_workflow.md) |
| 3月14日 | 勾配を移動時間のコストにする手法（Tobler）は**保留** | 締切に対して実装が重いため。今後の課題とした | [03-14](worklog/2026-03-14_slope-aware-network.md), [03-12](worklog/2026-03-12_future-directions.md) |

## 5. 中心性指標 / Centrality

| 時期 | 判断 | 理由 | ログ |
|---|---|---|---|
| 2月〜4月 | Integration（to-movement）を主指標にした | 先行研究で歩行量との相関が比較的高いため（メモの文献整理） | [memo](worklog/memo.md) |
| 3月〜4月 | 「高中心性」= 各半径の**上位20%** | 駅どうしを同じ割合で比較できるため | [04-03](worklog/2026-04-03_slope-length-results.md) |
| 5月8日 | **NAIN と NACH**（正規化Integration・Choice）の両方を使う形に変更し、上位10/20/30%で感度分析 | 中心へ向かう動き（to-movement）と通り抜け（through-movement）の両方を見るため | [05-08](worklog/2026-05-08_research-refinement.md) |
| 5月以降（論文執筆時） | 高中心性の定義を「上位○%」から「**平均＋1SD以上**」に変更した | 上位10／20／30%のような割合で区切る方法は、統計的な根拠が弱いと指導教員から指摘を受けたため。値の分布（平均と標準偏差）にもとづく基準にした | 論文 3章 |

## 6. 現地調査 / Field survey

| 時期 | 判断 | 理由 | ログ |
|---|---|---|---|
| 3月12日 | Mapillaryで中心性 × 勾配の分類を検証することにした。GPS軌跡による検証は保留 | 高齢の協力者を集めにくいため | [03-12](worklog/2026-03-12_future-directions.md) |
| 5月8〜10日 | 長さが同程度で性格の違うルートA（歩行者空間）とB（車道沿いのアクセス路）を比較。ルートCは保留 | 条件をそろえて、歩行環境の違いだけを比べるため | [05-08](worklog/2026-05-08_research-refinement.md), [05-10](worklog/2026-05-10_survey-preparation.md) |
| 5月12日 | Mapillaryの撮影区間は、NAIN/NACH上位30%の道のうち既存画像がない（または夜間だけの）区間に絞った | 撮影の手間を、検証に必要な区間に集中させるため | [05-12](worklog/2026-05-12_nagareyama-otakanomori.md) |
