# keep-alive

- keep-aliveをつけると全ての全てのデータがキャッシュされる
- 該当するものに切り替えるとactivated
- 該当しないものに切り替えるとdeactivated

```vue
// About.vue
<template>
  <div>
    <p>about</p>
    <input>
  </div>
</template>
<script>
  export default{
    activated(){
      console.log('activated');
    },
    deactivated(){
      console.log('deactivated');
    }
  };
</script>

// Home.vue
<template>
  <p>home</p>
</template>


// app.vue
<template>
  <div>
    <button @click="currentComponent = 'Home'">Home</button>
    <button @click="currentComponent = 'About'">About</button>
    <!-- 動的にAboutまたはHomeに変更 -->
    <component :is="currentComponent"></component>
    <!-- 一緒 -->
    <About v-if="currentComponent === 'About'"></About>
    <Home v-if="currentComponent === 'Home'"></Home>
    <!-- keep-aliveをつけると全てのデータがキャッシュされる -->
    <keep-alive>
      <component :is="currentComponent"></component>
    </keep-alive>
  </div>
</template>

<script>

import About from "./components/About.vue";
import Home from "./components/Home.vue";

export default {
  data(){
    return {
      currentComponent: "Home"
    };
  },
  components: {
    About,
    Home
  }
}
</script>

<style>
</style>
```



