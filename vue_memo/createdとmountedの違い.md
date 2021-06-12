# createdとmountedの違い

## ライフサイクルフック
- createdとmountedは、それぞれライフサイクルフックの一つ(vueの初期化の中の決められたタイミングで実行される関数)
- ライフサイクルフックの中にデータを取得するプログラムを書くと、vueの起動中にデータを取得し、取得が完了するとブラウザにデータ内容を表示できる。
	- APIを利用して外部からデータを取得してブラウザに表示させたい場合、ライフサイクルフックの中に外部からデータを取得できる**axios**を使う

## createdとmountedの違い

- createdはDOMがまだ作られていない状態
- mountedではDOMが作成された直後の状態
```js
// this.$elはDOMの要素なのでDOMが作成されていないとthis.$elは存在しない。
new Vue({
  el: '#app',
  data: {
  },
  created : function(){
    console.log('created')
    console.log(this.$el)
  },
  mounted : function(){
    console.log('mounted')
    console.log(this.$el)
  }
})

```
createdではDOMが作成されていないので、`this.$el`はundefined。
mountedではDOMの作成が完成しているので`<div id=”app”></div>`が表示。
![createdとmountedでのthis.$elの状態](https://reffect.co.jp/wp-content/uploads/2019/06/mounted_created_diffrence.gif)

## createdの中身

```js
  el: '#app',
  data: {
    message : 'Hello World'
  },
  created : function(){
    this.message = 'Hello Vue'
    console.log('created')
    console.log(this)
  }
```
![createdの状態でVueインスタンスを確認](https://reffect.co.jp/wp-content/uploads/2019/06/created_vue_instance.gif)
- vueインスタンスを作成できている。
- this.messageをcreatedの中で上書きした値も反映されている。つまり、dataオブジェクトがリアクティブになっており、createdの中でAPIを使ってデータ取得を開始しても取得したデータを設定することができるのでcreatedの中で外部からデータを取得しても問題がない。
```js
 created :function(){
    axios.get('https://jsonplaceholder.typicode.com/usedrs')
		//データプロパティusersに外部サーバから取得したデータを代入
  		.then(response => this.users = response.data)
  }
```
## 参考
[vue.jsのcreatedとmountedの違いを目で見て理解](https://reffect.co.jp/vue/vue-js-created-mounted-diffrence)