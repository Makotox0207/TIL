# getメソッドを使ってサーバーのデータを取得する

```html
<script>
// axiosをインポート
import axios from 'axios';
  export default {
    data(){
      return {
        name: "",
        comment: ""
      };
    },
    // 最初のロード時にデータを取得する
    created(){
      // axios.get(取得したいサーバーのurl, リクエストの設定)
      // promiseを返す
      axios.get('https://firestore.googleapis.com/v1/projects/vuejs-http-40386/databases/(default)/documents/comments',
      )
      .then( response => {
        console.log(response);
      });
    }
  }
</script>
```

## サーバから取得したデータを表示する

```html
<template>
  <div id="app">
  <!-- postにリストレンダリングする -->
    <div v-for="post in posts" :key="post.name">
      <!-- post.返ってきた値の場所 -->
      <div>name:{{post.fields.name.stringValue}}</div>
      <div>comment:{{post.fields.comment.stringValue}}</div>

    </div>
  </div>
</template>

<script>
// axiosをインポート
import axios from 'axios';
  export default {
    data(){
      return {
        name: "",
        comment: "",
        // リストレンダリングをするために配列を用意する
        posts :[]
      };
    },
    created(){
      axios.get('https://firestore.googleapis.com/v1/projects/vuejs-http-40386/databases/(default)/documents/comments',
      )
      .then( response => {
        // 返ってきた値をpostsに代入
        this.posts = response.data.documents;
      });
    }
  }
</script>

```
