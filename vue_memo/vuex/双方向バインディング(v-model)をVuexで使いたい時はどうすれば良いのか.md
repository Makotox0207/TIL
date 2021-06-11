# 双方向バインディング(v-model)をVuexで使いたい時はどうすれば良いのか
```html
<template>
<div>
  <p>v-model: {{message}}</p>
  <input type="text" v-model="message">
</div>
</template>

<script>
import {mapGetters} from "vuex";

export default {
  computed: {
    message: {
      // computedはデフォルトではゲッターの役割しかないので、セッターを用意する
      get(){
        return this.$store.getters.message;
      },
      set(value){
        // dispatch  actions→mutation→state書き換え
        this.$store.dispatch('updateMessage', value);
      }
    },
    count(){
      return this.$store.state.count;
    }
  }
}
</script>
```

```js
// store.js

export default new Vuex.Store({
  state: {
    message: ""
  },
  getters: {
    // stateの算出プロパティを用意
    message: state => state.message
  },
  mutations: {
    // actionからコミットされ受け取ったnewMessageをstate.messageに代入
    updateMessage(state, newMessage){
      state.message = newMessage;
    }
  },
  actions: {
    // セッターで値を変更した時にupdateMessageをコミット
    updateMessage({ commit }, newMessage){
      commit('updateMessage', newMessage);
    }
  }
});

```
