### sed でサブパターン文字列を置換後に使う

```
#\1、\2で各サブパターン文字列が取れる
echo "xxx:yyy" | sed -E 's/(.*):(.*)/\2\1/'
```

### 以下は同じ動作

```
sort <(seq 10)
 seq 10 | sort
```

### ファイル削除時に 3 回上書き

```
rm -P ファイル
```

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
ls **/file_? => カレントディレクトリ下位の任意の階層にあるfile_?に適合するファイル
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

### ひとつ前のコマンドを表示

```
!!
```

### 一つ前のコマンドに対する置換

```
!!:s/置換対象/置換後/
```

#### 簡易置換

```
^置換対象^置換後

^1^5
```

### シェルスクリプトの構文解析のみ行う

```
bash -n シェルスクリプト
```

### 環境変数一覧

```bash
set
```

### grep コマンドの正規表現オプション

```bash
grep -G 基本正規表現
grep -E 拡張正規表現
grep -P Perl正規表現
```

### sed のサブパターンで大文字=>小文字、小文字=>大文字

(mac 標準 sed では不可。)

```bash
# 小文字 => 大文字 (\Uは\Eまで有効。\Eは省略可)
echo kingKONG | sed -E 's/(.*)/\U\1\E/'

# 大文字 => 小文字
echo kingKONG | sed -E 's/(.*)/\L\1\E/'
```

### テキストの各行を反転表示

```bash
rev text.txt
```

### grep で正規表現のリストをファイルから受け取る

```bash
grep -f exp_file ...
#-f の後に何も書かない(環境によっては-)の場合は標準入力から受け取る
```

### nkf

```bash
echo ナニカシラ | nkf --hiragana #ひらがなにする -hでも可
echo なにかしら | nkf --katakana #カタカナにする
echo ナニカシラ | nkf -Z4        #カタカナの全角半角切り替え
echo ﾅﾆｶｼﾗ | nkf -Z             #半角を殲滅
```

### trans コマンド

翻訳。要インターネット接続
https://www.soimort.org/translate-shell/#installation からインストール

```bash
# 日本語->英語
echo 大盤石を覆すが如し  | trans ja:en
```

### mecab コマンド

形態素解析。

```
brew install mecab
brew install mecab-ipadic
```

```bash
echo 大盤石を覆すが如し  | mecab
```

### pandoc コマンド

ドキュメントのフォーマット変換。

```
brew install pandoc
```

```bash
#markdown->html
echo '# 見出し' | pandoc
```

### yes コマンド

echo の無限版

```
yes takasu
```
