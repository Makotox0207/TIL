## elementオブジェクト

- Element.classList
- add()やremove()を用いてオブジェクトを変更する

```js

// addで足したいクラス名を記述する

const test = document.getElementById('test');
test.classList.add('red');

// 上と一緒
// idがテストのものを取得(返り値はelement)→classListで取得可能
const test = document.getElementById('test').classList.add('red');

// removeでクラスを取り除く
const testList = document.querySelector('.test_list');
testList.classList.remove('blue')
```
