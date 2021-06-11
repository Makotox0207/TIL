# Objectオブジェクト

- `const test = new Object();`でも書けるが`const test = {};`としても書ける

```js
const test = {
  test1:10,
  test2:20,
  test3:30
};

console.log(test);

// {
//   "test1": 10,
//   "test2": 20,
//   "test3": 30
// }
```

## Object.values
指定されたオブジェクトが持つ列挙可能なプロパティの値を、 for...in ループで提供される場合と同じ順序で配列にして返す
```js
console.log(Object.values(test));
// => [10, 20, 30]

for(value of Object.values(test)){
  console.log(value);
};
// 10
// 20
// 30
```

## 組み込み関数 in ユーザー定義関数
- 関数名はcamelCaseまたはsnack_caseで名前をつける.
- jsは主にcamelCase
- 動詞 + 名詞
```js
const postalCodde = '123-4567';
const postalCodde_2 = '123-45678';
function checkPostalCode(string){
  const replaced = string.replace('-', '');
  const length = replaced.length;

  if(length === 7){
    return true;
  }
  return false;
}

console.log(checkPostalCode(postalCodde));
// => true
console.log(checkPostalCode(postalCodde_2));
// => false
```

## オブジェクトにメソッドや値を組み込む(this)

```js
const postalCode = '123-4567';
const postalCode_2 = '123-45678';

const postal = {

  postalCode:'123-4567',

  checkPostalCode(string){
    
    const replaced = string.replace('-', '');
    const length = replaced.length;

    if(length === 7){
      return true;
    }
    return false;
  }
};

console.log(postal.checkPostalCode(postalCode));
// => true
console.log(postal.checkPostalCode(postalCode_2));
// => false
console.log(postal.postalCode);
// => 123-4567
```

### this
- thisはオブジェクト自体
- `this.postalCode`みたいにすると、オブジェクトの中の値を取ってこれる

```js
const postal = {

  postalCode:'123-4567',
  checkPostalCode(){
    
    const replaced = this.postalCode.replace('-', '');
    const length = replaced.length;

    if(length === 7){
      return true;
    }
    return false;
  }
};

// オブジェクトの中のpostalコードを使って判定を行う
console.log(postal.checkPostalCode());
// => true
```
