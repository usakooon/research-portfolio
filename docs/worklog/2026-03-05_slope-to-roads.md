# 2026-03-05 道路データにslopeを付与

> **Summary (EN):** Assigned a longitudinal grade to every road piece by splitting lines into ≤10 m segments, sampling DEM elevation at start and end vertices, and computing `abs(Zend − Zstart) / $length × 100` using each segment's actual length. Instead of setting bridges to 0% (put on hold), grades above 30% were set to NULL as DEM artefacts. With 7,010 segments, the top-20% integration threshold was INTr900m ≥ 368.40308 (1,410 segments); 81.0% of segments had grade ≤ 8%, 1,241 (17%) were both highly integrated and low-gradient, and 171 (2.4%) were highly integrated but steep.
>
> **Phase:** 3. Analysis & first results (分析・初期結果) · **Tools/Data:** QGIS (Split lines by maximum length, Extract specific vertices, Sample raster values, Field Calculator), DEM, OSM roads, depthmapX/Space Syntax Toolkit output

[← 作業ログ一覧 / Work log index](README.md)

---

## 道路に grade(%)（縦断勾配）を付与する作業フロー（簡潔版）

---

## 0) 前提

- 入力：道路ライン（OSM等）、DEM（標高ラスタ）
- 目的：各道路セグメントの **grade(%) = |Z_end − Z_start| / length × 100** を付与
- 分割長：**最大10m**（※分母は固定10ではなく各線分の実長）

---

## 1) 道路ラインを最大10mで分割

- ツール：**最大長で線を切断（Split lines by maximum length）**
- 最大長：10 m
  → 出力：10m以下の短い道路セグメント（最後は10m未満あり）

---

## 2) 分割後セグメントから「始点点」と「終点点」を作る

- ツール：**線の始点（Extract specific vertices：vertex=0）**
  - 入力：分割後セグメント：10ｍ
  - 出力：start_points
- ツール：**線の終点（Extract specific vertices：vertex=-1）**
  - 出力：end_points

---

## 3) DEMから標高を点に付与

- ツール：**ベクタレイヤにラスタ値を付加（Sample raster values）**
  - 入力：start_points → 出力：start_points_z（Zstart列）
  - 入力：end_points → 出力：end_points_z（Zend列）

---

## 4) 点の標高を「分割後セグメント」に戻す（IDで結合）

- 分割後セグメントに一意IDを付与（例：`seg_id`）
- start_points_z と end_points_z にも同じ ***fid*** が残る前提で進める
- ツール：属性テーブルで結合(table join)
  - セグメント ← start_points_z を ***fid*** で結合（Zstartを持たせる）
  - セグメント ← end_points_z を ***fid*** で結合（Zendを持たせる）

※「空間結合」でも可だが、点が線端にあるので誤結合を避けるなら **ID結合が安定**。

> [!NOTE]
> ※田奈駅ではfidが出力されなかったため、新規フィールド作成で、`$id`で新規idを作った

---

## 5) grade(%) を計算して列を作る

- フィールド計算機で新規列 `grade_pct`（実数）
- 式：
  - `abs("Zend" - "Zstart") / $length * 100`
  - [傾きの割合と傾き角を求める | アメリカ地質調査所](https://www.usgs.gov/educational-resources/determine-percent-slope-and-angle-slope)

ポイント：

- `$length` は **分割後の各セグメント長**（10m未満も反映）
- 分母を10固定にはしない

---

## 6) 橋の処理（原則 0%）　保留

- OSM属性などで `bridge` 判定できるならフラグ `is_bridge`
- `grade_pct_final` を作って：
  - `CASE WHEN "is_bridge" = 1 THEN 0 ELSE "grade_pct" END`

（湾曲橋は「湾曲」自体ではなくDEMが床版を表さない問題なので、0%扱いが一番壊れにくい）

これの代わりに、grade_pct_finalフィールドを作成し、下のコードを実行

```javascript
CASE WHEN grade_pct > 30 THEN NULL ELSE grade_pct END
```

---

## 7) 出力

- 分割済み道路セグメントに `grade_pct_final` が付いた状態
  → このまま「低勾配（≤8%）」判定や集計に使用

---

## Result

### **①Integration上位20％（高統合）**

全体　7010　(NULLを抜いたとき)

上位20％ (1402) 368.40308　→　1410

```javascript
 "INTr900m" >=368.40308 
```

<!-- image: download failed (Integration上位20%の道路セグメント地図) -->

### ②Grade8.0%以下（低勾配）

全体　7010

grade8.0%以下　5680

81.027%

### ③高統合∩低勾配

高統合∩低勾配：1241

17％

### ④高統合∩高勾配

高統合∩高勾配：171

2.439%
