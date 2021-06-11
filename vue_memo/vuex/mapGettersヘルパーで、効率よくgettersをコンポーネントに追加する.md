# mapGettersヘルパーで、効率よくgettersをコンポーネントに追加する

```js
// store.js
export default new Vuex.Store({
  state: {
    count: 0
  },
  getters: {
    // Vuex側でcountを2倍するものという処理を返す
    doubleCount: state => state.count * 2,
    tripleCount: state => state.count * 3
  }
});
```

```html
<!-- HeaderHome.vue -->
<template>
<div>
  <h1>HeaderHome</h1>
  <!-- Home.vueでのメソッドの処理も適応される -->
  <p> {{doubleCount}} </p>
  <p> {{tripleCount}} </p>
</div>
</template>

<script>
import {mapGetters} from "vuex";

export default {
  computed: mapGetters(["doubleCount", "tripleCount"])
}
</script>
```

## computedにmapGetters以外の処理も適応させる方法
- スプレッド構文 (...) を使うと、配列式や文字列などの反復可能オブジェクトを、0 個以上の引数 (関数呼び出しの場合) や要素 (配列リテラルの場合) を期待された場所で展開したり、オブジェクト式を、0 個以上のキーと値のペア (オブジェクトリテラルの場合) を期待された場所で展開したりすることができる。

```html
<!-- HeaderHome.vue -->
<template>
<div>
  <h1>HeaderHome</h1>
  <!-- Home.vueでのメソッドの処理も適応される -->
  <p> {{doubleCount}} </p>
  <p> {{tripleCount}} </p>
  <p> {{count}} </p>
</div>
</template>

<script>
import {mapGetters} from "vuex";

export default {
  computed: {
    // スプレッド演算子
    // この場合、他のcountもできる
    ...mapGetters(["doubleCount", "tripleCount"]),
    count(){
      return this.$store.state.count;
    },
  }
}
</script>
```

