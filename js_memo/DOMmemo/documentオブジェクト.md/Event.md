# イベントの追加

- ①と②は上書きされるので好ましくない

## ① HTMLタグにonclickと設定する

```html
<body>
  <div id="test"></div>
  <button id="target" onclick="change_color()">押してね</button>

  <script>
    function change_color(){
      const test = document.getElementById('test');
      test.textContent = 'テスト';
      test.classList.add('red');
    }
  </script>
</body>

```

## ② JSで設定 Element.onclick
```html
<body>
  <div id="test"></div>
  <button id="target">押してね</button> 

  <script>
    document.getElementById('target').onclick = function(){
      const test = document.getElementById('test');
      test.textContent = 'テスト';
      test.classList.add('red');
    }
  </script>
</body>
```

## ③ イベントリスナー EventTarget.addEventLister()
- コールバック関数(アロー関数)をつけてたくさん処理をつけられるので推奨
```html
<body>
  <div id="test"></div>
  <button id="target">押してね</button> 

  <script>
    const target = document.getElementById('target');

    target.addEventListener('click', ()=> {
      const test = document.getElementById('test');
      test.textContent = 'テスト';
      test.classList.add('red');
    });
  </script>
</body>
```

## バブリングとキャプチャリング

[![Image from Gyazo](https://i.gyazo.com/a0dcf0970b33259add39bc5583e30611.jpg)](https://gyazo.com/a0dcf0970b33259add39bc5583e30611)

1. イベントを見つけに行く(Capture Phase)
2. イベントが見つかる(Target Phase)
3. イベントが見つかると戻っていく(Bubbling Phase)
<br>
<br>
- 上に戻って行くときにイベントがあるとそれも実行してしまう
- これを止めるためにはバブリングを止める必要がある
- → `e.stopPropagation();`
<br>
<br>
- aタグを押すとリンクがとび自動でページが飛ぶ
- aタグの仕組みを止めたいときには、'e.prevendDefault();'

