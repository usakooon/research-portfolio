# 2026-02-21 比較駅と高齢者(65歳以上)歩行平均速度決定／15分徒歩圏ポリゴン生成／道路データ準備

> **Summary (EN):** Fixed the station pair (Shin-Yurigaoka: 20.53% aged 65+, 13.901% mean slope; Tana: 21.95%, 9.774%) and adopted 1.0 m/s (3.6 km/h) as the representative walking speed for people aged 65+, based on the design speed used for pedestrian signal timing and supporting literature (≈1.06 m/s for older people; FHWA's 0.9 m/s). Generated 15-minute (900 s) walking isochrones with QNEAT3 and started road-network preparation: OSM roads clipped to a 2 km buffer, motorway/indoor/private/parking_aisle removed, trunk/primary/secondary kept, and geometries snapped.
>
> **Phase:** 2. Network & slope data preparation (道路・勾配データ整備) · **Tools/Data:** QGIS, QNEAT3 (Iso-Area as Polygons), OSM roads, JGD2011 平面直角座標系, depthmapX

[← 作業ログ一覧 / Work log index](README.md)

---

# Station

新百合ヶ丘：20.53%(高齢化率)＋**13.901%**（平均勾配）
**田奈駅（Tana）**：21.95%（差 +1.42）＋**9.774%**（青葉区内でかなり平坦側）

# Walking speed for elderly people(≥65)

1.0m/s

60m/m

3.6km/h

⇒15分徒歩圏ポリゴン生成

> [!NOTE]
> 本研究では65歳以上を一括して扱うため、歩行速度は歩行者信号の青時間設定で一般的に用いられる設計歩行速度1.0 m/sを代表値として採用した。なお高齢者の歩行速度には個人差があり、これは平均値ではなく基準値である点に留意する。

参考文献

[歩行者横断秒数の延長　警視庁](https://www.keishicho.metro.tokyo.lg.jp/kotsu/iken_yobo/shingo_faq/crosswalk_extention.html)

> 歩行者の安全を確保するために、渋滞状況など他の状況も考慮しながら検討して参ります。
>
> 歩行者用信号の青時間は、横断歩道の長さによって異なります。一般的には歩行速度を秒速1メートルとして道路を渡りきれるよう調整しています。
>
> 歩行速度が遅い高齢者や子供が多く利用する場所については、より多くの時間を出すよう考慮しています。

（参考資料: hatoyama216748.pdf, 元ファイルは非公開）

> 歩行者の挙動に関する研究としては、まず信号交差点における横断歩行者の歩行速度はどのように分布しているのかを 10,000 人を超える大量なサンプルの分析から明らかにした斉藤・有薗（1986）の研究がある。これによると、社会人の歩行速度の 50 パーセンタイル値は約 1.37m/s（N=11604）であることや、老人のそれは約 1.06m/s（N=146）であることなどが示されている

[www.iatss.or.jp](https://www.iatss.or.jp/entry_img/31-4-14.pdf)

<!-- image: download failed (歩行速度に関する参考資料の抜粋) -->

[www.fhwa.dot.gov](https://www.fhwa.dot.gov/publications/research/safety/pedbike/05085/pdf/lesson8lo.pdf)

> Guidelines and Recommendations to Accommodate Older Drivers and Pedestrians recommends an assumed walking speed of 0.9 m/s (2.8 ft/s) for less capable older pedestrians because of their shorter stride, slower gait, difficulty negotiating curbs, difficulty judging speeds of oncoming vehicles, and exaggerated startup time before leaving the curb (see figure 8-2).

---

# 15分徒歩圏ポリゴン生成

- Tool：Iso-Area as Polygons (from Point) ⇒QNEAT3
- Optimization：Fastest path (time)
- Time field：900(秒単位)
- Contour interval：300
- Cellsize：10（Interpolation使わないなら不要）
- Speed: 3.6km/h
- CRS:そろえること。WGS84は使えない

<!-- image: download failed (QNEAT3による15分徒歩圏ポリゴン) -->

---

# 道路データの準備

## 道路の整備

### ①投影座標をメートルに統一

→ JGD2011 / 平面直角系でOK

### ②ネットワーク範囲

- 15分徒歩圏より大きい範囲でDepthmapXにimportする。
- Bufferを2kmに設定し、道路データをclip

### ③削除する道路

まずこれを入れてください：

```txt
"highway"IN ('motorway','motorway_link')
```

選択されるはずです。

---

### trunkも消すなら

```txt
"highway"IN ('motorway','motorway_link','trunk')
```

---

### parking_aisleも消すなら

```txt
"highway"IN ('motorway','motorway_link','trunk','parking_aisle')
```

---

### private道路を選択

```txt
"access"='private'
```

---

### indoorを選択

```txt
"indoor"='yes'
```

> [!NOTE]
> 新百合ヶ丘の場合、'parking_aisle'、"indoor" = 'yes'、"access" = 'private'　に該当する道路を削除

### ④スナップ

- **「ジオメトリをレイヤにスナップ」**
- road_raw_2kmコピーを入力レイヤ、参照レイヤに

<!-- image: download failed (ジオメトリをレイヤにスナップの設定画面) -->

> [!NOTE]
> - ✅ motorway / motorway_link：削除
> - ✅ indoor / private / parking_aisle：削除
> - ✅ trunk/primary/secondary：**残す**（歩道がある限り）
> - ✅ 2–3kmでネットワーク作成 → Depthmap計算 → 最後に15分圏で切り出し
