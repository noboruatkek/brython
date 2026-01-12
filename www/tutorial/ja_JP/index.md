
この自習資料は、Pythonプログラム言語を使って、ウェブブラウザ上で動作するアプリケーションを作成する方法を説明しています。電卓プログラムの作成を例にして、説明します。
This tutorial explains how to develop an application that runs in the browser using the Python programming language. We will take the example of writing a calculator.

この資料の自習には、テキストエディタがインターネット接続が可能なブラウザと共に必要です。
You will need a text editor, and of course a browser with Internet access.

この自習資料では、自習者がHTML(一般的なWebページの構造、殆どのよく使われるタグ）、スタイルシート（CSS)およびPython言語についての基礎的な知識を持っていることを仮定しています。
The contents of this tutorial assume that you have at least a basic knowledge of HTML (general page structure, most usual tags), of stylesheets (CSS) and of the Python language.

まず、最初にテキストエディタを使って次の内容のHTMLページを作成しましょう。
In the text editor, create an HTML page with the following content:

```xml
<!doctype html>
<html>

<head>
    <meta charset="utf-8">
    <script type="text/javascript"
        src="https://cdn.jsdelivr.net/npm/brython@{implementation}/brython.min.js">
    </script>
    <script type="text/javascript"
        src="https://cdn.jsdelivr.net/npm/brython@{implementation}/brython_stdlib.js">
    </script>
</head>

<body>

<script type="text/python">
from browser import document

document <= "こんにちは！"


</body>

</html>
```

空のディレクトリを作成し、このページを __`index.html`__　として保存しましょう。このファイルをブラウザで閲覧するのに二つの方法があります。
In an empty directory, save this page as __`index.html`__. To read it in the browser, you have two options:

- 「ファイル／開く」メニューを利用する： これは最も簡単な方法です。この方法は、進んだユーザにとっては、 [ちょっとした制限](/static_doc/en/file_or_http.html) がありますが、このチュートリアルを実行するには十分な方法です。
- use the File/Open menu: it is the most simple solution. It brings [some limitations](/static_doc/en/file_or_http.html) for an advanced use, but it works perfectly for this tutorial
- チュートリアル用のウェブサーバーを立ち上げる。：例えば、python.org で提供されているPython言語を実行する環境が整っていれば、`python -m http.server` を作業用ディレクトリで実行しdてみましょう。サーバが立ち上がったら、ブラウザのアドレス欄に　_localhost:8000/index.html_　を入力します。
- launch a web server : for instance, if the Python interpreter available from python.org is available on your machine, run `python -m http.server` in the file directory, then enter _localhost:8000/index.html_ in the browser address bar


ページを開くと、ブラウザウィンドウには"こんにちは！"の文字が表示されるはずです。
When you open the page, you should see the message "Hello !" printed on the browser window.

ページの構造　(Page structure)
============================
さあ、ページの中身の詳細を見ていきましょう。 `<head>` ゾーンの中では　javascript　のフィルド__`brython.js`__をロードしています。：これはBrythonを実行する中心機関です。このページの中のPythonスクリプトを見つけ出し、実行します。この例ではこれをCDNから入手してい流ので、PCにこれをインストールする必要はありません。バージョン番号((`brython@{implementation}`))についての注意: 新しいバージョンのBrythonに合わせて更新する事が可能です。
Let's take a look at the page contents. In the `<head>` zone we load the script __`brython.js`__ : it is the Brython engine, the program that will find and execute the Python scripts included in the page. In this example we get it from a CDN, so that there is nothing to install on the PC. Note the version number (`brython@{implementation}`) : it can be updated for each new Brython version.

この　__`index.html`__　の中には次のスクリプトが記述されいます。
Our __`index.html`__ page embeds this script:

```python
from browser import document

document <= ""こんにちは !"
```
この部分は、標準的なPythonプログラムに従って、モジュール　 __`browser`__ (この例では、モジュールはBrythoの中心部分　 __`brython.js`__　と共に配布されている）をインポート(import)することからはじめれれています。このモジュールには、ブラウザの窓のなかに表示される構成要素を参照する `document` が定義されています。
This is a standard Python program, starting by the import of a module, __`browser`__ (in this case, a module shipped with the Brython engine __`brython.js`__). The module has an attribute `document` which references the content displayed in the browser window.
ドキュメントにテキストを追加する、即ちブラウザ中でテキストを表示するのに、Brythonでは次の文法を使います。
To add a text to the document - concretely, to display a text in the browser - the syntax used by Brython is

```python
document <= "こんにちは !"
```
記号 ｀<=` は左向きの矢印と考えて良いでしょう。：　ウェブの文書(document)は新しい要素、ここでは"こんにちは !"という文字列です、を受け取ります。後で見るように、Brythonではウェブ文書とのやりとりを標準的なDOMの文法を使って記述することが可能です。　Brythonではプログラムが冗長になり過ぎないように、いくつかの短縮記法(ショートカット)が用意されています。
You can think of the `<=` sign as a left arrow : the document "receives" a new element, here the string "Hello !". You will see later that it is always possible to use the standardized DOM syntax to interact with the page, but Brython provides a few shortcuts to make the code less verbose.

特にこの例では、記号 `<=` を使うことに躊躇する場合には標準的なDOM要素の `attach` メソッドを使えます。
For this specific case, those who are not at ease with the use of the operator `<=` can use the method `attach()` of DOM elements instead:

```python
document.attach("こんにちは !")
```

HTMLタグを使ったテキストの整形　Text formatting with HTML tags
============================================================
HTMLでは　__太字__　や　_イタリック体_ などの文字の整形に タグ( `<B>` : __太字__, `<I>` : _イタリック体_  など
)を使えます。(訳註：これらのタグを使うよりも 意味を持ったタグ　や　CSS  を使うことが推奨されています）
HTML tags allow text formatting, for instance to write it in bold letters (`<B>` tag), in italic (`<I>`), etc.

Brythonでは　それらのタグは　__`browser`__ パッケージ中の __`html`__ モジュールの中で、関数として利用可能となっています。　次の例をご覧ください：
With Brython, these tags are available as functions defined in module __`html`__ of the __`browser`__ package. Here is how to use it:

```python
from browser import document, html

document <= html.B("こんにちは !")
```

タグを入れ子にして使うこも可能です。
Tags can be nested:

```python
document <= html.B(html.I("こんにちは ! !"))
```
複数のタグや文字列を足し合わせることも可能です。
Tags can also be added to each other, as well as strings:

```python
document <= html.B("Hello, ") + "world !"
```
タグを表す関数の最初の引数は、文字列、数字、別のタグが許されます。Pythonの"イテラブル"なオブジェクト(リスト、内包「コンプリヘンション」, 生成子「ジェネレーター」）が最初の引数になることもあります：この場合には、繰り返しの中で生成される全ての要素がタグに追加されます。
The first argument of a tag function can be a string, a number, another tag. It can also be a Python "iterable" (list, comprehension, generator): in this case, all the elements produced in the iteration are added to the tag:

```python
document <= html.UL(html.LI(i) for i in range(5))
```
タグの属性値[アトリビュー」はキーワード付きの関数引数としてタグに渡されます。
Tag attributes are passed as keyword arguments to the function:

```python
html.A("Brython", href="http://brython.info")
```

電卓の描画
======================
電卓をHTMLのテーブルとして描画しましょう。
We can draw our calculator as an HTML table.

最初の行は結果を表示する領域です。リセットボタンが付いています。次の4行には  数字と演算記号のボタンが入ります。
The first line is made of the result zone, followed by a reset button. The next 4 lines are the calculator touches, digits and operations.

```python
from browser import document, html

calc = html.TABLE()
calc <= html.TR(html.TH(html.DIV("0", id="result"), colspan=3) +
                html.TD("C", id="clear"))

lines = ["789/",
         "456*",
         "123-",
         "0.=+"]
calc <= (html.TR(html.TD(x) for x in line) for line in lines)

document <= calc
```

Pythonの生成子を使う事で、読みやすさを損なうことなくプログラムのサイズを短くできます。
Note the use of Python generators to reduce the program size, while keeping it readable.

ここで電卓の見映えをよくするために `<TD>` タグに　スタイルシートを使って 装飾をつけましょう。
Let's add style to the `<TD>` tags in a stylesheet so that the calculator looks better:

```xml
<style>
*{
    font-family: sans-serif;
    font-weight: normal;
    font-size: 1.1em;
}
td{
    background-color: #ccc;
    padding: 10px 30px 10px 30px;
    border-radius: 0.2em;
    text-align: center;
    cursor: default;
}
#result{
    border-color: #000;
    border-width: 1px;
    border-style: solid;
    padding: 10px 30px 10px 30px;
    text-align: right;
}
</style>
```

エベントの取り扱い　Event handling
==========================================
次のステップは利用者が電卓のボタンを押した時に行うべき動作を起動することです。
The next step is to trigger an action when the user presses the calculator touches:

- ボタンの種類 :　実行される動作
- 数字と演算記号：数字あるいは演算記号を結果の領域に印刷します。
- `=` 記号: 操作を実行し、結果を表示部分に印刷します。入力が適切でない場合には、エラーメッセージを表示します。
- `C` ボタン: 表示領域をリセットします。
- for digits and operations : print the digit or operation in the result zone
- for the = sign : execute the operation and print the result, or an error message if the input is invalid
- for the C letter : reset the result zone

ページに表示されている要素を取り扱うためには、プログラムはまずこれらの要素への参照を入手する必要があります。
ボタンは `<TD>` タグを使って作成されましたので、これらの全てのタグへの参照を入手するためにh、次の文法を使います。
To handle the elements printed in the page, the program need first to get a reference to them. The buttons have been created as `<TD>` tags; to get a reference to all these tags, the syntax is

```python
document.select("td")
```
`select` メソッドに渡された引数は　_CSS セレクタ_ です。　よく使われる　_CSS セレクタ_ には： タグの名前("tag"),
要素の `id` 属性("#result") あるいは クラス属性(".classname")があります。`select()` の呼び出しの戻り値は常に要素のリストです。
The argument passed to the `select()` method is a _CSS selector_. The most usual ones are: a tag name ("td"), the element's `id` attribute ("#result") or its attribute "class" (".classname"). The result of `select()` is always a list of elements.

ページ上の要素に対して発生するエベントは正規化された名前を持っています。要素をクリックした場合には、"click"という名前のエベントが起動されます。このプログラムでは、このエベントは関数の実行を引き起こします。この　要素ーエベントー関数の関係は次の文法で定義されます。
The events that can occur on the elements of a page have a normalized name: when the user clicks on a button, the event called "click" is triggered. In the program, this event will provoque the execution of a function. The association betweeen element, event and function is defined by the syntax

```python
element.bind("click", action)

電卓プログラムでは、全てのボタンで "click" エベントに付いて同じ関数を関係づけます。
For the calculator, we can associate the same function to the "click" event on all buttons by:

```python
for button in document.select("td"):
    button.bind("click", action)
```
Pythonの文法規則に合わせるため、 `action()` 関数はこれより前にプログラムのどこかで定義されている必要があります。
このような "コールバック"関数は一つの引数を取ります。この引数はエベントを表現するオブジェクトです。
To be compliant to Python syntax, the function `action()` must have been defined somewhere before in the program. Such "callback" functions take a single parameter, an object that represents the event.

完全なプログラム　Complete program
================================================
ここで示すプログラムは最小版の電卓プログラムです。　最重要の部分は　関数 `action(event)` です。
Here is the code that manages a minimal version of the calculator. The most important part is in the function `action(event)`.

```python
from browser import document, html

# Construction de la calculatrice
calc = html.TABLE()
calc <= html.TR(html.TH(html.DIV("0", id="result"), colspan=3) +
                html.TD("C"))
lines = ["789/", "456*", "123-", "0.=+"]

calc <= (html.TR(html.TD(x) for x in line) for line in lines)

document <= calc

result = document["result"] # direct acces to an element by its id

def action(event):
    """Handles the "click" event on a button of the calculator."""
    # The element the user clicked on is the attribute "target" of the
    # event object
    element = event.target
    # The text printed on the button is the element's "text" attribute
    value = element.text
    if value not in "=C":
        # update the result zone
        if result.text in ["0", "error"]:
            result.text = value
        else:
            result.text = result.text + value
    elif value == "C":
        # reset
        result.text = "0"
    elif value == "=":
        # execute the formula in result zone
        try:
            result.text = eval(result.text)
        except:
            result.text = "error"

# Associate function action() to the event "click" on all buttons
for button in document.select("td"):
    button.bind("click", action)
```

結果　Result
==================
<iframe width="800", height="400" src="/gallery/calculator.html"></iframe>
