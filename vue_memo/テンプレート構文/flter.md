# filter

## filter グローバル登録
```js
// main.js
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false;
// Vue.filter("filterName", func(){})
Vue.filter("upperCase", function(value){
  return value.toUpperCase();
});

new Vue({
  render: h => h(App),
}).$mount('#app')
```

```html
<!-- Home.vue -->
<template>
<div>
<!-- {{data | filterName}} -->
  <p>{{ title | upperCase }}</p>
  <p>{{subtitle | upperCase }}</p>
</div>
</template>

<script>
  export default{
    data(){
      return {
        title: "Hello",
        subtitle: "World"
      }
    }
  }
</script>

```

## filter ローカル登録

```html
<!-- Home.vue -->
<template>
<div>
  <p>{{ title | lowerCase }}</p>
  <p>{{subtitle | lowerCase }}</p>
</div>
</template>

<script>
  export default{
    data(){
      return {
        title: "Hello",
        subtitle: "World"
      }
    },
    filters: {
      lowerCase(value){
        return value.toLowerCase();
      }
    }
  }
</script>

```
