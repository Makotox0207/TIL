# postメソッドを使ってデータをサーバーに送る

```html
<template>
  <div id="app">
    <h3>掲示板に投稿する</h3>
    <label for="name">ニックネーム：</label>
    <input id="name" type="text" v-model="name">
    <br><br>
    <label for="comment">コメント：</label>
    <textarea id="comment" v-model="comment"></textarea>
    <h2>掲示板</h2>
    <br><br>
    <!-- クリックしたらサーバーにデータを送る -->
    <button @click="createComment">サーバーにコメントを送る</button>
  </div>
</template>

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
    methods: {
      createComment(){
        // axios.post(url, data, [options]) データをサーバに送る
        // promiseを返す
        axios.post('https://firestore.googleapis.com/v1/projects/vuejs-http-40386/databases/(default)/documents/comments',
        {
          fields: {
            name: {
              stringValue: this.name
            },
            comment: {
              stringValue: this.comment
            }
          }
        }
        )
        // axios.postが成功したら送った結果が返ってくる
        .then(response => {
          console.log(response);
        })
        // axios.postで失敗した場合の処理
        .catch(error => {
          console.log(error);
        });
      }
    }
  }
</script>
```
