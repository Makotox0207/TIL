# Vue-routerの準備

## Vue-routerのインストール

- `npm install vue-router`

## router.jsとviewsフォルダを作成

- viewsフォルダを新たに作る
  - 今回の場合だと、Home.vueとUsers.vue

- router.jsを新たに作成し、以下入力
```js
import Vue from "vue";
import Router from "vue-router";
import Home from "./views/Home.vue";
import Users from "./views/Users.vue";

// Vue.use() Vueのプラグインを使う
Vue.use(Router);

// export defalt, main.jsで読み込む  vue-routerを読み込む
export default new Router({
  routes: [{path: "/", component: Home}, {path: "/users", component: Users}]
});
```

## main.jsで読み込む

- `main.js`でルータを読み込む
```js
import Vue from 'vue';
import App from './App.vue';
import router from './router';

Vue.config.productionTip = false;
Vue.filter("upperCase", function(value){
  return value.toUpperCase();
});

new Vue({
  router: router,
  render: h => h(App),
}).$mount('#app')

```
## App.vueで表示

- `<router-view></router-view>`でルータで設定した物をURLごとに表示できる(動的コンポーネントになる)
- しかし、同じindex.htmlを返す→jsファイルで中身をURLを解析して表示するという流れになっている
```js
<template>
  <router-view></router-view>
</template>

<script>
</script>
```
