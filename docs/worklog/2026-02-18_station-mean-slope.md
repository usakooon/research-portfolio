# 2026-02-18 青葉区「平均勾配」%と度

> **Summary (EN):** Computed a slope raster from the GSI DEM in QGIS, converted degrees to percent (`tan(deg × π/180) × 100`), and used Zonal Statistics on 1 km station buffers to rank stations by mean and max slope in Aoba Ward (Yokohama) and Asao Ward (Kawasaki). In Aoba Ward Ichigao (9.153%) and Tana (9.774%) were the flattest; in Asao Ward Shin-Yurigaoka scored 13.901%. Aobadai's extreme max (161.62%) was located by clipping the slope raster with its buffer.
>
> **Phase:** 1. Site selection (駅選定) · **Tools/Data:** QGIS (Slope, Raster Calculator, Buffer, Zonal Statistics, Clip raster by mask), 基盤地図情報 DEM

[← 作業ログ一覧 / Work log index](README.md)

---

## QGISで「平均勾配」を出す最短手順

### Step A-1：DEMをQGISに入れる

- 基盤地図情報からDEMを取得 → QGISへ読み込み
  （手順の例がまとまった解説もあります）

### Step A-2：Slope（傾斜）ラスタを作る

- **Processing → Toolbox → Raster terrain analysis → Slope**
- これは「傾斜角（degrees）」で出るのが基本です（QGIS公式ドキュメント）。

### Step A-3：（任意）傾斜を % に変換

%の方が直感的なら、Raster Calculatorで変換します。

**Raster calculator は角度計算が “radians” なので、度→ラジアン変換が必要**です。

- 変換式
  ```javascript
  tan("name" * 0.017453292519943295) * 100
  ```

### Step A-4：駅ポイント → 1kmバッファ（全駅まとめてOK）

- 駅ポイントレイヤを作成（手作業で点打ちでもOK）
- **Vector geometry → Buffer** で **距離=1km**（全ポイント一括で作れます）
- 重要：距離(m)を正しく扱うため、**投影座標系（メートル単位）**で作業してください（日本なら平面直角座標系やUTMなど）。

### Step A-5：Zonal Statisticsで平均勾配を付与

- **Processing → Zonal Statistics「ゾーン統計量（ベクタ）」**（ポリゴン＝バッファ、ラスタ＝slope）
- ポリゴン範囲に含まれるラスタ値を集約して属性として付ける、というのがZonal Statisticsです。

最低限は **Mean（平均）**だけでOK。可能なら **Min/Max/Std dev** も付けると“局所的な急坂が混じる駅”が見分けやすくなります（ツール側で選択できる範囲で）。

# Result

<!-- image: download failed (青葉区 駅1kmバッファのゾーン統計結果) -->

## 平均勾配（%）ランキング（小さい＝平坦寄り）

1. **市が尾 Ichigao：9.153**
2. **田奈 Tana：9.774**
3. 江田 Eda：10.500
4. 青葉台 aobadai：10.724
5. あざみ野 Azamino：10.786
6. たまプラーザ Tamapuraza：10.892
7. 藤が丘 Fujigaoka：11.619
8. 恩田 Onda：13.955
9. こどもの国 Kodomono：14.071

## max（%）のランキング（小さい＝極端な急斜面が少ない）

- **1位: あざみ野 81.33**（いちばん"極端値"が小さい）
- 2位: たまプラーザ 95.14
- 3位: 江田 96.06
- 4位: 藤が丘 100.53
- 5位: こどもの国 107.14
- 6位: 市が尾 108.17
- 7位: 恩田 116.39
- 8位: 田奈 124.05
- **9位: 青葉台 161.62**（突出して大きい＝バッファ内にかなり急な場所が含まれる）

# 青葉台のslopepct_maxはどこだ？

## 手順1：青葉台バッファだけをマスクにして slope_pct を切り抜く（Extract by mask）

1. **バッファ（駅1km円）レイヤ**を開いて、属性で `name = aobadai` の行をクリック
   → その1件だけ **選択状態**にする
2. **プロセシングツールボックス**で検索：
   - `マスク` と打つ

`GDAL > ラスタ抽出 > マスクレイヤで切り抜き (Clip raster by mask layer)`

1. パラメータ設定（共通の考え方）
   - **入力ラスタ**：`slope_pct`
   - **マスクレイヤ**：バッファレイヤ
   - **選択地物のみ使用**：✅（ここ重要。青葉台だけにする）
   - **出力**：`slope_pct_aobadai_clip.tif` などで保存

これで「青葉台の円の中だけの slope_pct」が1枚できます。

---

## 手順2：色分けで“どこが極端に急か”を一発で可視化

1. 切り抜いたラスタ（`…clip`）を右クリック → **プロパティ**
2. **シンボロジ** → `単バンド疑似カラー`
3. 分類を **連続**にして、Min/Max を **実データから取得**（更新/読み込みボタン）
4. 見つけたいのは 161% なので、**表示レンジを 0〜170** くらいにすると見やすい
   （最上位が真っ赤になるように）

ここまでで「161%が出てる斜面の塊」が地図上で見えるはず。

<!-- image: download failed (青葉台1km圏 slope_pct 切り抜きの疑似カラー表示) -->

# 麻生区の駅　平均勾配ランキング

## 麻生区：平均勾配（%）ランキング（小さい＝平坦寄り）

1. **wakabadai**：**13.207357342971456**
2. **kurihira**：**13.802538610610657**
3. **sinyuri**：**13.901424057916957**
4. **haruhino**：**14.125321603790425**
5. **kurokawa**：**14.810439431881473**
6. **yurigaoka**：**16.675312883810914**
7. **satsukidai**：**17.156625999091567**
8. **kakio**：**17.551034130778934**

<!-- image: download failed (麻生区 駅1kmバッファのゾーン統計結果) -->
