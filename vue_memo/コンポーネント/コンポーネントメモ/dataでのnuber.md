```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Component</title>
</head>
<body>
  <script src="vue.js"></script>
  <div id="app">
    <my-component></my-component>
    <my-component></my-component>
    <my-component></my-component>
    
  </div>
  <script>
    // returnでdataを全て共有しているため全て更新される
    var data = {
      number: 12
    }

    // Vue.component('componentの名前',{})
    Vue.component('my-component', {
      data: function(){
        // componentではデータは関数
        return data
      },
      template: '<p>いいね({{number}})<button @click="increment">+1</button></p>',
      methods: {
        increment: function(){
          this.number += 1;
        }
      }
    })
    new Vue({
      el: '#app'
    })
  </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Component</title>
</head>
<body>
  <script src="vue.js"></script>
  <div id="app">
    <my-component></my-component>
    <my-component></my-component>
    <my-component></my-component>
    
  </div>
  <script>
    // Vue.component('componentの名前',{})
    Vue.component('my-component', {
      // componentではデータは関数
      data: function(){
        // それぞれのコンポーネントのデータのメモリを別々のところに置いている
        return {
          number: 12
        }
      },
      template: '<p>いいね({{number}})<button @click="increment">+1</button></p>',
      methods: {
        increment: function(){
          this.number += 1;
        }
      }
    })
    new Vue({
      el: '#app'
    })
  </script>
</body>
</html>

```
