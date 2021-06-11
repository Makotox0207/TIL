# マップオブジェクト

[リファレンス](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Map)
```javascript
const myMap = new Map();

myMap.set('id', 3);
myMap.set('name', '本田');

console.log(myMap);

// {"key": "id","value": 3},
// {"key": "name","value": "本田"}

console.log(myMap.get('name'));
// => 本田

const keyList = myMap.keys();

for (key of keyList){
  console.log(key);
}
// => id
// => name

const valueList = myMap.values();

for (value of valueList){
  console.log(value);
}

// => 3
// => 本田

```

