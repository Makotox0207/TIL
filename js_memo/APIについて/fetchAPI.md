# fetch API

## fetchとは
- Promiseがベースになっている
- options fetchとしてAPIサーバーにリクエストを送り、データとしてレスポンスが返ってくる
```js
const url = 'https://dog.ceo/api/breeds/image/random';
// 短期間に集中的にアクセスするのはNG 1~2秒は間隔を開けて使うようにする

const options = {
  method: 'GET'
}

const fetchTest1 = fetch(url, options);

console.log(fetchTest1);
// => Promise {<pending>}
// [[PromiseState]]: "fulfilled"
// [[PromiseResult]]: Response

// 返り値はPromise
// この状態では中身が確認できない

// APIがJSONで取得できるなら  response.json()でパースする
const fetchTest2 = fetch(url, options)
.then( response =>  response.json() );

console.log(fetchTest2);
// Promise {<pending>}
// [[PromiseState]]: "fulfilled"
// [[PromiseResult]]: Object
```

### パース
[![Image from Gyazo](https://i.gyazo.com/bd29d0b7ab300b9a02172da59c4d197f.png)](https://gyazo.com/bd29d0b7ab300b9a02172da59c4d197f)
- `resoponse.json()`は、resoponse.json.parseとなっており、json→objectになる

## データの取得を待ってから取得する

### データを取得できない

- responseの前にconsole.logが走っているため`undefined`
- →responseが返ってきてから処理をさせる必要がある(データの取得を待ってから処理をする)
- async, awaitを使う

```js
const url = 'https://dog.ceo/api/breeds/image/random';

const options = {
  method: 'GET'
}

const fetchTest2 = fetch(url, options)
.then( response =>  response.json() );

console.log(fetchTest2.message);
// => undefined
```

### async, await

```js
const url = 'https://dog.ceo/api/breeds/image/random';

const options = {
  method: 'GET'
}

function getDogImage(url, options){
  return fetch(url, options)
  .then( response =>  response.json() );
}

// getDogImageは同期関数なので、非同期関数で待たせる
// await getDogImageでfetch実行後に動かす
async function getImage(url, options){
  const response  = await getDogImage(url, options);
  // console.log(response.message);
  const ImageElement = document.createElement('img');
  ImageElement.src = response.message;
  document.body.appendChild(ImageElement);
}

getImage(url, options);
```

## エラー
- 処理がうまく行かなくても,fetchではokとみなしてしまうので、判定をかける

## okの場合

```js
// random→rando
const url = 'https://dog.ceo/api/breeds/image/random';
const options = {
  method: 'GET'
}

// fetchの返り値はPromiseオブジェクト
// 状態(ok/ng), それぞれの値
function fetchDogImage(url, options){
  return fetch(url, options)
  .then( response => {
    console.log(response.ok);
    console.log(response.status);
    if(response.ok){
      return response.json();
      // => true
      // => 200
    }

    // okじゃなかったらthrowで例外を作る
    throw new Error('エラーです');
  }).catch(e => console.log(e.message));
}
```

## エラーの場合

```js
// random→rando
const url = 'https://dog.ceo/api/breeds/image/rando';
const options = {
  method: 'GET'
}

// fetchの返り値はPromiseオブジェクト
// 状態(ok/ng), それぞれの値
function fetchDogImage(url, options){
  return fetch(url, options)
  .then( response => {
    console.log(response.ok);
    console.log(response.status);
    if(response.ok){
      return response.json();
    }

    // okじゃなかったらthrowで例外を作る
    throw new Error('エラーです');
  }).catch(e => console.log(e.message));
  // => false
  // => 404
  // => エラーです
}
```

