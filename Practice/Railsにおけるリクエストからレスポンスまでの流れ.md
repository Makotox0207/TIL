# Railsにおけるリクエストからレスポンスまでの流れ

## リクエストからレスポンスまでの流れについて(GET編)


(1) ユーザーがユーザー一覧ページを表示するためのURLである/userのリンクボタンを押す

(2) /userに対するGETリクエストが発生する

(3) Webサーバがこのリクエストを受け取る

(4) ルーティングでそのリクエストに対応するコントローラとアクションを実行する

(5) のコントローラはUser.allを呼び出し、Userモデルを経由し、usersデータベースからコレクションを読み込む

(6) インスタンス変数である@usersに、このコレクションが代入される

(7) @usersをビューで展開し、クライアントに対してレンダリングする


## リクエストからレスポンスまでの流れについて(POST編)

### 新規作成ページを表示する

* (1)  ユーザーがtasks/newというURLの新規作成ページにつながるリンクを押す
![スクリーンショット 2021-02-20 17.12.39.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/e4d1afd4-eb13-4eb1-bf81-4675b8a04bb4.png)

```
/ index.html.slim
h1 タスク一覧
/ このlink_toを押すと新規作成ページのurlへのGETリクエストが発生する
= link_to '新規登録', new_task_path, class: 'btn btn-primary'

.mb-3
table.table.table-hover
  thead.thead-default
    tr
      th= Task.human_attribute_name :name
      th= Task.human_attribute_name  :created_at
      th
    tbody
    - @tasks.each do |task|
      tr
        td= link_to task.name, task
        td= task.created_at
        td
          = link_to '編集', edit_task_path(task), class: 'btn btn-primary mr-3'
          = link_to '削除', task, method: :delete, data: { confirm: "「#{task.name}」を削除します。よろしいですか？"}, class: 'btn btn-danger'
```

* (2) 新規作成ページのURLへのGETリクエストが発生する
![スクリーンショット 2021-02-16 16.01.15.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/627d8881-328d-4d75-92d2-7f285b70583f.png)

* (3) WEBサーバがこのリクエストを受け取る

* (4) ルーティングで、このリクエストに対応するコントローラとアクション（`tasks#new`）を実行する
```
#routes.rb
Rails.application.routes.draw do
        get '/tasks/new', to: 'tasks#new'
end
```

* (5) newアクション内で、Task.newによって空のインスタンスを作成し、`@task`に代入する
```
#tasks_controller.rb
class TasksController < ApplicationController
        def new
            @task = Task.new
        end
end
```
* (6) tasks/newのビューファイルが、クライアント側にレンダリングされる
```
/ new.html.slim
h1 タスクの新規登録
.nav.justify-content-end
        = link_to '一覧', tasks_path, class: 'nav-link'
= form_with model: @task, local: true do |f|
        .form_group
                = f.label :name
                = f.text_field :name, class:'form-control', id: 'task_name
        = f.submit nil, class: 'btn-primary'
```


*  ![スクリーンショット 2021-02-20 17.40.58.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/e12f6aa8-a194-42e6-8763-1382240d1ad5.png)
![スクリーンショット 2021-02-20 17.50.49.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/50238507-f7f9-4d7b-a665-800c75b725ed.png)

### タスク名(name)を入力する

* (7) 入力欄にタスク名(name)を入力する。
今回は、例としてtestと入力する。
![スクリーンショット 2021-02-20 21.01.20.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/4700b9f4-b80a-45b5-8a6a-285d65640f6d.png)

### 作成ボタンを押す

* 前提として、form_withとそれに付随するhtmlタグは以下のようにコンパイルされる
```
/ new.html.slim
/ model: モデルを指定するために、そのモデルのインスタンスが代入されたインスタンス変数である@taskが代入される
= form_with model: @task, local: true do |f|
        .form_group
                = f.label :name
                = f.text_field :name, class:'form-control', id: 'task_name'

        = f.submit nil, class: 'btn-primary'
```

```
# actionでは送信先のURLを指定するので、action = "/tasks"ということはcreateアクションのURLが指定されているということである
# methodではHTTPリクエストを指定するので、method="post"である場合HTTPリクエストはPOSTである
<form action="/tasks" accept-charset="UTF-8" method="post
        <input name="utf8" type="hidden" value="✓">
        <input type="hidden" name="authenticity_token" value="UsEM4eWu7Yxl4DaWjJEWKr5n5pCkTifkJ4WESe9vZWzKzk+wUs692dv3W8xzdhJln4UmuaXPcH1qvJ68whzVhQ==">
        <div class="form_group">
                <label>名称</label>
                # name="task[name]"では、params = { task: { name: "タスク名"}}のように、フォームデータが2重のハッシュ構造を持たせられる
                <input class="form-control" id="task_name" type="text" name="task[name]">
        </div>
        <input type="submit" name="commit" value="登録する" class="btn-primary" data-disable-with="登録する">
</form>
```

* (8) `task[name]: test`は`{ task: {name: “test”}}`というデータ構造であり、そのPOSTリクエストが、createアクションのURLである/tasksに対して発生する
![スクリーンショット 2021-02-21 19.34.22.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/fd2cc8e2-dc0d-4f96-8d8f-d11ae3e56741.png)
![スクリーンショット 2021-02-21 19.34.59.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/79b349c1-22c1-49cb-b8a9-5c082ac06063.png)

* (9) ルーティングで、このリクエストに対応するコントローラとアクションである`tasks#create`を実行する
```
#routes.rb
Rails.application.routes.draw do
        get '/tasks/new', to: 'tasks#new'
        post '/tasks', to: 'tasks#create'
end
```

* (10) createアクション内で、Taskモデルのインスタンスを作成する。そのために、newメソッドの引数を、ストロングパラメータである`tasks_params`で受け取った値にし、データベース内のtasksテーブルを操作するTaskモデルのクラスのインスタンスを作成し、`@task`に代入する。
```
#tasks_controller.rb
class TasksController < ApplicationController
        def new
            @task = Task.new
        end
        def create
            # task_paramsで取得した値を引数として、インスタンスを作成する
            @task = Task.new(task_params)
        end
        def show
            @task = Task.find(params[:id])
        end
private
        def task_params
            # ストロングパラメータを設定し、不正な情報を受け取らないようにする
            # paramsメソッドで、クライアント側から送られてきたパラメータで取り出したい値のキーを指定する
            # requireメソッドで、paramsで取得したパラメーターのオブジェクト名を指定する。
            # permitメソッドで、許可された値のみを取得する
            params.require(:task).permit(:name)
        end
end
```

### タスクの詳細画面へ遷移する

* (11) データベースへの保存に成功した際に、詳細画面にリダイレクトさせる場合、`redirect_to`は指定したパスに遷移させられるため、そのパスとしてshowのパスを設定する。
```
#tasks_controller.rb
class TasksController < ApplicationController
        def new
            @task = Task.new
        end
        def create
            @task = Task.new(task_params)

            if @task.save
                redirect_to @task, notice: "タスク「#{@task.name}」を登録しました"
            else
                render :new
            end
        end
private
        def task_params
            params.require(:task).permit(:name)
        end
end
```

* また、なぜ、`redirect_to`のパスとしてインスタンス変数である`@task`を指定しているのかというと、ルーティングのtasks#showのパスが使われるからである。
![スクリーンショット 2021-02-22 19.59.17.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/83b880e8-0b30-40ea-aa6c-28eded18ce02.png)
そのため、`redirect_to @task`はtasksコントローラのshowアクションを呼び出す。

* (12) クライアント側からから改めてサーバー側へリクエストが送信され、
GETリクエストが発生する。
![スクリーンショット 2021-02-22 19.32.16.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/f70e4021-1a03-4373-961c-d46a7e4d9f03.png)

* (13) ルーティングでリクエストに対応するコントローラとアクション`tasks#show`が実行される
```
#routes.rb
Rails.application.routes.draw do
	    get '/tasks/new', to: 'tasks#new'
	    post '/tasks', to: 'tasks#create'
	    get '/tasks/:id', to: 'tasks#show'
end
```

* (14) showアクションが起動され、受け取ったパラメータのidに対応するレコードを、Taskモデルを経由し、tasksデータベースから取得し、そのデータは`@task`に代入される
```
#tasks_controller.rb
class TasksController < ApplicationController

	    def new
                @task = Task.new
        end
	    def create
                @task = Task.new(task_params)

                if @task.save
                    redirect_to @task, notice: "タスク「#{@task.name}」を登録しました"
                else
                    render :new
                end
        end
        def show
            #findメソッドは、モデルオブジェクトに対応するレコードを受け取ったパラメータのidでデータベースから検索する
            @task = Task.find(params[:id])
        end
private
        def task_params
            params.require(:task).permit(:name)
        end
end
```

* (15) `@task`がビューで展開される

````
/ show.html.slim

h1 タスクの詳細

.nav.justify-content-end
  = link_to '一覧', tasks_path, class: 'nav-link'
table.table.table-hover
  tbody
    tr
      th= Task.human_attribute_name :id
      td= @task.id
    tr
      th= Task.human_attribute_name :name
      td= @task.name
    tr
      th= Task.human_attribute_name :created_at
      td= @task.created_at
    tr
      th= Task.human_attribute_name :updated_at
      td= @task.updated_at
```
(16) そのビューはクライアント側にレスポンスされて、表示される
![スクリーンショット 2021-02-22 20.10.40.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/15e4aa0d-f68f-4ee8-beaa-3ac0044ebdac.png)
