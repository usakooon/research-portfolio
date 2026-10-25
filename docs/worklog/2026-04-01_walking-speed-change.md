# 2026-04-01 歩行速度変更

> **Summary (EN):** Replaced the earlier 1.0 m/s walking-speed assumption with age-specific speeds: 47.0 m/min (≈2.8 km/h, ages 60–74) as the main setting and 41.8 m/min (≈2.5 km/h, 75+) as an auxiliary setting, both over 15 minutes of network distance. The resulting 5/10/15-minute distances (235/470/705 m main; 209/418/627 m auxiliary) were used as the ranges for extracting OSM highway data.
>
> **Phase:** 4. Method revision & conference prep (手法見直し・学会準備) · **Tools/Data:** OpenStreetMap highway, QGIS network distance

[← 作業ログ一覧 / Work log index](README.md)

---

**主設定**

- 距離: ネットワーク距離
- 歩行速度: 47.0 m/分(60歳‐74歳)⇒2.8km/h
- 時間: 15分
- 235,470,705m範囲でhighway抽出

**補助設定**

- 41.8 m/分(75歳以上)⇒2.5km/h
- 15分
- 209, 418, 627, n
