---
layout: ../../layouts/MarkdownPostLayout.astro
title: 'entersys詳細'
pubDate: Jun 23 2023
description: 'This is a record of the creation of "entersys"'
author: 'gae'
---

## 作成準備

`entersys`を作成するにあたり，大まかに流れを考えた．大まかに以下の 5 つに分けられる．

-   id 判別
-   入退室判別
-   slack 送信
-   名簿の自動更新
-   入室者一覧更新

## 作成

### id 判別

まず，id 判別をどのように何を使って行うのかについては，学生証のバーコードをバーコードリーダーを用いて読み取り，名簿と照合することにした．

```python:main.py
while True
    input("Student ID > ")
```

次にバーコードで読み取った番号をどのように名簿と照合するのかであるが，今回は`csv`モジュールを用いて csv ファイルを読み込み，配列に格納してから照合していくようにした．他にも`pandas`便利なモジュールもあるが今回は気分でこっちにした．標準ライブラリであるため，インストール等は不要である．

```python:get_id.py
import csv
import update_member

with open('csvfile') as f:
    reader = csv.reader(f)
    row_list = [row for row in reader]
lastrow = len(row_list)
with open('csvfile') as f:
    for i in range(0, lastrow):
        if number == row_list[i][1]:
            id = row_list[i][0]
    break

if id == "":
    update_member.update_member()
    with open('csvfile') as f:
        reader = csv.reader(f)
        row_list = [row for row in reader]
    lastrow = len(row_list)
    with open('csvfile') as f:
        for k in range(0, lastrow):
            if number == row_list[k][1]:
                id = row_list[k][0]
            break
```

名簿にない場合には，入部届のスプレッドシートからこの csv ファイルに追加するようにした．それでも見つからない場合は，`ldapsearch`を用いることにした．ここまでに見つからない場合はエラーを返す．

ここでは，`Google Spreadsheet`から入部届の情報を見るために，`oauth2client`モジュールと`gspread`モジュールを使用している．Google の API 関連についてはいろいろ転がっているのでそちらを参考に．

```python:update_member.py
import gspread, csv
from oauth2client.service_account import ServiceAccountCredentials

csvfile = 'csvfile'
scope = []
json = "jsonfile"
credentials = ServiceAccountCredentials.from_json_keyfile_name(json, scope)
gc = gspread.authorize(credentials)
workbook = gc.open_by_key("key")
worksheet = workbook.get_worksheet(0)

def update_member():
try:
    with open(csvfile) as f:
        reader = csv.reader(f)
        row_list = [row for row in reader]
    lastrow_in = len(list(filter(None, worksheet.col_values(2))))
    # 最新のidが一致しない時
    if worksheet.acell("A" + str(lastrow_in)).value != row_list[0][0]:
        # 入部届の一致する場所探して
        for k in range(2, lastrow_in + 1):
            if row_list[0][0] == worksheet.acell("A" + str(k)).value:
                # 一致する場所から最終行まで追加
                worksheet_last_id = worksheet.acell("A" + str(lastrow_in)).value
                while row_list[0][0] != worksheet_last_id:
                    row_list.insert( 0, [worksheet.acell("A" + str(k + 1)).value, worksheet.acell("B" + str(k + 1)).value],)
                    k += 1
                break
        with open(csvfile, "w") as f:
            writer = csv.writer(f)
            writer.writerows(row_list)
except:
    pass
else:
    with open(csvfile) as f:
        reader = csv.reader(f)
        lastrow = len(row_list) - 1
        row_list = [row for row in reader]
```

ここまでが id 判別の部分である．

### 入退室判別

次に入退室判別だが，長くなったので次に回そうと思う．

まだ次回．
