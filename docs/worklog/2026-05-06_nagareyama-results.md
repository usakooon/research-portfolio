# 2026-05-06 流山おおたかの森駅分析結果

> **Summary (EN):** Ran the same Integration × slope analysis for the second (flat) site, Nagareyama-Otakanomori Station, at 2.8 km/h and 2.5 km/h with 5% and 8% slope thresholds. At 5% / 2.8 km/h, 70.1% of the 44.3 km network is low-slope and 62.6% of high-integration streets are also low-slope. Loosening the threshold to 8% raises low-slope streets only from 70.1% to 72.6%. So on flat terrain the result hardly depends on the slope threshold, unlike the hilly site.
>
> **Phase:** 5. Second site & field survey (流山・現地調査) · **Tools/Data:** QGIS, Space Syntax (Integration), DEM-based slope, road network

[← 作業ログ一覧 / Work log index](README.md)

---

## 勾配5%+2.8km/h

<!-- image: download failed -->

<!-- image: download failed -->

<!-- image: download failed -->

<!-- image: download failed -->

| 区分 | 意味 | 地物数 | 延長 m | 延長 km | 全体比 |
| --- | --- | --- | --- | --- | --- |
| p1 | 高統合：Integration 上位20% | 262 | 8,735.965 | 8.735 | 19.720% |
| p2 | 低勾配：`grade_pct_final_mean <= 5.0` | 865 | 31,043.821 | 31.043 | 70.078% |
| p3 | 高統合 ∩ 低勾配 | 175 | 5,467.317 | 5.467 | 12.342% |
| all | 全道路 | 1,310 | 44,298.862 | 44.298 | 100.000% |

| 指標 | 値 |
| --- | --- |
| 高統合道路のうち低勾配でもある割合 | 62.587% |
| 低勾配道路のうち高統合でもある割合 | 17.612% |

## Less than Grade5%+2.5km/h

<!-- image: download failed -->

<!-- image: download failed -->

<!-- image: download failed -->

<!-- image: download failed -->

| 区分 | 意味 | 地物数 | 延長 m | 延長 km | 全体比 |
| --- | --- | --- | --- | --- | --- |
| p1 | 高統合：Integration 上位20% | 214 | 7,361.564 | 7.361 | 20.840% |
| p2 | 低勾配：`grade_pct_final_mean <= 5.0` | 696 | 24,186.385 | 24.186 | 68.471% |
| p3 | 高統合 ∩ 低勾配 | 139 | 4,580.260 | 4.580 | 12.966% |
| all | 全道路 | 1,072 | 35,323.370 | 35.323 | 100.000% |

| 指標 | 値 |
| --- | --- |
| 高統合道路のうち低勾配でもある割合 | 62.218% |
| 低勾配道路のうち高統合でもある割合 | 18.937% |

## Grade Less than 8.0%+2.8km/h

<!-- image: download failed -->

<!-- image: download failed -->

## Grade Less than 8.0%+2.5km/h

<!-- image: download failed -->

<!-- image: download failed -->

## 比較表

| 区分 | 意味 | 2.8km/h版 延長 km | 2.8km/h版 割合 | 2.5km/h版 延長 km | 2.5km/h版 割合 |
| --- | --- | --- | --- | --- | --- |
| p1 | 高統合：Integration 上位20% | 8.735 | 19.720% | 7.361 | 20.840% |
| p2 | 低勾配：`grade_pct_final_mean <= 5.0` | 31.043 | 70.078% | 24.186 | 68.471% |
| p3 | 高統合 ∩ 低勾配 | 5.467 | 12.341% | 4.580 | 12.966% |
| all | 全道路 | 44.298 | 100.000% | 35.323 | 100.000% |

| 指標 | 意味 | 2.8km/h版 | 2.5km/h版 |
| --- | --- | --- | --- |
| p3 / p1 | 高統合道路のうち低勾配な割合 | 62.584% | 62.218% |
| p3 / p2 | 低勾配道路のうち高統合な割合 | 17.611% | 18.937% |

## 2.8km/h版：勾配5.0%以下と8.0%以下の比較

| 区分 | 意味 | 勾配5.0%以下 延長 km | 勾配5.0%以下 割合 | 勾配8.0%以下 延長 km | 勾配8.0%以下 割合 |
| --- | --- | --- | --- | --- | --- |
| p1 | 高統合：Integration 上位20% | 8.735 | 19.720% | 8.735 | 19.720% |
| p2 | 低勾配 | 31.043 | 70.078% | 32.158 | 72.594% |
| p3 | 高統合 ∩ 低勾配 | 5.467 | 12.341% | 5.759 | 13.000% |
| all | 全道路 | 44.298 | 100.000% | 44.298 | 100.000% |
