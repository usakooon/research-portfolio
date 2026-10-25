# 2026-02-16 留意点：平均勾配の過小評価と橋・立体交差

> **Summary (EN):** A methods note on two pitfalls when deriving road gradients from a DEM: averaging over long segments hides short steep sections, and bridges/underpasses/tunnels produce spurious extreme gradients because the DEM represents the ground surface. Countermeasures listed: judge by max or 95th-percentile slope (e.g. `slope_max <= 8%`), aggregate 10 m segments back with both mean and max, exclude `bridge`/`tunnel`/`layer ≠ 0` features, and inspect segments with slope > 20–30%.
>
> **Phase:** 2. Network & slope data preparation (道路・勾配データ整備) · **Tools/Data:** QGIS, DEM, OSM roads

[← 作業ログ一覧 / Work log index](README.md)

---

## A）平均勾配が“過小評価”になるってどういうこと？

### 何が起きる？

道路1本（長いセグメント）に対して「平均勾配」を作ると、途中に **短い急坂**があっても、

- 急坂区間（例：10mで15%）
- 平坦区間（例：190mで2%）

みたいな場合、平均すると「だいたい2〜3%」になって、**“歩行上のボトルネック”が消えます**。

歩行のしんどさって、現実には「短い急坂でもしんどい」ので、平均だけだとズレやすい、という話。

### どう対策する？

本研究の目的（“歩ける中心軸がどれだけ残るか”）なら、次のどれか1つ採用で十分です。

**対策1（いちばん簡単・堅い）｜最大値で判定する**

- `slope_max <= 8%` を“歩ける”条件にする
  → 急坂が1箇所でもある道は弾ける（厳しめだが安全）

**対策2（論文っぽく“外れ値ノイズ”も避ける）｜95パーセンタイルで判定**

- `slope_p95 <= 8%`
  → 1点だけのノイズ（誤サンプル）を無視しつつ、急坂は拾える

**対策3（10m分割しているなら相性◎）｜元の線に戻すとき“集約の仕方”を変える**

10m分割した道路（segments10m）に slope が入ってるなら、

- 元の道路（original）へ戻すときに **平均だけじゃなく max も作る**と良いです。

QGISなら：

- **Processing → Statistics by categories**
  - Categories（分類）：`original_id`
  - Values（値）：`slope_%`
  - 統計：`mean` と `max`（可能なら `percentile 95`）
- 出力テーブルを元道路に **属性結合**（original_idで結合）

→ これで「mean_slope と max_slope を両方持った元道路」ができます。

## B）橋・アンダーパスで“勾配が壊れる”ってどういうこと？

### 何が起きる？

DEMは基本 **地表（ground）** の標高です。

でも道路は、

- **橋・高架**：地表より上を走る
- **立体交差**：上下に重なる
- **トンネル**：地表の下を通る

ので、道路線の位置でDEMをサンプルすると、

- 橋の上を走ってるのに、DEMは「谷底の地面」を拾う
  → 標高差が“急に落ちた”ように見えて **異常な急勾配**が出る

という現象が起きます。

### どう対策する？

**対策1（現実的・卒論向き）｜橋/トンネルっぽい区間を除外 or 別扱い**

OSM道路を使っているなら、属性に `bridge` / `tunnel` / `layer` が入っていることがあります。

- `bridge = yes` や `tunnel = yes`、`layer ≠ 0` を **除外**
- もしくは「別レイヤ」にして解析結果から注釈をつける

**対策2｜“ありえない急勾配”を検出して点検**

例えば：

- `slope_max > 20%` とか `>30%` を抽出して地図で見る
  → 立体交差・橋・DEM誤差の候補が一気に見つかります

**対策3｜DEMの解像度・種類を変える（できれば）**

- 5m/1m DEM（レーザ等）があると改善することが多い
- ただし「橋の上面」を表すDSMが必要な場合もあり、データ次第

卒論の時間感なら、まずは **対策1＋2** で十分です。
