# Interceptorsを使って、thenやcatchの前に共通で行いたい処理を書く

```js
// サーバーにいく前の処理
// use(成功した時の処理、失敗した時の処理)
axios.interceptors.request.use(
  config => {
    return config;
  },
  error => {
    // Main.jsのcatchに返す
    return Promise.reject(error);
  }
);

// サーバーから返ってきたものを受け取る前の処理
axios.interceptors.response.use(
  response => {
    return response;
  },
  error => {
    // Main.jsのcatchに返す
    return Promise.reject(error);
  }
);
```
