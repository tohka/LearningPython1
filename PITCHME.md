# Learning Python

---

## はじめに

---

### 進め方と目標

- 前半は文法等を説明。後半はサンプルプログラムを元に説明。
- 手を動かすのは、後日、休日の日にでも各自で。
- 完全に理解しようとしない。
- Python でどんなことができるのか漠然と理解する。
- リファレンス（マニュアル）の使い方を覚える。

---

### 環境のインストール

いろいろな方法があるが、ここでは敢えて Windows 版 QGIS 3.x をインストールしたときに一緒にインストールされる Python 3.x を使うことにする（新規にインストールはしない）。

Mac や Linux は簡単にインストールすることができる、あるいはデフォルトでインストール済みなので、そのまま使おう。

---

### OSGeo4W Shell から Python の実行

スタートメニューから「 QGIS 3.6 」→「 OSGeo4W Shell 」を実行すると OSGeo4W Shell が起動される。

OSGeo4W Shell 自体はコマンドプロンプトだが、検索パスに `C:\Program Files\QGIS 3.6\bin` の追加等の設定がなされており、 GDAL/OGR などのコマンドが使用できるようになっている。

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

`python3` あるいは `python` コマンドを引数なしで実行すると対話モードで起動される。ちょっとした動作の確認や、簡単なスクリプトの実行なら対話モードでよい。対話モードの Python を終了するには `quit()` を実行する。

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

## 文法等

