# 2026-03-24 採用ネットワーク

> **Summary (EN):** Defined which links make up the analysis network: road centrelines as the base, plus footbridges, underpasses, stairs, the station pedestrian deck, clear pedestrian shortcuts and main sidewalks along arterial roads. Sidewalks are kept as separate lines only on major roads (arterial / 2+ lanes); elsewhere they are absorbed into the road network at intersections. Rotary-internal loops, platform-level local paths and bus/service lanes are excluded or reserved for auxiliary analysis.
>
> **Phase:** 2. Network & slope data preparation (道路・勾配データ整備) · **Tools/Data:** OpenStreetMap road network, QGIS

[← 作業ログ一覧 / Work log index](README.md)

---

**採用ネットワーク**

- ベース：車道中心線
- 追加：歩道橋、地下道、階段、駅前デッキ、明確な歩行者ショートカット、大通り沿いの主要歩道

### ペデストリアンデッキ

A. 主解析に入れる

- デッキ上の主通路
- デッキから駅へ入る通路
- デッキから街へ出る通路
- デッキと地上を結ぶ階段・EV・スロープ
- 地上の外周歩道
- 横断歩道
- バス停へ行く最低限のアクセス歩道
- 幹線道路、一般道路（2車線以上）の歩道は残し、それ以外は車道を採用

  **主要道路**

  → 歩道を別線で残す

  **それ以外**

  → 歩道は基本削除して、**交差点で車道ネットワークに吸収**

B. 補助解析では削ってよい

- ロータリー内部をぐるぐる回るだけの地上歩道
- 乗降場内の局所動線
- 通過性が低く、外部街路への寄与が小さい短リンク

C. そもそも入れない

- バス車道
- 一般車のロータリー車路
- サービス道路で歩行不可のもの
