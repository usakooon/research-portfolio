# 2026-03-07 道路の延長調査

> **Summary (EN):** Switched the indicator from segment counts to total road length (`$length`, summed via the attribute-table statistics) for three categories: p1 high integration (top 20%), p2 low gradient, and p3 both. On the 10 m grade layer, p1/p2/p3 were 12.352 / 46.455 / 10.722 km of 59.554 km (20.7% / 78.0% / 18.0%). After aggregating 10 m grades onto the Space Syntax segments with a spatial join (mean) and using `grade_pct_final_mean <= 8.1`, the values became 14.751 / 52.266 / 13.386 km of 61.682 km (23.9% / 84.7% / 21.6%); the SS-segment layer was adopted as the common population.
>
> **Phase:** 3. Analysis & first results (分析・初期結果) · **Tools/Data:** QGIS (Field Calculator, Statistics panel, Join attributes by location (summary)), Space Syntax Toolkit segments, 10 m grade layer

[← 作業ログ一覧 / Work log index](README.md)

---

# STEP1 道路延長を作る

道路レイヤで

**フィールド計算機**

新しいフィールド

```txt
length_m
```

式

```txt
$length
```

これで

```txt
各道路の長さ（m）
```

が入ります。

QGISでは `$length` でジオメトリから長さを計算できます。

---

# STEP2 条件道路を作る

例

### 高統合

```txt
"Integration" >= 392.04123
```

### 低勾配

```txt
"grade_clean" <= 8
```

### 両方

```txt
"Integration" >= 392.04123
AND
"grade_clean" <= 8
```

---

# STEP3 延長合計を出す

属性テーブル

```txt
length_m
```

```txt
統計量の出力 (Statistics)
```

出てくる

```txt
Sum
```

これが

```txt
総延長
```

<!-- image: download failed (p1 の length_m 統計量) -->

<!-- image: download failed (p2 の length_m 統計量) -->

<!-- image: download failed (p3 の length_m 統計量) -->

<!-- image: download failed (all の length_m 統計量) -->

| length_m | Sum（m） | Sum（km） | all比（%） |
| --- | --- | --- | --- |
| p1 | 12351.637 | 12.352 | 20.7 |
| p2 | 46454.664 | 46.455 | 78.0 |
| p3 | 10722.067 | 10.722 | 18.0 |
| all | 59554.194 | 59.554 | 100.0 |

| 区分 | 意味 | 延長 (km) | 割合 |
| --- | --- | --- | --- |
| **p1** | 高統合（Integration 上位20%） | 12.352 | 20.7% |
| **p2** | 低勾配（≤8%） | 46.455 | 78.0% |
| **p3** | 高統合 ∩ 低勾配 | 10.722 | 18.0% |
| **all** | 全道路 | 59.554 | 100% |

---

# STEP4 面積を出す　保留

徒歩圏ポリゴンで

フィールド計算機

新しいフィールド

```txt
area_km2
```

式

```txt
$area / 1000000
```

これで

```txt
km²
```

になります。

---

# STEP5 延長割合

例えば

```txt
高統合延長 / 全道路延長
```

例

```txt
38.1 km / 122.8 km
```

など。

# 10m grade → SSセグメントに集約（QGISフロー）

## 前提レイヤ

- **A：road_10m_grade**（10m分割道路。`grade_clean` が入ってる）
- **B：ss_segments**（Space Syntax Toolkitの交差点分割セグメント。Integration/NAIN/NACH/segLENなど）

---

## STEP 1：10m側の異常値処理（もうやってればOK）

10mレイヤ（A）に `grade_clean` を作る

- `CASE WHEN "grade_pct" > 30 THEN NULL ELSE "grade_pct" END`

※道路フィーチャは消さず、値だけNULLにするのが基本。

---

## STEP 2：SSセグメントに grade を“空間結合（集計付き）”で付ける

プロセシングツール：

**「属性の空間結合（集計付き）」**（Join attributes by location (summary)）

設定：

- **入力レイヤ（結合先）**：B（ss_segments）
- **比較対象（結合元）**：A（road_10m_grade）
- 空間関係：**交差する (intersects)**
- 集計するフィールド：`grade_pct_final`
- 集計関数：まずは **mean**（平均）
  （余裕があれば max も一緒に作ると後で比較できる）
- 出力：`ss_segments_with_grade`

結果：

- SSセグメントに `grade_clean_mean`（＋必要なら`grade_clean_max`）が追加される

---

## **STEP 4：p1/p2/p3の作り方（SSセグメント側で統一）**

以後は “ss_segments_with_grade” を母集団にして作ります。

- **p1 高統合**：`Integration >= ____(20%以上)`（上位20%の閾値 or ランク）

  <!-- image: download failed (p1 高統合の統計量) -->
- **p2 低勾配**：`grade_pct_final_mean <= 8.1`

  <!-- image: download failed (p2 低勾配の統計量) -->
- **p3 両方**：`Integration >= ____(20%以上) AND grade_pct_final_mean <= 8.1`

  <!-- image: download failed (p3 高統合∩低勾配の統計量) -->
- All

  <!-- image: download failed (all の統計量) -->

| length_m | Sum（m） | Sum（km） | all比（%） |
| --- | --- | --- | --- |
| p1 | 14751.032 | 14.751 | 23.9 |
| p2 | 52266.592 | 52.266 | 84.7 |
| p3 | 13386.945 | 13.386 | 21.6 |
| all | 61682.302 | 61.682 | 100.0 |

| 区分 | 意味 | 延長 (km) | 割合 |
| --- | --- | --- | --- |
| **p1** | 高統合（Integration 上位20%） | 14.751 | 23.9% |
| **p2** | 低勾配（≤8.1%） | 52.266 | 84.7% |
| **p3** | 高統合 ∩ 低勾配 | 13.386 | 21.6% |
| **all** | 全道路 | 61.682 | 100.0% |

> 表の丸め規則：Sumは小数第三位まで（以降切り捨て）、%は小数第一位まで（以降切り捨て）。

---

## STEP 5：延長を出す（SSセグメント側の長さを使う）

ここが重要：**segLENがkmっぽい単位なら、単位を統一**します。

一番安全なのは、SSセグメントで新規に

- `len_m = $length`
  を作って、それを集計に使うこと（単位が確実）。

集計：

- 各pレイヤで `len_m` の **Sum** → kmに換算（/1000）

---

# 超重要チェック（1分）

うまくいってるかはこれだけ見ればOK：

1. `ss_segments_with_grade` に **grade_clean_meanが入っている**
2. `grade_clean_mean` が NULL だらけではない（多くが値を持つ）
3. `len_m` の合計が、allで期待する総延長と近い

---
