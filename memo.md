### 変数

```
$RANDOM [0,32767]の区間のランダムな値
$$ 自身のプロセスID
$PIPESTATUS[@] パイプラインの全ての終了ステータス
```

### ビルトインコマンドか判定

```
type コマンド => ビルトインか調べる
which コマンド => ビルトインならbuilt-in commandと出る
```

### ステータス

```
1    標準出力
2    標準エラー出力
2>&1 標準エラー出力を標準出力に向ける
|&   標準出力、標準エラー出力の両方を次のコマンドに渡す

```

```
echo $? => 最後に実行されたコマンドのステータス
```

### ブレース展開

```
echo {a,b}{c,d} => ac ad bc bd
echo {a,b}.{c,d} => a.c a.d b.c b.d
echo {2..10..2} => 2 4 6 8 10
```

### ファイルグロブ

```
ls ?.txt => 任意の1文字.txt
ls {1,2}?.txt => 1? 2?のtxt
ls [^29].txt  => 2,9以外の1文字.txt
ls **/file_? => 任意の階層にあるfile_?に適合するファイル
```

### スペルチェック用辞書

```
cat /usr/share/dict/words
```

### ランダムな順序

```
cat anything | sort -R
```

### 確実に kill

trap で、シグナル(この場合は 2)に対する挙動を

```
trap '' 2 #シグナル2(Ctrl+Cで発生するシグナル)に対し、何もしないようにする
trap '' 15 #シグナル15(killコマンドの通常のシグナル)に対し、何もしないようにする
```

SIGKILL シグナルに対する挙動は、プログラム側で制御できない

```
kill -SIGKILL 25636
```

#### man kill の記述の抜粋

1 HUP (hang up)
2 INT (interrupt)
3 QUIT (quit)
6 ABRT (abort)
9 KILL (non-catchable, non-ignorable kill)
14 ALRM (alarm clock)
15 TERM (software termination signal)

### pipefail

パイプでつながったコマンドのどれかの終了ステータスが 1 以上なら、その時点でスクリプトが終了する

```
set -o pipefail
```

### true,false

```
true 終了ステータス0を返すだけのコマンド
false 終了ステータス1を返すだけのコマンド
```