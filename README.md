# bash_practical
自分メモ用。


## ファイルに対する処理

```
# list.txtにある文字列を一行ずつ処理。
cat list.txt | xargs -n 1 -I {}  echo “{}”

echo "select * from your_table" | list.db | xargs -n 1 -I {}  echo “{}”
```

## dbに対する処理
例はすべてsqlite3。他dbでも要領は同じ。
```

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
