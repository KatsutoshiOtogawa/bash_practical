# bash_practical
自分メモ用。


## ファイルに対する処理

```
# 固定長の文字列があるファイル作成
width=12
row=6
cat /dev/urandom | tr -dc a-zA-Z0-9 | fold -w $width | head -n $row > list.txt
## 速度気にしないなら下の方が安全で楽。パスワードにも使えるし。
bits=256
seq 1 $row | xargs -n 1 bash -c "pwmake ${bits} | cut -c 1-${width} >> list.txt"
 
# ファイルの特定の文字列を変換する
# 変換前のファイルlist.txt.orgとlist.txt後のファイルlist.txtができる。バックアップがいらないなら-i.orgを-iに変更。
search=SEARCH
replace=REPLACE 
sed -i.org "s/${search}/${replace}/g" list.txt
## 検索して表示のみの場合
sed "s/${search}/${replace}/g" list.txt

# list.txtにある文字列を一行ずつ処理。
cat list.txt | xargs -n 1 -I {}  echo {}

# ランダムな名前リスト作成
## pythonのライブラリが必要になる。
## pip3 install names
row=30
seq 1 30 | xargs -n 1 names >> list.txt

# 指定の語句を含む行のみ処理
search=awesome
grep $search list.txt | xargs -n 1 -I {} echo {}

# 指定の語句を含まない行のみ処理
grep -v $search list.txt | xargs -n 1 -I {} echo {}

# 指定の行のみ処理
row=3
echo `sed -n ${row}p list.txt`
```


## dbに対する処理
例はすべてsqlite3。他dbでも要領は同じ。
```
# テーブル作成
sqlite3 list.db << END
CREATE TABLE your_table (
  id integer primary key autoincrement
  ,name text
);
END

# ランダム文字列作成
## sqlite3では難しいので上記にある固定文字列の作成から作成したファイルの文字列を放り込むのが楽。
cat list.txt | xargs -n 1 -I {} echo "INSERT INTO your_table(name) VALUES('{}');" | sqlite3 list.db

# データベースないのデータをbash上で処理
## プログラミング言語でも要領は同じだが、
## 他のシステムとの連携や再起クエリなどで表現できない繰り返し処理が多いならストアド推奨。
## ストアドが必要な可能性がある時点でsqlite3から他のDBに以降すべき。
echo "SELECT * FROM your_table" | sqlite3 list.db | xargs -n 1 -I {}  echo “{}”
### ストアド(sqliteではないので)
### 例えば、あなたのシステムはユーザー情報(名前、emailaddress,住所,職種,役職)を管理しており、
### 
## 参考postgresql
## select from your_scheme.your_table select from other_scheme.other_table


```

## カレントフォルダに対する処理

```
# 実行前にechoでどうなるか必ず確認すること。
ls -1 | xargs -I {} echo {} 

# 現在のフォルダのファイルを特定のフォルダに移動
destination=/home/path/to
ls -1 | xargs -I {} mv {} $destination

# 現在のフォルダのファイルを特定のフォルダにコピー(上と同じ要領)
ls -1 | xargs -I {} cp {} $destination
```

# 参考
[Bashレファレンス](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html)
