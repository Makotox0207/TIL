# ルーティングでネストした際のform_withのエラー



[![Image from Gyazo](https://i.gyazo.com/00ac376052d98abcb0252a7fc5728091.png)](https://gyazo.com/00ac376052d98abcb0252a7fc5728091)
```ruby
# views/questions/show.html.slim

.col-sm 
  - if @question.user.avatar?
    = image_tag @question.user.avatar.url, size: '50x50'
.col-sm
  h2 =@question.title
.col-sm 
  - if current_user.id == @question.user_id
    = link_to '編集', edit_question_path, class: 'btn btn-success'
    = link_to '削除', question_path, method: :delete, data: { confirm: '質問を削除しますか？'}, class: 'btn btn-danger'
p =@question.body

= form_with url: question_answers_path, local: true do |f|
  .form-group 
    = f.label :body, '回答'
    = f.text_area :body, class: 'form-control'
  = f.submit '回答'


```

```ruby
# questions_conroller.rb
class AnswersController < ApplicationController
 def show
    @question= Question.find(params[:id])
    @answer = Answer.new
  end
end
```

```ruby
# answers_controller.rb
class AnswersController < ApplicationController
  def create
    answer =  Answer.new(params[:body, :question_id])
    if answer.save
      redirect_to root_url, notice: '回答しました'
    else
      render "questions/show"
  end

end
```

```ruby
Rails.application.routes.draw do

  resources :questions do 
    resource :answers, only: [:create]
  end
end

```

```ruby
# xxxxxx_create_answers.rb
class CreateAnswers < ActiveRecord::Migration[5.2]
  def change
    create_table :answers do |t|
      t.integer :user_id
      t.integer :question_id
      t.text :body, null: false
      t.timestamps
    end
  end
end
```

## 仮説
* `No route matches {:action=>”create”, :controller=>”answers”, :id=>”6”}, missing required keys: [:question_id]`はルートが一致せず、question_idが必要だが無い状態である
* question_idを渡す必要がある
* user_idも渡していないようなので渡す必要がある

* 以上のことから、answersコントローラのcreateアクションで、*回答を作ったユーザーのidをuser_idに代入*し、*question_idに対してURLでのquestion_idを代入する*。
```ruby

# answers_controller.rb
class AnswersController < ApplicationController
  def create
    question= Question.new(params[:body])
    question.user_id = current_user.id
    if question.save
      redirect_to root_url, notice: '質問を投稿しました'
    else
      render :new
    end
  end
```

## 検証結果

まだ、:question_idが表示されていない状態だった。

form_withでのurlがミスしてるっぽい。
```
(byebug) question_answers_path
*** ActionController::UrlGenerationError Exception: No route matches {:action=>"create", :controller=>"answers", :id=>"6"}, missing required keys: [:question_id]
```

というか、データベースに保存するときはmodelで引数をしていしないといけないので、urlの時点で間違っている。






