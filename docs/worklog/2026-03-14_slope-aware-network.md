# 2026-03-14 勾配考慮型ネットワーク分析

> **Summary (EN):** Designed a minimal, deadline-friendly way to turn slope into a walking-time cost: using the 10 m road segments, the end nearer to the station is identified with "Distance to nearest hub", a signed slope toward the station is computed, and Tobler's hiking function converts it into speed and travel time in seconds per segment. A full bidirectional (separate uphill/downhill speed) implementation was deliberately postponed because QGIS shortest-path tools accept only one speed field per line.
>
> **Phase:** 4. Method revision & conference prep (手法見直し・学会準備) · **Tools/Data:** QGIS (Extract specific vertices, Distance to nearest hub, Join attributes by field value, Field calculator), DEM, Tobler's hiking function

[← 作業ログ一覧 / Work log index](README.md)

---

**締切優先なら、駅へ向かう方向だけに限定してやる**のが一番現実的です。

Tobler は、勾配 (s) に応じて歩行速度 (W) を変える関数で、代表形は **W = 6e^(−3.5|s+0.05|)**（km/h）です。平地では約5 km/h、わずかな下りで最速になります。 ([Wikipedia](https://en.wikipedia.org/wiki/Tobler%27s_hiking_function))

### 最小手順

1. **10mセグメントを使う**

   いまの10m分割道路をそのまま使います。始点と終点は QGIS の **Extract specific vertices** で、`0` が始点、`-1` が終点です。 ([QGIS](https://docs.qgis.org/3.40/lt/docs/user_manual/processing_algs/qgis/vectorgeometry.html))

   > [!NOTE]
   > 💡 seg_idを作ること：`$id`

2. **始点・終点の標高を取る**

   もうやっている方法でOKです。DEMから `Zstart` と `Zend` を取ります。

   > [!NOTE]
   > 💡 保存しないとseg_idが消える

3. **駅に近い方を「駅側の端点」と決める**

   start点とend点の両方について、駅ポイントまでの距離を出します。QGIS には **Distance to nearest hub (points)** があり、点から最寄りハブまでの距離を属性として持たせられます。 ([QGIS](https://docs.qgis.org/latest/en/docs/user_manual/processing_algs/qgis/vectoranalysis.html))

   - 入力；start/end points
   - hub：駅代表中心点
   - 単位：ｍ

   そして

   - `d_start > d_end` なら **start → end が駅へ向かう方向**
   - `d_end > d_start` なら **end → start が駅へ向かう方向**

     > [!NOTE]
     > 💡
     > - `d_start > d_end`
     >   → end の方が駅に近い
     >   → **start → end が駅へ向かう方向**
     > - `d_end > d_start`
     >   → start の方が駅に近い
     >   → **end → start が駅へ向かう方向**

     と決めます。

     - ツール：**Join attributes by field value**
     - Input Layer：`10mセグメント`
     - Table field：`fid`
     - Input layer 2：`start_points`
     - Table field 2：`fid`

   ```javascript
   CASE
   WHEN   "start_HubDist"  >   "endHubDist"   THEN 'start_end'
   WHEN   "endHubDist"   >   "start_HubDist"   THEN 'end_start'
   ELSE 'same'
   END
   ```

4. **「駅へ向かう勾配」を作る**

   ここがポイントです。

   - `d_start > d_end` のとき：`s_to_station = (Zend - Zstart) / $length`
   - `d_end > d_start` のとき：`s_to_station = (Zstart - Zend) / $length`

     これで、**駅へ向かう方向で見た signed slope** が出ます。

     - 上り坂 → **正**
     - 下り坂 → **負**
     - 平坦 → **0**

     ```javascript
     CASE
     WHEN  "start_HubDist"  >  "endHubDist" THEN ( "enddem1"  -  "start_dem1" ) / $length
     WHEN  "endHubDist"  >  "start_HubDist"  THEN ( "start_dem1"  -  "enddem1" ) / $length
     ELSE 0
     END
     ```

5. **Toblerで速度を作る**

   フィールド計算機で

   ```javascript
   speed_kmh_old = 4.32 * exp(-3.5 * abs("s_to_station" + 0.05))
   ```

   を作ります。これは Tobler の式そのものです。 ([Wikipedia](https://en.wikipedia.org/wiki/Tobler%27s_hiking_function))

6. **時間コストを作る**

   `time_sec = ($length / 1000) / "speed_kmh" * 3600`

   で、各セグメントを駅へ向かうときの**所要時間（秒）**にできます。

   > [!NOTE]
   > 💡 10ｍ=`$length`だから、短すぎて分は0になってしまうから、秒で計算

### ここまでで何ができるか

この方法でまずできるのは、**各道路セグメントの「駅へ向かう負担」を可視化すること**です。

つまり、

- この道は駅へ向かうと急で時間がかかる
- この道は平坦で早く到達できる

  を地図で出せます。

### いまはやらなくていいこと

QGIS の shortest path 系アルゴリズムは **Fastest** と **Speed field** を持っていますが、基本的に1本の線に対して使う速度欄は1つです。方向フィールドは持てても、**上り用速度と下り用速度を同じ1本の線に別々に持たせる**ところまでは素直ではありません。なので、完全な双方向実装は後回しで大丈夫です。 ([QGIS](https://docs.qgis.org/3.10/fr/docs/user_manual/processing_algs/qgis/networkanalysis.html))

### 結論（本研究向け）

**今回は「駅へ向かう方向だけの Tobler 時間コスト」を作る**で十分です。

これなら、

- 上り下り問題を無視していない
- 実装がまだ間に合う
- 高齢者が駅へ向かう負担を直接表現できる
