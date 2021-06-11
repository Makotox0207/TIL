# Array.metod 

## Array.filter
与えられた関数によって実装されたテストに合格したすべての配列からなる新しい配列を生成
```js
const scores = [10, 20, 30, 40];

const newScores = scores.filter((value) => {
  return value >= 30
});

console.log(newScores);
// => [30, 40]
```

## Array.find
提供されたテスト関数を満たす配列内の 最初の要素 の 値 を返す
```js
const members = ['本田', '香川', '長友'];

const member = members.find((value) => value === '長友' );

console.log(member);
// => 長友
```

## Array.map 
与えられた関数を配列のすべての要素に対して呼び出し、その結果からなる新しい配列を生成
```js
const userList = [10, 20, 30, 40];

const userIdList = userList.map((value) => {
  return `user_${value}`;
});

console.log(userIdList);
// => ["user_10", "user_20", "user_30", "user_40"]
```
