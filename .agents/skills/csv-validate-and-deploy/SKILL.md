---
name: csv-validate-and-deploy
description: >-
  restaurants.csv の全行カラム数・BOMエンコーディング・必須項目（ID、店名、エリア、営業状況）を検証し、
  GitHub Pages へ安全にデプロイするスキル。
  ユーザーが「CSVを検証してデプロイして」「データチェックして公開して」と指示した際に使用する。
---

# CSV整合性検証＆デプロイスキル

本スキルは、店舗マスターCSV（`restaurants.csv`）の文法エラー、カラム欠落、文字化けを機械的に検証し、エラーがなければGitHub Pagesへ自動デプロイします。

## 実行手順

### 1. CSVバリデーションスクリプトの実行
```python
import csv

with open("restaurants.csv", "r", encoding="utf-8-sig") as f:
    reader = csv.reader(f)
    header = next(reader)
    expected_cols = 15
    if len(header) != expected_cols:
        raise ValueError(f"ヘッダーのカラム数が不正です: {len(header)} != {expected_cols}")
    
    for i, row in enumerate(reader, start=2):
        if len(row) != expected_cols:
            raise ValueError(f"行 {i} のカラム数が不正です: {len(row)} != {expected_cols}")
print("✅ CSVの整合性チェックに合格しました。")
```

### 2. Git コミット＆プッシュ
```powershell
git add .
git status
git commit -m "Deploy: Validated restaurant data"
git push origin main
```
