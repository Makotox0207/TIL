# windowオブジェクト

## 位置関係
- window
  - document
  - location

## windowオブジェクトとは
- ブラウザのタブごとにwindowオブジェクトがある
- グローバルオブジェクト

```js
    // window.console.log();
    // window.document.getElementById();
    // alert('ウィンドウです');
    // confirm('本当に削除しますか');
    //window.setInterval, setTimeout

    // 現在のスクロールしている位置を取得
    console.log(window.scrollY);

    window.addEventListener('scroll', ()=>{
      console.log(window.scrollY); // 処理が重くなる
    });
    
    // ページを読み込みしたら実行する
    window.addEventListener('load')

```
