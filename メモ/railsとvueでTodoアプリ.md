# railsとvueでTodoアプリ

- RailsでAPIを作り、APIへのリクエストと返ってきたデータの表示をVue.jsで行う
[![Image from Gyazo](https://i.gyazo.com/7e1087b5fd601052d03aaed34c423b26.png)](https://gyazo.com/7e1087b5fd601052d03aaed34c423b26)

## 要件
-   テキストボックスにタスクを入力して追加ボタンを押すとリストに追加されて表示される。
-   チェックボックスをチェックすると取り消し線が引かれる
-   削除ボタンを押すと削除

## 1. railsでapiを作る

### APIの処理を作る
#### ToDoリストにタスクを追加するためにモデルを作成
	- `rails g model Task name:string is_done:boolean`

#### 表示用のhomeとデータを返すAPI用のapi::tasksを追加。
```ruby
Rails.application.routes.draw do
  root to: 'home#index'

  namespace :api, format: 'json' do
    resources :tasks, only: [:index, :create, :destroy, :update]
  end
end
```

#### リクエストを受けたらデータを返すTasksのコントローラーを作成
	- `rails g controller Api::Tasks` 

```ruby
module Api
  class TasksController < ApplicationController
	# XHRでリクエストするAPIとして利用する場合は、CSRFトークンの検証ができないため、エラーになる。
	# そのためCSRFトークン検証をスキップする
    skip_before_action :verify_authenticity_token

    def index
      @tasks = Task.order('created_at DESC')
    end

    def create
      @task = Task.new(task_params)

      if @task.save
        render json: @task, status: :created
      else
        render json: @task.errors, status: :unprocessable_entity
      end
    end

    def destroy
      Task.find(params[:id]).destroy!
    end
	# toggle! インスタンスに保存されているbooleanの値を反転させて、データベースに保存する。
	# 処理成功時にtrueをreturnする。
    def update
      Task.find(params[:id]).toggle!(:is_done)
    end

    private def task_params
      params.require(:task).permit(:name, :is_done)
    end
  end
end
```

#### jbuilderでJSON形式の文字列のデータを作成
```ruby
# json.set! 属性をまとめられる
json.set! :tasks do
  # json.array! 全てのインスタンスが、配列に格納されたJSON形式の文字列のデータで返却する
  json.array! @tasks do |task|
    # json.extract! 第一引数に指定したインスタンス変数のデータをJSON形式の文字列で返却
    json.extract! task, :id, :name, :is_done, :created_at, :updated_at
  end
end

```
## 2. vueでフロント作成

### コンポーネント
- コンポーネントとは、名前付きの再利用可能な Vue インスタンス
- 再利用出来そうなパーツごとにコンポーネントを区切って実装する
- ex. ヘッダーとToDoリストを表示するボディ部分でコンポーネントを分けて実装する

```erb
# views/home.html.erb
<div id="app">
  <nav></nav>
</div>

<%# app/javascript/packs配下のtodo.jsファイルを読み込む%>
<%= javascript_pack_tag 'todo' %>

```

#### ヘッダーの作成
- ヘッダー部分のコンポーネントを用意する

```html
<!-- header.vue -->
<template>
  <h1>Todoリスト</h1>
</template>
```

- `views/home/index.erb`の`<navbar></navbar>`に`components/header.vue`をマウントして表示

```js
// todo.js
import Vue from 'vue';
import Header from './components/header.vue';

// views/home/index.erbの<navbar></navbar>にheader.vueをマウントして表示
var app = new Vue({
  el: '#app',
  components: {
    'navbar': Header
  }
});
```

#### Vueの読み込み設定
- **webpackはVue.jsの読み込み方がわからないので以下を実行**
```js
// /config/webpack/loaders/stylus.js
module.exports = {
  test: /\.styl$/,
  use: [
    'style-loader', 'css-loader', 'stylus-loader'
  ]
}
```

```js
// /config/webpack/environment.js

const { environment } = require('@rails/webpacker')
const { VueLoaderPlugin } = require('vue-loader')
const vue = require('./loaders/vue')
// 作ったstylusをrequireする
const stylus = require('../loaders/stylus') 

environment.plugins.prepend('VueLoaderPlugin', new VueLoaderPlugin())
environment.loaders.prepend('vue', vue)
module.exports = environment
// 作ったstylusをロード
environment.loaders.prepend('stylus', stylus) 
```

- **webpackを再読み込みしてからサーバーを再起動**
	- `bin/webpack`
	- `rails s`
#### Todoリストを表示する部分
 - `yarn axios`でaxiosを追加して、フロントエンドからHTTPリクエストをする。
```html
/app/javascript/packs/components/index.vue
<template>
  <div>
    <div>
      <input v-model="newTask" placeholder="to doを追加して下さい">
      <div v-on:click="createTask">
        <i>追加</i>
      </div>
    </div>
    <ul>
      <li v-for="(task, index) in tasks">
        <input type="checkbox" v-model="task.is_done" v-on:click="update(task.id, index)">
        <span v-bind:class="{done: task.is_done}">{{ task.name }}</span>
        <button v-on:click="deleteTask(task.id, index)">削除</button>
      </li>
    </ul>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    data: function () {
      return {
        tasks: [],
        newTask: ''
      }
    },
    mounted: function () {
      this.fetchTasks();
    },
    methods: {
      fetchTasks: function () {
        axios.get('/api/tasks').then((response) => {
          for(let i = 0; i < response.data.tasks.length; i++) {
            this.tasks.push(response.data.tasks[i]);
          }
        }, (error) => {
          console.log(error, response);
        });
      },
      createTask: function () {
        if(this.newTask == '') return;

        axios.post('/api/tasks', { task: { name: this.newTask } }).then((response) => {
          this.tasks.unshift(response.data);
          this.newTask = '';
        }, (error) => {
          console.log(error, response);
        });
      },
      deleteTask: function (task_id, index) {
        axios.delete('/api/tasks/' + task_id).then((response) => {
          this.tasks.splice(index, 1);
        }, (error) => {
          console.log(error, response);
        });
      },
      update: function (task_id) {
        axios.put('/api/tasks/' + task_id).then((response) => {
        }, (error) => {
          console.log(error);
        });
      }
    }
  }
</script>
```
- つくったindexを組み込む。
```js
/app/javascript/packs/todo.js

import Vue from 'vue/dist/vue.esm.js'
import Header from './components/header.vue'
import Index from './components/index.vue' // 追加

var app = new Vue({
  el: '#app',
  components: {
    'navbar': Header,
    'contents' : Index // 追加
  }
});

```

- ヘッダーのしたにindex.vueを表示するため、`<navbar></navbar>`の下に`<contents></contents>`を追加。
- 
```erb
/app/views/home/index.erb

<div id="app">
  <navbar></navbar>
  <contents></contents>
</div>

<%= javascript_pack_tag 'todo' %>
```

## 参考
[初心者向けVue.js × Railsでのアプリ作成(ToDoリスト編)](https://qiita.com/fujigaki/items/6358dc6b6890f042fdb3)