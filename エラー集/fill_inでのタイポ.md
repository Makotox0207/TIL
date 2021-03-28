withで文字列として入力しており、ログインが失敗→そこで最初のタスクを期待→エラー
```ruby
require 'rails_helper'

describe 'タスク管理機能', type: :system do
  describe '一覧表示機能' do
    # ユーザーAとユーザーBを定義
    let(:user_a) { FactoryBot.create(:user, name: 'ユーザーA', email: 'a@example.com') }
    let(:user_b) { FactoryBot.create(:user, name: 'ユーザーB', email: 'b@example.com') }
    before do
      # ユーザーAのタスクがデータベースに登録される
      FactoryBot.create(:task, name: '最初のタスク', user: user_a)
      visit login_path
		# ここが原因
      fill_in 'session_email', with: 'login_user.email'
      fill_in 'session_password', with: 'login_user.password'
      click_button 'ログイン'
    end
    
    context 'ユーザーAがログインしているとき' do
      # login_userをletで定義
      let(:login_user) { user_a }
      it 'ユーザーAが作成したタスクが表示される' do
        expect(page).to have_content '最初のタスク'
      end
    end
    byebug
    context 'ユーザーBがログインしているとき' do
      # login_userをletで定義
      let(:login_user) { user_b }
      it 'ユーザーAが作成したタスクが表示されない' do
        # ユーザーAが作成したタスクの名称が画面上に表示されていないことを確認
        expect(page).to have_no_content '最初のタスク'
      end
    end
  end
end

```
```
# エラー
Failures:

  1) タスク管理機能 一覧表示機能 ユーザーAがログインしているとき ユーザーAが作成したタスクが表示される
     Failure/Error: expect(page).to have_content '最初のタスク'
       expected to find text "最初のタスク" in "Taskleaf\nログイン\nログイン\nメールアドレス\nパスワード"
     
     [Screenshot]: tmp/screenshots/failures_r_spec_example_groups_nested_nested_a_ユーザーaが作成したタスクが表示される_233.png

     
     # ./spec/system/tasks_spec.rb:21:in `block (4 levels) in <top (required)>'

Finished in 5.41 seconds (files took 7.4 seconds to load)
2 examples, 1 failure

Failed examples:

rspec ./spec/system/tasks_spec.rb:20 # タスク管理機能 一覧表示機能 ユーザーAがログインしているとき ユーザーAが作成したタスクが表示される
```
[![Image from Gyazo](https://i.gyazo.com/c2d7e31f5b696d20380e7def2fb0d9d9.png)](https://gyazo.com/c2d7e31f5b696d20380e7def2fb0d9d9)

こんな感じにすると、それぞれのユーザーのemailとpasswordでログインする
```ruby
fill_in 'session_email', with: login_user.email 
fill_in 'session_password', with: 'login_user.password
```
