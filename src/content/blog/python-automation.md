---
title: 'Python で業務を自動化する方法'
description: '日々の繰り返し作業を Python スクリプトで効率化するテクニックを紹介します。'
pubDate: 'Nov 15 2024'
---

エンジニアとして働いていると、毎日のように繰り返し行う作業に出会います。そんなとき、Python を使って自動化することで、時間を大幅に節約できます。

## なぜ Python なのか

Python は自動化に最適な言語です。その理由は以下の通りです：

- **シンプルな構文**: 読みやすく、書きやすい
- **豊富なライブラリ**: ほとんどのタスクに対応するライブラリが存在
- **クロスプラットフォーム**: Windows, Mac, Linux で動作

## 自動化の例

### 1. ファイル操作の自動化

```python
import os
import shutil
from pathlib import Path

def organize_downloads():
    """ダウンロードフォルダを整理する"""
    downloads = Path.home() / "Downloads"
    
    extensions = {
        '.pdf': 'Documents',
        '.jpg': 'Images',
        '.png': 'Images',
        '.mp4': 'Videos',
    }
    
    for file in downloads.iterdir():
        if file.is_file():
            ext = file.suffix.lower()
            if ext in extensions:
                dest = downloads / extensions[ext]
                dest.mkdir(exist_ok=True)
                shutil.move(str(file), str(dest / file.name))
```

### 2. Web スクレイピング

```python
import requests
from bs4 import BeautifulSoup

def get_news_headlines(url):
    """ニュースサイトからヘッドラインを取得"""
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    headlines = soup.find_all('h2', class_='headline')
    return [h.text.strip() for h in headlines]
```

### 3. Excel 操作

```python
import openpyxl

def merge_excel_files(file_list, output_file):
    """複数の Excel ファイルを1つにまとめる"""
    wb_out = openpyxl.Workbook()
    ws_out = wb_out.active
    
    for file in file_list:
        wb = openpyxl.load_workbook(file)
        ws = wb.active
        
        for row in ws.iter_rows(values_only=True):
            ws_out.append(row)
    
    wb_out.save(output_file)
```

## 自動化のベストプラクティス

1. **小さく始める**: 最初は簡単なタスクから自動化
2. **エラーハンドリング**: 予期せぬ状況に対応できるように
3. **ログを残す**: 何が起きたか後から確認できるように
4. **定期実行**: タスクスケジューラや cron を活用

## まとめ

自動化は最初の設定に時間がかかりますが、長期的には大きな時間の節約になります。まずは小さなタスクから始めて、徐々に範囲を広げていくのがおすすめです。

次回は、Power Automate を使ったノーコード自動化について紹介する予定です。
