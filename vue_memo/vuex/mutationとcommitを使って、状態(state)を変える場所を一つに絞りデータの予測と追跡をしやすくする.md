# mutationとcommitを使って、状態(state)を変える場所を一つに絞りデータの予測と追跡をしやすくする

- mutations
  - データの追跡性と予測性を高めるために、例えばmutationsでしかstateを変更しないというようにする。
  - コンポーネント上で値を書き換えている場合、参照しているのか値を書き換えているのかわかりにくいので、データを変更するうえで便利。

```js
// store.js

export default new Vuex.Store({
  state: {
    count: 0
  },
  // mutation データを書き換える処理
  mutations: {
    increment(state, number){
      state.count += number;
    },
    decrement(state, number){
      state.count -= number;
    },
  }
});

// Home.vue
<script>
export default {
  methods: {
    increment(){
      //  commit('mutationName', value) mutaionのメソッドに値を渡す
      this.$store.commit('increment', 1);
    },
    decrement(){
      this.$store.commit('decrement', 1);
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
