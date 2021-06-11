# 要素の取得

## document.getElementById()
- 返り値はelement
- idを取れる
```js
const test = document.getElementById('test');
console.log(test);

// => <div id = "test">テスト</div>

// <body>
//   <div id="test">テスト</div>
//   <script src = "getElementById.js">
//   </script>
// </body>
```

## document.querySelector()
- 返り値はelement
- idだけでなく、classやdivも取れる
- 同じクラス名の要素が複数ある場合、一番上のものが取れる

```js
const test_2 = document.querySelector('#test');
const testList = document.querySelector('.test_list');

console.log(test_2);
// => <div id = "test_2">テスト</div>

console.log(testList);
=// > <div id="test" class="test_list"> 
```

## document.querySelectorAll()
- 返り値はNodeList


