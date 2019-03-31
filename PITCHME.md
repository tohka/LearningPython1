# Learning Python

---

### 進め方と目標

- 前半は文法等を説明。後半はサンプルプログラムを元に説明。
- 手を動かすのは、後日、休日の日にでも各自で。
- 完全に理解しようとしない。
- Python でどんなことができるのか漠然と理解する。
- リファレンス（マニュアル）の使い方を覚える。
- 主にテキスト処理をできるようにする。（ CSV とか）

---

### リファレンス

- python Japan https://www.python.jp/
  - Python 3.7.3 ドキュメント https://docs.python.org/ja/3/
    - ライブラリリファレンス https://docs.python.org/ja/3/library/
    - 言語リファレンス https://docs.python.org/ja/3/reference/

---

### 環境のインストール

いろいろな方法があるが、ここでは敢えて Windows 版 QGIS 3.x をインストールしたときに一緒にインストールされる Python 3.x を使うことにする（新規にインストールはしない）。

Mac や Linux は簡単にインストールすることができる、あるいはデフォルトでインストール済みなので、そのまま使おう。

---

### OSGeo4W Shell から Python の実行

スタートメニューから「 QGIS 3.6 」→「 OSGeo4W Shell 」を実行すると OSGeo4W Shell が起動される。

OSGeo4W Shell 自体はコマンドプロンプトだが、検索パスに `C:\Program Files\QGIS 3.6\bin` の追加等の設定がなされており、 GDAL/OGR などのコマンドが使用できるようになっている。

```
run o-help for a list of available commands
C:\>gdalinfo --version
GDAL 2.4.1, released 2019/03/15

C:\>
```

---

### OSGeo4W Shell から Python の実行

`python` (Python 2.7) や `python3` (Python 3.x) にもパスは通っているが、 Python 関係で必要な設定が不十分でエラーが発生する。

```
run o-help for a list of available commands
C:\>python -V
Python 2.7.14

C:\>python3 -V
Python 3.7.0

C:\>python3
Fatal Python error: initfsencoding: unable to load the file system codec
  File "C:\PROGRA~1\QGIS3~1.6\apps\Python27\lib\encodings\__init__.py", line 123

    raise CodecRegistryError,\
                            ^
SyntaxError: invalid syntax

Current thread 0x00000f90 (most recent call first):

C:\>
```

---

### OSGeo4W Shell から Python の実行

そこで `py3_env.bat` を実行すると、自動的に必要な設定が行われる。

```
run o-help for a list of available commands
C:\>py3_env

C:\>SET PYTHONPATH=

C:\>SET PYTHONHOME=C:\PROGRA~1\QGIS3~1.6\apps\Python37

C:\>PATH C:\PROGRA~1\QGIS3~1.6\apps\Python37;C:\PROGRA~1\QGIS3~1.6\apps\Python37\Scripts;C:\PROGRA~1\QGIS3~1.6\apps\Python27\Scripts;C:\PROGRA~1\QGIS3~1.6\bin;C:\Windows\system32;C:\Windows;C:\Windows\system32\WBem

C:\>python3
Python 3.7.0 (v3.7.0:1bf9cc5093, Jun 27 2018, 04:59:51) [MSC v.1914 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

4行目、6行目、8行目は `py3_env.bat` によって実行された内容であり、ユーザが入力するする必要はない。

---

### Python の対話モード

`python3` あるいは `python` コマンドを引数なしで実行すると対話モードで起動される。ちょっとした動作の確認や、簡単なスクリプトの実行なら対話モードでよい。

対話モードの Python を終了するには `quit()` を実行する。

```
C:\>python3
Python 3.7.0 (v3.7.0:1bf9cc5093, Jun 27 2018, 04:59:51) [MSC v.1914 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> quit()

C:\>
```

---

### Python スクリプトの実行

`python3 SCRIPT_FILE` を実行すると、スクリプトファイルが実行される。



---

### Python スクリプトの実行

QGIS を複数バージョンインストールしている場合、種々の Python 環境がインストール済みのため、関連付けを行ってダブルクリックで実行する方法はやめたほうがよい。そこでエクスプローラ上でスクリプトファイルをドロップすると適切に実行するバッチファイルを紹介する。


---

### 対話モードの使い方

プロンプト（ `>>> ` ）のあとに式を書いてエンターキーを押下すると、式が評価される。次の例では評価された結果が `3` であることを意味している。

```
C:\>python3
Python 3.7.0 (v3.7.0:1bf9cc5093, Jun 27 2018, 04:59:51) [MSC v.1914 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> 1 + 2
3
>>> 
```

Python は制御構文等を使用する際は複数行にまたがるが、その場合にはプロンプトが `... `となる。
```
C:\>python3
>>> if 1 > 0:
...     "positive"
... 
'positive'
>>> 
```

---

### 対話モードの使い方

`print()` 関数を使い、値を出力してもよい。対話モードだと `print()` は必ずしも必要ないが、スクリプトファイルから実行する場合はデバッグとしてよく使用する。

```python
>>> print(1 + 2)
3
>>> 
```

`print()` 関数自体の返り値は `None` であるため、式を評価結果自体は表示されない。 `3` と表示されているのは `print()` の副作用である出力によるもの。

```python
>>> v = print(1 + 2)
3
>>> v # None なので何も表示されない。空白行すら表示されない。
>>> v is None
True
>>> 
```

---

### 数値と四則演算

数学と同様、演算子には優先順位がある。優先順位が同等の場合、（一般的には）左から評価される。

Python 3.x は `整数 / 整数` の演算は浮動小数点数（実数）が返るが、 Python 2.x や C 言語、 Ruby などは整数が返るので注意が必要。逆に言えば、 Python 3.x では整数と浮動小数点数の違いは意識しなくてよい。

余剰を捨てて、整数値の結果が必要なときは `//` を用いる。

```python
>>> 1 + 2
3
>>> 1 + 2 * 3
7
>>> 1 + 2 * 3 / 4
2.5
>>> (1 + 2) * 3 / 4
2.25
>>> (1 + 2) * 3 // 4
2
```

- @size[0.8em](Python 3.7.3 ドキュメント　＞　ライブラリーリファレンス　＞　組み込み型　＞　数値型)
  - @size[0.8em](https://docs.python.org/ja/3/library/stdtypes.html#numeric-types-int-float-complex)

--- 

### 整数と浮動小数点数

コンピュータ内部での取扱いが異なるため、多くのプログラミング言語で区別される。

Python 3.x では整数型は `int` 型（ `numbers.Integral` クラス）、浮動小数点数型は `float` 型（ `numbers.Real` クラス）として扱われる。

`type()` 関数を使用すると、オブジェクトのクラスを確認できる。

```python
>>> type(1)
<class 'int'>
>>> type(1.5)
<class 'float'>
>>> type(4/2)
<class 'float'>
>>> type(4//2)
<class 'int'>
```

- @size[0.8em](Python 3.7.3 ドキュメント　＞　ライブラリーリファレンス　＞　組み込み型　＞　数値型)
  - @size[0.8em](https://docs.python.org/ja/3/library/stdtypes.html#numeric-types-int-float-complex)

---

### 数値の丸め

数値を丸めるには `round()` を使う。ただし、 Python 3.x では四捨五入ではなく `N.5` に対して偶数側に丸める（銀行丸め、 ISO 丸め、 JIS 丸め等と呼ばれる）。丸め手法として好ましいとされる手法であるが他言語との統一性はないため注意を要する。四捨五入は `math.floor(num + 0.5)` 等で代替可能。

```python
>>> round(1.5)
2
>>> round(2.5)
2
>>> round(3.5)
4
>>> round(-1.5)
-2
>>> round(-2.5)
-2
>>> round(-3.5)
-4
```

- @size[0.8em](Python 3.7.3 ドキュメント　＞　ライブラリーリファレンス　＞　組み込み型　＞　数値型)
  - @size[0.8em](https://docs.python.org/ja/3/library/stdtypes.html#numeric-types-int-float-complex)
- @size[0.8em](Python 3.7.3 ドキュメント　＞　ライブラリーリファレンス　＞　組み込み関数　＞　round)
  - @size[0.8em](https://docs.python.org/ja/3/library/functions.html#round)

---

### 数値の切り捨て、切り上げ

床関数 `floor()` や天井関数 `ceil()` は数学関係の関数や定数をパッケージングした `math` モジュールに含まれる。 `math` モジュールを使用するにはあらかじめ `import` する必要がある。

```python
>>> import math
>>> math.floor(1.5)
1
>>> math.floor(2.5)
2
>>> math.floor(-1.5)
-2
>>> math.ceil(1.5)
2
>>> math.ceil(2.5)
3
>>> math.ceil(-1.5)
-1
```

- @size[0.8em](Python 3.7.3 ドキュメント　＞　ライブラリーリファレンス　＞　組み込み型　＞　数値型)
  - @size[0.8em](https://docs.python.org/ja/3/library/stdtypes.html#numeric-types-int-float-complex)
- @size[0.8em](Python 3.7.3 ドキュメント　＞　ライブラリーリファレンス　＞　math)
  - @size[0.8em](https://docs.python.org/ja/3/library/math.html)

---

### 数値型に関する他の操作

主な他の操作として、余剰を返す `x % y` 、累乗を返す `x ** y`, `pow(x, y)` 、絶対値を返す `abs()` 等がある。

```python
>>> 42 % 11
9
>>> 3 ** 4
81
>>> pow(3, 4)
81
>>> abs(-21)
21
```

- @size[0.8em](Python 3.7.3 ドキュメント　＞　ライブラリーリファレンス　＞　組み込み型　＞　数値型)
  - @size[0.8em](https://docs.python.org/ja/3/library/stdtypes.html#numeric-types-int-float-complex)
- @size[0.8em](Python 3.7.3 ドキュメント　＞　ライブラリーリファレンス　＞　組み込み関数　＞　abs)
  - @size[0.8em](https://docs.python.org/ja/3/library/functions.html#abs)
- @size[0.8em](Python 3.7.3 ドキュメント　＞　ライブラリーリファレンス　＞　組み込み関数　＞　pow)
  - @size[0.8em](https://docs.python.org/ja/3/library/functions.html#pow)

---

### 文字列

Python 3.x における文字列は Unicode 列で、変更不可（ immutable ）なシーケンス型である。

文字列は一重引用符 `'` あるいは二重引用符 `"` で囲むことで表される。また改行文字 `\r` / `\n` やタブ文字　`\t` などの制御文字や引用符自身 `\'` / `\"` は `\` でエスケープすることで記述することができる。なお、 Python 3.x は一重引用符であってもエスケープされる。エスケープせずに `\` は `\` として扱いたい場合は `r"..."` のように `r` プレフィックスをつける。

```python
>>> "apple"
'apple'
>>> 'banana'
'banana'
>>> 'abc\tdef'
'abc\tdef'
>>> r'abc\tdef'
'abc\\tdef'
```

- @size[0.8em](Python 3.7.3 ドキュメント　＞　言語リファレンス　＞　リテラル　＞　文字列およびバイト列リテラル)
  - @size[0.8em](https://docs.python.org/ja/3/reference/lexical_analysis.html#string-and-bytes-literals)

---

### 文字列

このほか、引用符を三連で囲む方法 `''' ... '''` / `""" ... """` もある。この方法は、途中に改行文字を含めることも、引用符をエスケープなしで記述することができる。コメント（ドキュメンテーション）などや、複数行のコメントとしても利用される。

```python
>>> """
... "abc"
... 'def'
... """
'\n"abc"\n\'def\'\n'
```

- @size[0.8em](Python 3.7.3 ドキュメント　＞　言語リファレンス　＞　リテラル　＞　文字列およびバイト列リテラル)
  - @size[0.8em](https://docs.python.org/ja/3/reference/lexical_analysis.html#string-and-bytes-literals)

---

### 文字コードと文字符号化方法

文字は、たとえば `a` であれば `97` 、 `あ` であれば `12354` といったように文字に数値を割り当てて管理されている。この対応のことを文字コードと呼び、様々な種類が存在する。しかし、そのコードを符号化する方法（エンコーディング）は更に複数の種類が存在する。

| 文字 | Unicode | UTF-8 | UTF-16 BE |
|:----:|--------:|------:|----------:|
|  a   | <tt>01100001</tt> | <tt>01100001</tt> | 00000000 01100001</tt> |
|  あ  | <tt>00110000 01000010</tt> | <tt>1110<u>0011</u> 10<u>000001</u> 10<u>000010</u></tt> | <tt>00110000 01000010</tt> |

---

変更不可であるため文字列の一部（たとえば3文字目）を変更することはできないが、文字列の一部を変更した新たな文字列を作成することはできる。

シーケンス型とは、順番をもった複数の要素を扱った型で、リストやタプル（後述）もシーケンス型にあたる。シーケンス型で利用できる関数等が用意されている。

