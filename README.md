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
 
# ランダムな位置に特定の文字列を含むファイル作成


# list.txtにある文字列を一行ずつ処理。
cat list.txt | xargs -n 1 -I {}  echo {}

# 指定の語句を含む行のみ処理
search=awesome
grep $search list.txt | xargs -n 1 -I {} echo {}

# 指定の語句を含まない行のみ処理
grep -v $search list.txt | xargs -n 1 -I {} echo {}

# 指定の行のみ処理
row=3 # 使いたい行に数字を変更
echo `sed -n ${row}p list.txt`
```


## dbに対する処理
例はすべてsqlite3。他dbでも要領は同じ。
```
# テーブル作成
sqlite3 list.db << END
CREATE TABLE your_table (
  id int autoincrement
  ,name text
);
END

# INSERT
echo "insert "

echo "select * from your_table" | sqlite3 list.db | xargs -n 1 -I {}  echo “{}”
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
