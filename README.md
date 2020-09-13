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

echo "SELECT * FROM your_table" | sqlite3 list.db | xargs -n 1 -I {}  echo “{}”
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
