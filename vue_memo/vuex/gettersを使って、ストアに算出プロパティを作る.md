# gettersを使って、ストアに算出プロパティを作る
- Vuex用の算出プロパティ。

```js
// store.js
export default new Vuex.Store({
  state: {
    count: 0
  },
  getters: {
    // Vuex側でcountを2倍するものという処理を返す
    doubleCount: state => state.count * 2
  }
});
```

```html
<!-- Home.vue -->
<template>
  <div>
    <h3>Home</h3>
    <hr>
    <h3>vuexの練習</h3>
    <button @click="increment">増加</button>
    <button @click="decrement">減少</button>
  </div>
</template>

<script>
export default {
  methods: {
    increment(){
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

```html
<template>
<div>
  <h1>HeaderHome</h1>
  <!-- Home.vueでのメソッドの処理も適応される -->
  <!-- 上は+1, 下は*2-->
  <p> {{count}} </p>
  <p> {{doubleCount}} </p>
</div>
</template>

<script>
export default {
  computed: {
    count(){
      return this.$store.state.count;
    },
    doubleCount(){
      return this.$store.getters.doubleCount;
    }
  }
}
</script>

```
