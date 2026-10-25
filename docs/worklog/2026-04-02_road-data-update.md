# 2026-04-02 道路データ整備

> **Summary (EN):** Turned the network-selection rules into a concrete QGIS cleaning workflow: keep separate sidewalks only on `highway=primary/tertiary`, split OSM data into ground, elevated (layer≥1 / bridge / tunnel) and steps layers, then delete duplicates, split at intersections, snap at 0.5 m, simplify with Douglas-Peucker at 1.0 m, merge and repair geometries. Space Syntax Toolkit tools (Road Centre Line Cleaner, Graph Analysis, Attributes Explorer) were listed for the centrality step, and cleaned networks were produced for the 705 m and 470 m ranges.
>
> **Phase:** 2. Network & slope data preparation (道路・勾配データ整備) · **Tools/Data:** QGIS processing tools, Space Syntax Toolkit, OpenStreetMap

[← 作業ログ一覧 / Work log index](README.md)

---

A. 主解析に入れる

- デッキ上の主通路
- デッキから駅へ入る通路
- デッキから街へ出る通路
- デッキと地上を結ぶ階段・EV・スロープ
- 地上の外周歩道
- 横断歩道
- バス停へ行く最低限のアクセス歩道
- 幹線道路、一般道路（2車線以上）の歩道は残し、それ以外は車道を採用

  ```javascript
  "highway"='tertiary' or "highway"='primary'
  ```

  **主要道路**→ 歩道を別線で残す

  - 主要道路と普通の道路を接続する短いラインはSplit with linesで削除

  **それ以外**

  → 歩道は基本削除して、**交差点で車道ネットワークに吸収**

## Space Syntax toolkit 使用ツール

**道路を整える：Road Centre Line Cleaner**

**中心性を計算する：Graph Analysis**

**結果を読む：Attributes Explorer**

### 2. QGIS標準機能で代用する場合（推奨ワークフロー）

駅前の立体交差（デッキ）を正確に扱うために、以下の手順で行うのが最も確実です。

### ステップA：データの仕分け（OSM属性の活用）

OSMから抽出したデータを2つのレイヤに分けます。

1. **A：地上レイヤ:** **`layer=0`** またはタグなし（普通の道路・歩道）
2. **B：立体レイヤ:** **`layer=1`** 以上、**`bridge=yes`**、**`tunnel=yes`**（ペデストリアンデッキ、地下道）
3. C：footway=steps

### ステップB：地上レイヤのトポロジークリーニング

Aのレイヤを使用

1. **重複の削除:** プロセシング ＞ 「重複ジオメトリを削除 (Delete duplicate geometries)」
2. **交差点での分割:** プロセシング ＞ **「split with lines」** (ツールボックス内)。
3. **スナップ処理:** プロセシング ＞ 「ジオメトリのスナップ (Snap geometries to layer)」。
   - 自分自身のレイヤを指定し、許容値を **`0.5m`** にして実行。

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

## Result

### 705m

<!-- image: download failed -->

## 470m

<!-- image: download failed -->
