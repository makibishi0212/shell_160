### -

- で標準入力から受け取った値をコマンドに渡せる

```bash
echo "abcdef" | diff - <(echo "abcdefg")
```

### dev/null

捨て場

```bash
echo a > /dev/null
```

### 出力を横に並べる

```bash
seq 10 | xargs
```

### sed でサブパターン文字列を置換後に使う

```bash
#\1、\2で各サブパターン文字列が取れる
echo "xxx:yyy" | sed -E 's/(.*):(.*)/\2\1/'

#サブパターン文字列は置換前にも使える ((.*?)は最短マッチ)
echo '15152@!2@!00' | gsed -E 's/(.*?)\1/\1/g'
```

### sed で特定行のみ抜き出す

```bash
seq 100 | sed -n '11,14p'
```

### 以下は同じ動作

```bash
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

### file コマンド

そのファイルがなんのファイルか表示する

```
file memo.md

memo.md: UTF-8 Unicode text
```

画像の場合、形式に加えて iphone で撮影したなどの付帯情報が見られることもある

```
file ../ignore/white_negi.jpg

iPhone 5, orientation=lower-right, xresolution=150, yresolution=158, resolutionunit=2, software=7.0.4, datetime=2013:11:21 11:47:07], baseline, precision 8, 3264x2448, components 3
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

### awk

#### print

出力

```bash
echo 1 | awk '{print $1}'
```

#### sprintf

値をフォーマットする

```bash
echo 1 | awk '{$1=sprintf("%03d",$1);print}'
```

#### -F オプション

```bash
# 区切り文字を指定する
awk -F'[/ ,]' '{...}'
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

```bash
yes takasu
```

### column コマンド

出力を揃える

```bash
column -t

# 例
seq 20 | awk '{print $1,$1}' | column -t
```

### jq で特定のフィールドの値を表示

```bash
somthing.json | jq .a.b.c
```

### jq で計算

```bash
somthing.json | jq '.a.b.c + a.b.d'
```

```bash
#上と同じ
cat something.json| jq '.a.b | .c +.d'
```

### sed で最後の行を削除

```bash
seq 3 | sed '$d'
```

```bash
# 4行目以降を削除する
seq 10 | sed '4,$d'
```

### sed で特定行を 2 度表示

```bash
#最後の行を2度表示
seq 10 | sed '$p'
```

```bash
#２行目から2度表示
seq 10 | sed '2,$p'
```

### seq -f

```bash
#フォーマットする
seq -f 'count:%g' 100
```

### join コマンド

2 つの入力を先頭の要素を使って結合

```
join file1 file2
```

```
# aオプションで外部結合
join -a 1 -a 2 file1 file2
```

### 基数変換

```bash
# $((n#n進数の値)) で10進数の値を得ることができる
echo $((16#101e))
```

### echo -e

```bash
# ユニコードエスケープシーケンスを変換し、元の文字を表示
echo -e '\U5A9B'
```

### xxd コマンド

入力を 16 進数でダンプするコマンド

```
echo 👽 | iconv -f UTF-8 -t UTF-32 | xxd

00000000: 0000 feff 0001 f47d 0000 000a            .......}....
```

- `00000000:` オフセット
- `0000 feff` Byte Order Mark (BOM)
  - `0000 feff`はビッグエンディアンで、-> 向きにデータを 1 バイトずつ読み込んでいく (iconv の場合、ビッグエンディアンの場合 BOM をつけないことがある)
  - `fffe0000`はリトルエンディアンで、<- 向きにデータを 1 バイトずつ読み込んでいく
- `0001 f47d` 👽 に対応
- `0000 000a ` 改行文字に対応
- `.......}....` 当該の行のデータに相当する文字 (文字化けしている)

-p オプションでスペースなし、最後の人間用の文字列なしで表示する。ワンライナーではこちらを使うことが多い

-r オプション(-p と併用)でエンコードではなくデコードを行う

```
echo aaa | xxd -p | xxd -p -r

aaa
```

### od コマンド

入力を 8 進数や 16 進数でダンプするコマンド

-x で 16 進数にする
-tx1 とすると、1 バイトずつ出力する(この場合、xxd と同じ並び方になる)

```
echo 👽 | iconv -f UTF-8 -t UTF-32 | od -tx1

0000000    00  00  fe  ff  00  01  f4  7d  00  00  00  0a
0000014
```

### base64 コマンド

```bash
# base64に変換
echo aaa | base6

# -dオプション base64から戻す
echo aaa | base64 | base64 -d
```

### du コマンド

現在のディレクトリの各ファイルのブロックの個数を表示
1 ブロックは大抵 4096 バイト

```bash
du | sort -n
```

### bc を使って 16 進数->8 進数に

```bash
echo "obase=8;ibase=16;10" | bc
```
