# 2026-02-26 道路整備手順ver.2、新百合ヶ丘 Result

> **Summary (EN):** Revised the road-preparation workflow into two phases: (1) whole-area processing — attribute filtering, duplicate removal, Fix Geometries, splitting at shared vertices only for ground-level lines (layer A), Douglas-Peucker simplification (1 m for ground lines, 0.5 m for bridges/elevated lines), keeping steps separately, then merging and snapping at 0.5 m; (2) manual fixes within ~1–1.5 km of the station limited to roundabouts, staggered junctions, squares and parallel lanes, following a road-centre-line simplification paper. Space Syntax Toolkit and depthmapX gave no notable differences, so this procedure was also applied to Tana; for Shin-Yurigaoka the top-20% INTr900m threshold was 367.62 (1,455 of 7,274 segments).
>
> **Phase:** 2. Network & slope data preparation (道路・勾配データ整備) · **Tools/Data:** QGIS, Space Syntax Toolkit, depthmapX, OSM roads

[← 作業ログ一覧 / Work log index](README.md)

---

## フェーズ1：必須処理（全体に適用）

これは全域にやる。ここは省略不可。

### ① フィルタリング

- highwayで不要道路削除（高速道路など）
- footwayやpathは研究目的次第で除外
- 追加の道路があれば追加

1. highway_2km (asao)
   - 新百合ヶ丘の場合、parking_aisle、"indoor" = 'yes'、"access" = 'private'　に該当する道路を削除
   - 田奈駅の場合、新ユリのverと該当するものがなかった
   - SS Toolkit：orphan, island削除

### ② ジオメトリクリーン

- 重複削除：「重複ジオメトリを削除」
- 無効ジオメトリ修正 : 「ジオメトリを修復」

1. road_clean2

### ③ トポロジー整理

論文p.13のSequence 1–7参照
Road-centre-line-simplification…

- 並行道路はできるだけ一本のlineに修正
- 共有頂点で分割：「線で分割」
  - レイヤA（地上）: **`layer`** 0（または空）、かつ **`bridge`** や **`tunnel`** を持たない線。
    - **レイヤAの処理:** 「線で分割」 実行後 ➔ 「Douglas-Peuckerで単純化」：許容値１ｍ

  <!-- image: download failed (レイヤA 線で分割・単純化の結果 1) -->

  <!-- image: download failed (レイヤA 線で分割・単純化の結果 2) -->

  - レイヤB（立体）: **`layer＞０`**/マイナス、または **`bridge=yes`** の線。
    - **レイヤBの処理:** （交差点分割はせず）➔ 「Douglas-Peuckerで単純化」許容値0.5
  - レイヤC（垂直）: **`highway=steps`** 。
  - **レイヤの再統合（マージ）**
    - 処理が終わったAレイヤ、Bレイヤ、Cレイヤを1つのレイヤに結合（ベクタレイヤのマージ）します。
- 立体交差は分割しない：check
- snapping
  - 0.5m
  - 最寄点を優先するが、頂点は追加しない

1. road_segment

### ④ Douglas-Peucker

- tolerance 1（田奈駅）

> [!NOTE]
> ③と一緒に行った

## フェーズ2：主分析エリアのみ手動処理

ここが研究の質を決める。

対象：

- 駅から半径1〜1.5km程度

---

## 手動でやるのはこの4つだけ

論文p.5・p.10–11参照
Road-centre-line-simplification…

### 1️⃣ ラウンドアバウト

円形のまま使わない。

進入→退出を直線で結ぶ。

### 2️⃣ Staggered junction

少しズレた交差点は斜め直線で統合。

### 3️⃣ Square（広場）

出入口同士を直線接続。

### 4️⃣ Parallel lanes

平行2本を1本の中心線へ。

> [!NOTE]
> ⇒これだけやった。基本的には残している。いろいろ削ると道路の繋がりがなくなるため。

> [!NOTE]
> Space Syntax ToolkitとDepthmapXの両方で行ったが特に違いは発生せず。この手順で田奈駅も実装する

## 新百合ヶ丘　Result

- 上位INTr900m20％：1455/7274→367.62
