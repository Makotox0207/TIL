# 要素タグ/属性の追加

[![Image from Gyazo](https://i.gyazo.com/c1f8c48ef1f3153ae248793804b1b3c7.png)](https://gyazo.com/c1f8c48ef1f3153ae248793804b1b3c7)

## 属性の追加
- 取得した要素に「.」で繋げて属性を追加できる
- setAttributeよりもこっちのほうがベスト
- `要素.属性`で追加
- `要素.setAttribute`でも追加でできる
```js
const input = document.getElementById('test');
// elementはタグによって色々ある

// 取得した要素に「.」で繋げて属性を追加できる
// こっちでつけた方が推奨されている
input.placeholder = "あああ";
input.name = 'test';
//=> <input id="test" placeholder = "あああ" name = 'test'>

// setAttributeでも属性を追加できる
input.setAttribute('type', 'text');
//=> <input id="test" type="text" placeholder = "あああ" name = 'test'>
console.log(input);
```

## タグの中の文字の追加
- `要素.textContent`で追加
```js

input.textContent = 'テストです';
//=> <input id="test" type="text" placeholder = "あああ" name = 'test'>テストです</input>

console.log(input);
```

## タグの追加
- `document.createElement('a')`で追加
```js
const anchor = document.createElement('a');
anchor.href = 'https://google.com';
anchor.target = '_blank';
//=> <a href = 'https://google.com' target = '_blank'></a> 

console.log(anchor);
```
この状態だけでは、aタグは作れているがHTMLに反映されていない

[![Image from Gyazo](https://i.gyazo.com/1d1d71ac4d83b26408f8c594a0347cda.png)](https://gyazo.com/1d1d71ac4d83b26408f8c594a0347cda)

## element.appendChild
- 動的にWebサイトに要素を追加できる。
- 特定の親ノードの子ノードの最後にノードを追加することができる。
- 追加しようとしたノードが既に存在していたら、既存のノードが新しいノードで置き換わる。

```html
<body>
  <div id = "grand-father">
    <div id = "parrent">
      <div id = "target">
        自分
      </div>
    </div>
  </div>
  <div id = "divlist">
    <div class="div1">テスト1</div>
    <div class="div2">テスト2</div>
    <div class="div3">テスト3</div>
  </div>
</body>
```
```js
const target = document.getElementById('target');
const newDiv = document.createElement('div');
newDiv.id = 'test';
newDiv.classList.add('red');
newDiv.textContent = 'テスト';

// つけたい場所(target)を指定してその場所につける
target.appendChild(newDiv);

// 同じ階層に作るにはparentNodeを挟む
target.parentNode.appendChild(newDiv);


// 同じ階層にある要素の間に作る方法

const targetList = document.getElementById('divlist');
// 間に挟みたい箇所を指定する
const reference = document.querySelector('.div2');

// 新しくdivタグを作る
const newElement = document.createElement('div');
// クラス名を追加
newElement.classList.add('div4');
// テキストを追加
newElement.textContent = '追加しました';
// div2の前にnewElementを追加する
targetList.insertBefore(newElement, reference);
```

