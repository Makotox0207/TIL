# vuexの準備

## 1. store.jsを用意して、main.jsで適応させる

```js
// store.js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    count: 2
  }
});

// main.js
import Vue from 'vue';
import App from './App.vue';
import store from  './store';

Vue.config.productionTip = false;

new Vue({
  store,
  render: h => h(App),
}).$mount('#app')
```

## $stateでどこでも適応させられる

```html
<!-- Home.vue -->
<template>
  <div>
    <h3>Home</h3>
    <button @click="toUsers">Usersのページに行く</button>
    <hr>
    <h3>vuexの練習</h3>
    <p> {{count}} </p>
    <button @click="increment">+1</button>
    <button @click="decrement">-1</button>
  </div>
</template>

<script>
export default {
  computed: {
    count(){
      return this.$store.state.count;
    }
  },
  methods: {
    increment(){
      // $store.state.count storeのデータを持ってくる
      this.$store.state.count++;
    },
    decrement(){
      this.$store.state.count--;
    },
    toUsers(){
      this.$router.push({
        name: 'users'
      });
    }
  }
}
</script>

```

