# Array オブジェクト
[Array](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array)

```javascript
// const fruits = new Array();
const fruits = [ // シンタックスシュガー（こっちで使うことが多い）
 'りんご','バナナ'
]; 

console.log(fruits);
// =>  [ "りんご", "バナナ"]

// 配列の末尾に要素を追加する

fruits.push('みかん');
console.log(fruits);
// =>  [ "りんご", "バナナ", "みかん"]
```

## コールバック関数

- 引数のところに関数が入ることをコールバック関数

```javascript
// callback 引数が関数
// 与えられた関数を配列の各要素に対して一度ずつ実行する
fruits.forEach(function(input){
  console.log(input);
});

// => りんご
// => バナナ
// => みかん
```

