# axiosを使用してPOSTを送信する
0. 処理の流れ
1. ルーティングの設定
2. コントローラの設定
3. jbuilderでjson形式で出力
4. vue側で送信機能の設定

## 処理の流れ
1. 値の入力
1. ボタンを押してpostリクエストの発生
2. ルーティングで該当するコントローラのアクションに流す
3. コントローラ側で`views/api/memos/show.json.jbuider`にレンダリング
4. 入力された値をjson形式で出力

## 1. ルーティングの設定

```ruby
  namespace :api, format: 'json' do
    resources :memos, only: [:index, :create]
  #=> api_memos GET api/memos api/memos#index
  #=> api_memos POST api/memos api/memos#create
  end
```

## 2. コントローラの設定

- `render json: オブジェクト` オブジェクトからJSON形式への変換と、変換されたJSONをブラウザに送信する
```ruby
 def create
    @memo = Memo.new(memo_params)
    if @memo.save
      # 201 Created, リクエストが成功してリソースの作成が完了したことを表す
      render :show, status: 201
    else
      # 422 Unprocessable Entity, サーバーが要求本文のコンテンツ型を理解でき、要求本文の構文は正しいものの、中に含まれている指示が処理できなかったことを表す
      render json: @memo.errors, status: 422
    end
  end

  private

  def memo_params
    params.permit(:title, :description)
  end
end
```

## 3. jbuilderでjson形式に出力

```ruby
json.title @memo.title
json.description @memo.description
#=> {"title": "メモタイトル1", "description": "メモ詳細1"}
```

## 4. vue側で送信機能の設定

1. `v-model`  値が入力されると、それに該当するdataプロパティも自動的に更新される
2. `@click=addMemo()`で関数を実行
3. addMemo()の処理
	1. dataプロパティであるtitleとdescriptionを、POSTのurlである`/api/memos`に対して送信
	2. postが成功したら、投稿した情報を画面に表示

```html
<template>
  <div id="app">
    <!-- memosを表示する -->
    <ul>
      <li v-for="memo in memos" :key="memo.id">
        title:{{memo.title}}, description{{memo.description}}
      </li>
    </ul>
    <!-- v-model dataオプションの中で該当するプロパティを自動的に更新 -->
    <input type="text" v-model="title" placeholder="title">
    <input type="text" v-model="description" placeholder="description">
    <!-- 投稿ボタンをクリックしたら、フォームに入力した内容をAxiosを使ってPOST送信 -->
    <button @click="addMemo()">メモを追加</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      memos: [],
      title: "",
      description: ""
    }
  },
  // アプリが立ち上がった最初のタイミングで実行される関数を定義
  mounted(){
    this.setMemo();
  },
  // axiosでapi/memosのAPIを呼び出す
  methods: {
    setMemo: function(){
      axios.get('/api/memos')
      .then( response => {
        this.memos = response.data;
      })
    },
    addMemo: function(){
      axios.post('/api/memos', {
        title: this.title,
        description: this.description
      })
      .then( response => {
        // POST送信が成功したら、setMemo()関数を呼び出し、投稿した情報を画面に表示
        this.setMemo();
      })
    }
  }
}
</script>
```