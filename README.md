# clipcount
![GitHub License](https://img.shields.io/github/license/yusu79/clipcount)
![PyPI - Version](https://img.shields.io/pypi/v/clipcount)
![PyPI - Downloads](https://img.shields.io/pypi/dm/clipcount)

クリップボードの文字を読み込み、文字数を出力するPythonパッケージです。

[Read this in English. (英語版のREADMEはこちら)](https://github.com/yusu79/clipcount/blob/main/README_en.md)


<!-- omit in toc -->
- [インストール（Setup）](#インストールsetup)
- [使い方（Quick usage）](#使い方quick-usage)
- [解説（Usage）](#解説usage)
- [使用しているパッケージ (Dependencies)](#使用しているパッケージ-dependencies)
- [開発日記（dev-diary）](#開発日記dev-diary)

## インストール（Setup）
pipでインストールします。
```PowerShell
pip install clipcount
```
### Windowsで「警告」が出た時の対処方法

Windowsで上記のコマンドを実行した際に「`"WARNING: The script clipcount.exe is installed in 'ファイルパス' which is not on PATH."`」が表示されたなら、以下の記事をご参照ください。

参考: [【Python Windows】pip install でPATHが通らない時の解決方法 | ゆすノート](https://yusu79.com/python-path-issue/)

「`WARNING: Failed to write executable - trying to use .deleteme logic`」という警告が出て`pip install`が出来ない場合は、以下の記事をご参照ください。

参考: [【Windows】警告が出て「pip install」できない時の解決方法 | ゆすノート](https://yusu79.com/pip-install-failure-fix/)


## 使い方（Quick usage）
| オプション          | 説明                                                                     | 
| ------------------- | ------------------------------------------------------------------------ | 
| clipcount -h        | ヘルプ画面を表示します。                                                 | 
| clipcount --help_jp | ヘルプ画面を日本語で表示します。                                         | 
| clipcount -b        | 改行コード（`\n`か`\r\n`）を削除した文字数を出力します。                 | 
| clipcount -s        | 半角空白を削除した文字数を出力します。                                   | 
| clipcount -S        | 全角空白を削除した文字数を出力します。                                   | 
| clipcount -t        | タブコード（`\t`）を削除した文字数を出力します。                                       | 
| clipcount --split   | 空白文字（改行、半角空白、全角空白、タブ）を削除した文字数を出力します。 | 
| clipcount -m        | 半角英数を0.5文字として換算して、文字数を出力します。                    | 


CUIツールとして使用し、文字数をターミナル上に出力できます。
```PowerShell
clipcount [オプション]
```

また、Pythonファイル上でインポートできます。
注意点として、`-`や`--`を排除してオプションを渡してください。
```python
from clipcount import clipcount
x = clipcount({オプション})
```



### 使用例（Example）
*※以下の例は「Windows」環境で実行しているので、改行コードを「`\r\n`」としてカウントしています。*

以下の文章をコピーした上で、clipcountを実行します。
```markdown
clipboardの
文字　を読み込みます。
```

#### CUI (ターミナルで使用)
- そのまま実行する
```PowerShell
# 空白文字を含めた全ての文字数を出力
> clipcount
23
```
- 半角空白を削除して出力
```PowerShell
# 上記の空白は全角空白なので何も変わらない
> clipcount -s
23
```
- 全角空白を削除して出力
```PowerShell
# 全角空白が1文字削除されて出力
> clipcount -S
22
```
- 改行コードを削除して出力
```PowerShell
# 改行コードが削除されて出力
> clipcount -b
21
```
- 空白文字全てを削除して出力
```PowerShell
# 空白文字が全て削除されて出力
> clipcount -sSbt
20
```
もしくは`--split`を使っても同じ結果がでる。
```PowerShell
# 空白文字が全て削除されて出力
> clipcount --split
20
```
- 半角英数を0.5文字と換算して出力
```PowerShell
# 半角英数は0.5文字になる
> clipcount -m
17.5
```
- 空白文字全てを削除して、半角英数を0.5文字に換算
```PowerShell
> clipcount -m --split
15.5
```

#### インポート (Pythonで使用)
clipcount関数をimport出来ます。

```python
from clipcount import clipcount
x = clipcount({"--split"})
print(x)
```
```PowerShell
# 「clipcount --split」と同じ結果となる
> python hoge.py
20
```

注意点として、importで使用する際は `-sSbt` のように複合させず、**1つずつ分けてセットで渡す必要があります**。

```python
from clipcount import clipcount

# 正しい使い方（オプションは1つずつセットにする）
x = clipcount({"-s", "-S", "-b", "-t"})
print(x)

# NG：複合オプションとして渡すと正しく判定されません
y = clipcount({"-sSbt"})
print(y)
```
```PowerShell
> python hoge.py
20 # clipcount -sSbt として出力
23 # オプションが機能せず、文字数をそのまま出力
```


## 解説（Usage）
`clipcount` は、クリップボードの文字数をカウントする Python パッケージです。

デフォルトでは、改行、半角空白、全角空白、タブなどの空白文字も含めて文字数を数えます。また、半角文字と全角文字の区別はせず、すべて **1文字として換算** します。

必要に応じて、空白文字を削除して文字数をカウントすることもできます。例えば、改行だけ削除したい場合は `-b`、半角スペースだけ削除したい場合は `-s`、全角スペースだけ削除したい場合は `-S`、タブだけ削除したい場合は `-t` といった具合です。

空白文字をまとめてすべて削除したい場合は `--split` オプションを使います。このオプションは `-b -s -S -t` と同じ意味になります。

さらに、半角文字と全角文字を区別してカウントしたい場合は `-m` オプションを使います。このオプションでは、半角文字を **0.5文字として換算** し、全角文字は 1文字として換算します。WordPress のタイトルやメタキーワードの文字数を気にする場面などで便利です。

ターミナルでオプションを組み合わせて使うことも可能です。例えば、全角空白とタブだけを削除して文字数をカウントしたい場合は、次のように実行します。

```PowerShell
> clipcount -St
```

Python から `import` して使うこともできます。この場合は、オプションをまとめて文字列として渡すのではなく、**1つずつセットにして渡す**必要があります。例えば次のように書きます。

```python
from clipcount import clipcount

x = clipcount({"--split"})
print(x)
```

注意点として、import から使用する場合は `-sSbt` のように複合オプションとして渡すことはできません。必ず 1つずつセットで渡してください。また、オプションはすべてハイフンで始める必要があります。ハイフンで始まらない文字列を渡すとエラーになります。

## 使用しているパッケージ (Dependencies)
- [pyperclip](https://github.com/asweigart/pyperclip)

## 開発日記（dev-diary）
- [【個人開発】文字数を計測できる「clipcount」 | ゆすノート](https://yusu79.com/dev-clipcount/)
