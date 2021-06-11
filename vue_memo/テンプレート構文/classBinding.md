# クラスをデータにバインディング

1. オブジェクトを渡し、真偽値を使って動的に適応させるクラスを切り替える
2. 配列を使って、適応させたいクラスを並べる

## ①オブジェクトを渡し、真偽値を使って動的に適応させるクラスを切り替える

```html
<body>
  <script src="vue.js"></script>
  <div id="app">
    <h1 :class="classObject">Hello World</h1>
    <button @click="isActive = !isActive">切り替え</button>
  </div>
<script>
  new Vue({
    el: '#app',
    data: {
      isActive: true,
    },
    computed: {
      classObject: function(){
        return { 
          red: this.isActive,
          'bg-blue': !this.isActive
        }
      }
    }
  })
</script>
```

## ②配列を使って、適応させたいクラスを並べる
```html
<body>
  <script src="vue.js"></script>
  <div id="app">
    <h1 :class="classObject">Hello World</h1>
    <button @click="isActive = !isActive">切り替え</button>
    <!-- 配列を使って適応させたいクラスを並べる -->
    <h1 :class="[{red: isActive}, bg]">Hello World!!</h1>
  </div>
<script>
  new Vue({
    el: '#app',
    data: {
      isActive: true,
      bg: 'bg-blue'
    },
    computed: {
      classObject: function(){
        return { 
          red: this.isActive,
          'bg-blue': !this.isActive
        }
      }
    }
  })
</script>
```
