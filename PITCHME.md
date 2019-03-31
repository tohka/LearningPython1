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

C:\>PATH C:\PROGRA~1\QGIS3~1.6\apps\Python37;C:\PROGRA~1\QGIS3~1.6\apps\Python37
\Scripts;C:\PROGRA~1\QGIS3~1.6\apps\Python27\Scripts;C:\PROGRA~1\QGIS3~1.6\bin;C
:\Windows\system32;C:\Windows;C:\Windows\system32\WBem

C:\>python3
Python 3.7.0 (v3.7.0:1bf9cc5093, Jun 27 2018, 04:59:51) [MSC v.1914 64 bit (AMD6
4)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

---

### Python の対話モード

`python3` あるいは `python` コマンドを引数なしで実行すると対話モードで起動される。ちょっとした動作の確認や、簡単なスクリプトの実行なら対話モードでよい。

対話モードの Python を終了するには `quit()` を実行する。

```
C:\>python3
Python 3.7.0 (v3.7.0:1bf9cc5093, Jun 27 2018, 04:59:51) [MSC v.1914 64 bit (AMD6
4)] on win32
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

プロンプト（ `>>> ` ）のあとに式を書いてエンターキーを押下すると、式が評価される。次の例では評価された結果が `3` 。

```
C:\>python3
Python 3.7.0 (v3.7.0:1bf9cc5093, Jun 27 2018, 04:59:51) [MSC v.1914 64 bit (AMD6
4)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> 1 + 2
3
>>> 
```

Python は制御構文等を使用する際にインデントを用いる。その場合はプロンプトが `... `と表示される。
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

`print()` 関数を使い、値を出力してもよい。対話モードだと `print()` は不要だが、スクリプトファイルから実行する場合、デバッグとしてよく使用する。

```python
>>> print(1 + 2)
3
>>> 
```

`print()` 関数自体の返り値は `None` であり、式を評価した結果が表示されているわけではなく、 `print()` の副作用である出力によるもの。

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

Python 3.x は `整数 / 整数` の演算は浮動小数点数（実数）が返るが、 Python 2.x や C 言語、 Ruby などは整数が返るので注意が必要。逆に言えば、 Python 3.x では整数と浮動小数点数の違いは意識しなくてよい。整数で返してほしい場合は `//` を用いる。

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

--- 

### 整数と浮動小数点数

コンピュータ内部での取扱いが異なる。

Python 3.x では整数型は `int` 型（ `numbers.Integral` クラス）、浮動小数点数型は `float` 型（ `numbers.Real` クラス）として扱われる。ただし、前述のとおりあまり意識しなくてもよい。（ `int` はビット演算が可能だが `float` は不可といった違いはあるが）

```python
>>> type(1)
<class 'int'>
>>> type(1.5)
<class 'float'>
>>> type(4/2)
<class 'float'>
```

---

### 数値の丸め

数値を丸めるには `round()` を使う。ただし、 Python 3.x では四捨五入ではなく `N.5` に対して偶数側に丸める（銀行丸め、 ISO 丸め、 JIS 丸め等と呼ばれる）。

```python
>>> round(1.5)
2
>>> round(2.5)
2
>>> round(3.5)
4
>>> round(4.5)
4
>>> round(5.5)
6
```

