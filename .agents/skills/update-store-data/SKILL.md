---
name: update-store-data
description: >-
  福岡・ホテル一楽周辺の店舗データ（営業時間、休業日、座敷有無、子連れポイント等）を
  restaurants.csv および index.html に正確に反映・更新するスキル。
  ユーザーが「店舗情報を更新して」「新しい店を追加して」「営業時間を修正して」と指示した際に使用する。
---

# 店舗データ更新スキル

本スキルは、`restaurants.csv` の15カラム仕様と文字コード（UTF-8 with BOM）を保ちながら店舗情報を追加・更新し、Webアプリに反映させる手順を定義します。

## 実行手順

### 1. CSVデータの読み込みと編集
Python スクリプトを使用して、BOMを維持しながらCSVを更新します。

```python
import csv

# 読み書き時は utf-8-sig (BOM付き) を厳守
with open("restaurants.csv", "r", encoding="utf-8-sig") as f:
    reader = csv.DictReader(f)
    rows = list(reader)

# データの更新・追加処理...

with open("restaurants.csv", "w", encoding="utf-8-sig", newline="") as f:
    writer = csv.DictWriter(f, fieldnames=reader.fieldnames)
    writer.writeheader()
    writer.writerows(rows)
```

### 2. `index.html` 側のデータ整合性確認
`index.html` 内の静的フォールバックデータやUI構造に矛盾が生じていないか確認します。

### 3. Git コミット＆自動プッシュ
```powershell
git add restaurants.csv index.html
git status
git commit -m "Update: Restaurant store data"
git push origin main
```
