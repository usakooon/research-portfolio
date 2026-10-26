# 作業ログ / Research Work Log（2026年2月〜5月）

本研究（[Terado, 2026](https://doi.org/10.5194/isprs-archives-L-4-W1-2026-311-2026)）の分析期間中に Notion に記録していた作業ログを、Markdown に変換して公開したものです。
各ログの冒頭には英語の要約（Summary）を付けています。本文は当時の記録をほぼそのまま残しており、軽い整形と誤字の修正だけを行いました。

This folder contains the day-by-day research notes kept while working on the paper, from February to May 2026.
Each log opens with a short English summary; the body is the original Japanese note, lightly cleaned up.
The log shows the trial and error behind the final method, including parameters that were later revised.
For the reasons behind each choice, see [decisions.md](../decisions.md).

> [!NOTE]
> ログ内の数値は**作業途中のもの**で、論文の最終値とは異なる場合があります（歩行速度・中心性の閾値・比較駅などは途中で変更しています）。最終的な結果は論文を参照してください。
>
> Figures in these logs are intermediate and may differ from the published paper.

## フェーズ / Phases

| # | フェーズ | Phase | 期間 |
|---|---|---|---|
| 1 | 駅選定 | Site selection | 2月中旬 |
| 2 | 道路・勾配データ整備 | Network & slope data preparation | 2月下旬〜4月上旬 |
| 3 | 分析・初期結果（新百合ヶ丘 × 田奈） | Analysis & first results | 3月上旬 |
| 4 | 手法見直し・学会準備 | Method revision & conference prep | 3月中旬〜4月 |
| 5 | 流山おおたかの森の分析・現地調査 | Second site & field survey | 5月 |

## 年表 / Timeline

### 2026年2月 — 対象駅の選定とデータ整備

| 日付 | ログ | 内容 | Phase |
|---|---|---|---|
| 02-16 | [留意点：平均勾配の過小評価と橋・立体交差](2026-02-16_slope-pitfalls.md) | DEMから勾配を出すときの2つの落とし穴と対策 | 2 |
| 02-18 | [青葉区・麻生区の駅別平均勾配](2026-02-18_station-mean-slope.md) | 駅1km圏の平均・最大勾配をZonal Statisticsで算出しランキング化 | 1 |
| 02-19 | [駅1km圏の高齢化率](2026-02-19_aging-rate-1km.md) | 町丁別年齢人口を面積按分して65歳以上割合を推計。比較駅を田奈に | 1 |
| 02-21 | [比較駅・歩行速度・15分徒歩圏・道路データ準備](2026-02-21_station-speed-isochrone-roads.md) | 歩行速度1.0 m/s を仮採用、QNEAT3で15分圏、OSM道路の除外条件 | 2 |
| 02-25 | [道路整備手順](2026-02-25_road-preparation.md) | スナップ・ジオメトリ修正・孤立線除去の第1版 | 2 |
| 02-26 | [道路整備手順 ver.2](2026-02-26_road-preparation-v2.md) | 地上／高架を分けた整備手順に改訂、新百合ヶ丘で試算 | 2 |

### 2026年3月 — 初期分析と手法の見直し

| 日付 | ログ | 内容 | Phase |
|---|---|---|---|
| 03-05 | [道路データにslopeを付与](2026-03-05_slope-to-roads.md) | 10 m分割 × DEMで縦断勾配を算出、30%超はNULL | 3 |
| 03-07 | [道路の延長調査](2026-03-07a_road-length-survey.md) | 指標を本数から延長(km)に変更、p1/p2/p3を定義 | 3 |
| 03-07 | [田奈駅で実装](2026-03-07b_tana-implementation.md) | 同じ手順を比較駅に適用 | 3 |
| 03-10 | [結果まとめ](2026-03-10_results-summary.md) | 新百合ヶ丘 × 田奈の比較：中心道路の約9割は低勾配、逆は約1/4 | 3 |
| 03-12 | [今後の発展に向けて](2026-03-12_future-directions.md) | 先行研究と比べた新規性の整理、Mapillary検証の採用 | 4 |
| 03-14 | [勾配考慮型ネットワーク分析](2026-03-14_slope-aware-network.md) | Toblerの関数で勾配を時間コスト化する最小案（保留） | 4 |
| 03-15 | [Abstract（日本語）](2026-03-15_abstract-ja.md) | 学会抄録の初稿 | 4 |
| 03-16 | [Abstract（英語）](2026-03-16_abstract-en.md) | 英語版抄録、文献で裏付けを確認しながら推敲 | 4 |
| 03-24 | [採用ネットワーク](2026-03-24_network-selection.md) | 分析に含める歩行リンク（デッキ・階段・歩道など）の定義 | 2 |

### 2026年4月 — 前提条件の改訂と学会準備

| 日付 | ログ | 内容 | Phase |
|---|---|---|---|
| 04-01 | [歩行速度変更](2026-04-01_walking-speed-change.md) | 1.0 m/s → 47.0 m/min（主）・41.8 m/min（補助）に変更 | 4 |
| 04-02 | [道路データ整備](2026-04-02_road-data-update.md) | 採用ネットワークの定義をQGIS手順に落とし込み | 2 |
| 04-03 | [slope付与・延長調査・Result・学会に向けて](2026-04-03_slope-length-results.md) | R705・勾配5%で再計算、学会ごとの方針 | 4 |
| 04-04 | [ワークフロー（分析＋Result）](2026-04-04a_workflow.md) | 全工程を1ページに統合、速度・閾値の感度比較 | 4 |
| 04-04 | [Result・学会方針](2026-04-04b_results-conference-plan.md) | 閾値5%/8%で結果が大きく変わることを確認 | 4 |
| 04-04 | [今後のスケジュール](2026-04-04c_schedule.md) | 4〜8月の計画（FOSS4G論文を優先） | 4 |
| 04-08 | [既存研究レビュー表](2026-04-08_literature-review.md) | Space Syntax・アクセシビリティ研究6本の整理と研究ギャップ | 4 |

### 2026年5月 — 流山おおたかの森の分析と現地調査

| 日付 | ログ | 内容 | Phase |
|---|---|---|---|
| 05-06 | [流山おおたかの森駅 分析結果](2026-05-06_nagareyama-results.md) | 平坦地では勾配閾値による差がほとんど出ないことを確認 | 5 |
| 05-08 | [研究詰め](2026-05-08_research-refinement.md) | NAIN/NACHの採用、調査ルートA/Bの選定、調査設計 | 5 |
| 05-10 | [調査準備](2026-05-10_survey-preparation.md) | ルート確定、当日の段取り、調査票 | 5 |
| 05-11 | [調査結果](2026-05-11_survey-results.md) | 両駅のヒアリング・歩行者カウント結果 | 5 |
| 05-12 | [流山おおたかの森駅（ルート・Mapillary撮影）](2026-05-12_nagareyama-otakanomori.md) | 流山のルート設定、Mapillaryの空白区間を撮影 | 5 |

### 日付なし

- [メモ / Notes](memo.md)：初期の設計の簡略化方針、Choiceを主指標にしない理由の文献整理

## 公開にあたっての処理 / Editing notes

- Notion 独自の書式を GitHub Markdown に変換しました。
- URL のトラッキング用パラメータを削除しました。
- 第三者の論文PDFなどの添付ファイルは再配布せず、出典の記載だけ残しました。
- ヒアリング回答者を特定できる情報は掲載していません。調査員の氏名は「調査員1／調査員2」に置き換えました。
- ログ内の画像は、当時のQGISなどの作業画面のキャプチャです。表示を軽くするため、縮小・減色しています。
- 背景地図の出典：© OpenStreetMap contributors（ODbL）、国土地理院（地理院地図）。
- Google ストリートビュー／Google Earth の画面と、第三者の論文の図表は掲載していません（該当箇所にその旨を記載）。
- 官公庁の公開資料（国土交通省のガイドラインなど）は、出典を明記したうえで一部を引用・掲載しています。
