# アロー関数

- 基本的な書き方
  - `const 関数名 = (引数) => {処理 戻り値};`
- 引数が一個なら()も不要 (0個なら省略できない)
  - `const test = test => test;`
- 処理が1行なら{}不要 (returnも省略できる)
  - `const tax = (price, tax) => price * tax;`

```javascript
  // 関数の名前あり
const getItem = () => {console.log('アロー')};
getItem();
// => アロー

const fruits = [
  'りんご','バナナ', 'みかん'
 ]; 
fruits.forEach(input => console.log(input));
  ```
