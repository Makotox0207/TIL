# actionをdispatchして、非同期処理を含む処理を扱いながらミューテーションをコミットする

```js
// store.js
export default new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state, number){
      state.count += number;
    },
    decrement(state, number){
      state.count -= number;
    }
  },
  actions: {
    increment({commit}, number){
      // mutationsのincrementをコミットする
      // numberをmutationsのnumberに渡す
      commit('increment', number);
    },
    decrement({commit}, number){
      commit('decrement', number);
    }
  }
});
```

```js
// Home.vue
export default {
  methods: {
    // actionsのincrementをdispatchで実行する
    increment(){
      this.$store.dispatch('increment', 1);
    },
    decrement(){
      this.$store.dispatch('decrement', 1);
    },
    toUsers(){
      this.$router.push({
        name: 'users'
      });
    }
  }
}
```
