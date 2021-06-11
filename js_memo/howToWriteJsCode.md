
```js
const arry = [
  ["赤", "青", "黄"],
  ["緑", "紫", "白"]
];

console.log(arry);

// オブジェクト
const member = {
  'name': '本田',
  'age': 39,
  'hobby': 'soccer'
};

console.log(member);
console.log(member.hobby);

const member_2 = {
  '1kumi':{
    'honda':{
      'height':170,
      'hobby':'soccer'
    },
    'kagawa':{
      'height':165,
      'hobby':'soccer'
    },
  }
};

console.log(member_2['1kumi']['honda']);
// => {height: 170, hobby: "soccer"}
console.log(member_2["1kumi"].honda.height);
// => 170

// 条件分岐

const height = 90;
const height_2 = '90';

console.log(typeof height);
// => number
console.log(typeof height_2);
// => string

if(height==90){
  console.log('身長は' + height + 'cmです');
};
// => 身長は90cmです

if(height_2===90){
  console.log('身長は' + height + 'cmです');
};
// => 

const height_3 = 91;

if(height_3=== 90){
  console.log('身長は' + height_3 + 'です');
} else {
  console.log('身長は91cmではありません');
};
// => 身長は91cmではありません

const signal = 'yellow';

if (signal==='red'){
  console.log('stop');
} else if(signal === 'yellow'){
  console.log('pause');
} else {
  console.log('go');
};
// => pause

const speed = 61;
const signal_2 = 'blue';
if (signal_2 === 'blue'){
  if(speed >= 60){
    console.log('スピード違反');
  }
}
// => スピード違反

// 三項演算子
// if else
// 条件 ? 真 : 偽

const score = 80;
const comment = score > 80 ? 'good' : 'not good';
console.log(comment);
// => not good

```
