# mapMutaionsヘルパーで、効率よくmutationsをコンポーネントに追加する

```html
<template>
  <div>
    <h3>Home</h3>
    <button @click="toUsers">Usersのページに行く</button>
    <hr>
    <h3>vuexの練習</h3>
    <!-- commitで本来渡すvalueはここで渡す -->
    <button @click="increment(1)">増加</button>
    <button @click="decrement(1)">減少</button>
  </div>
</template>

<script>
import { mapMutations } from 'vuex';
export default {
  methods: {
    // スプレッド演算子(...) 配列やオブジェクトの値を展開するオペレーター
    // const foo = [1, 2];
    // const bar = [3, 4]; 
    // console.log([...foo, ...bar]) // => [1, 2, 3, 4]
    ...mapMutations(['increment', 'decrement']),
    toUsers(){
      this.$router.push({
        name: 'users'
      });
    }
  }
}
</script>

```

## mapActionsヘルパーを使って、効率よくActionsをコンポーネントで使用する

```html
<template>
  <div>
    <h3>Home</h3>
    <button @click="toUsers">Usersのページに行く</button>
    <hr>
    <h3>vuexの練習</h3>
    <button @click="increment(1)">増加</button>
    <button @click="decrement(1)">減少</button>
  </div>
</template>

<script>
import { mapActions } from 'vuex';
export default {
  methods: {
    ...mapActions(['increment', 'decrement']),
    // actionsのincrementをdispatchで実行する
    // increment(){
    //   this.$store.dispatch('increment', 1);
    // },
    // decrement(){
    //   this.$store.dispatch('decrement', 1);
    // },
  }
}
```

