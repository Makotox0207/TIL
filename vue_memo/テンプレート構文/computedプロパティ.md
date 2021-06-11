# computedプロパティ

- データはあくまでも初期値しか扱わないので、動的なものを表現できない
- methodsで複雑な処理を{{}}に表示してしまうと、再描画された時に毎回実行されてしまうが、computedプロパティの場合だと表示されているかつ、プロパティ内部で参照している値が変更したときのみ実行される。
- そのため、イベントが発生した際には毎回methodであれば実行されてしまい動かしたくない場合でもうごいてしまうが、computedであれば特定の値が実行された時のみ実行されるので、基本的にはcomputedを使う。

```html
<body>
  <script src="vue.js"></script>

  <div id="app">
    <p>{{counter}}</p>
    <p>{{ lessThanThree }}</p>
    <button @click="counter += 1">up</button>
    <button @click="counter -= 1">down</button>
  </div>
  <script>
    new Vue({
	    el: '#app',
      data: {
      	counter: 0
      },
      computed: {
        lessThanThree: function (){
          return this.counter > 3 ? '3以上' : '3以下';
        }
      }
    })
  </script>
  
</body>
</html>

```
