# DOM
[![Image from Gyazo](https://i.gyazo.com/2e40cf2590c11bd924b897703c0956ec.png)](https://gyazo.com/2e40cf2590c11bd924b897703c0956ec)

## DOMとは
- HTML や XML 文書のためのプログラミングインターフェイス
- ページを表現するため、プログラムが文書構造、スタイル、内容を変更することができる。
-  DOM は文書をノードとオブジェクトで表現して、プログラミング言語をページに接続できる。
- ウェブページを表現、保存、操作する方法。
- ウェブページを操作したり、作成したりするために用意されているすべてのプロパティ、メソッド、イベントは、オブジェクトにまとめられている。
- JavaScript のようなスクリプト言語から変更できる。
---
## データ型
### Document
  - 文書自身を表現する
### Node
  - 文書内にあるすべてのオブジェクトは何らかの種類のノード。
  HTML文書内では、すべてのノードが要素。
### Element
  - element 型は node に基づいている。

- DOMを使用するコードの大部分では、常にDOM内のノードを**elements**として参照するのが一般的。

## DOMオブジェクトの繋がり
[![Image from Gyazo](https://i.gyazo.com/68b700701d068b1c6539057eee9717c6.png)](https://gyazo.com/68b700701d068b1c6539057eee9717c6)

## DOMのよくある流れ
1. 変化させたい要素を指定する
    - `document.getElementByld('id名')` -> スピードが早い
    - `document.querySelector()` -> 万能
    - `document.querySelectorAll()` -> 万能

2. イベントを指定
    - `el.addEventListener(event, callback);`
      - event ... click, keydown, change など
      - callbackはアロー関数で指定する。`()=> {}`

3. 処理を書く
    - HTML要素を追加
      - `el.createElement('div')` ... つくって
      - `el.appendChild`/ `el.removeChild` ... くっつける
    - classをつける
      - `el.classList.add('class')`
      - `el.classList.remove(class)`

## 参考
- [js講座-dom](http://hakuhin.jp/js/dom.html)
