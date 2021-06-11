# v-modelでフォームを作る

```html

<!-- // App.vue -->
<template>
  < style="padding: 10rem;">
    <label for="title">タイトル</label>
    <!-- インプットタグでデータバインディング -->
    <input id="title" type="text" v-model="formData.title">
    <p>{{formData.title}}</p>

    <!-- 配列で表示して複数のセレクトボックスを指定する -->
    <select v-model="formData.location">
      <option v-for="location in locations" :key="location">{{location}}</option>
    </select>
    <p>{{formData.location}}</p>
  </div>
</template>

<script>

import About from "./components/About.vue";
import Home from "./components/Home.vue";

export default {
  data(){
    return {
      locations: ["東京", "大阪", "札幌"],
      formData: {
        title: "タイトル",
        location: []
      }
    };
  }
}
</script>

<style>
</style>
```
