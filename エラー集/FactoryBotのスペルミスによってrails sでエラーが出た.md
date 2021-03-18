rails sをするとこんな感じでエラーがでた

```
taskleaf/spec/factories/tasks.rb:1:in `<top (required)>': undefined local variable or method `factoryBot' for main:Object (NameError)
```

なので、ここに書いてある通りtasks.rbをみてみるとこんな感じで書いていた。

```ruby
# spec/factories/tasks.rb
factoryBot.define do
  factory :task do
    name { 'テストを書く'}
    description {'RSpec&Capybara&FactoryBotを準備する'}
    user
  end
end
```
上のコードは一見するとなんの間違いも内容に見えるが、system specのファイルでは、factoryBotではなく、FactoryBotと使用しており、そもそもfactoryBot存在しない。
また、system specではFactoryBotでのメソッドを利用している？

```ruby
require 'rails_helper'

describe 'タスク管理機能', type: :system do
  describe '一覧表示機能' do
    before do
      user_a = FactoryBot.create(:user, name: 'ユーザーA', email: 'a@example.com')
      FactoryBot.create(:task, name: '最初のタスク', user: user_a)
    end
    context 'ユーザーAがログインしているとき' do
      before do
        visit login_path
        fill_in 'メールアドレス', with: 'a@example.com'
        fill_in 'パスワード', with: 'password'
        click_button 'ログインする'
      end

      it 'ユーザーAが作成したタスクが表示される' do
        expect(page).to have_content '最初のタスク'
      end
    end
  end
end
```

そのため、spec/factories/tasks.rbのコードは以下のように直す必要があり、spec/factories/users.rbも同じような間違いをしていたため、それぞれfactoryBotからFactoryBotに直す

```ruby
# spec/factories/tasks.rb
FactoryBot.define do
  factory :task do
    name { 'テストを書く'}
    description {'RSpec&Capybara&FactoryBotを準備する'}
    user
  end
end
```
```ruby
# spec/factories/users.rb
FactoryBot.define do
  factory :user do
    name { 'テストユーザー'}
    email { 'test1@example.com'}
    password { 'password'}
  end
end
```

直し終えたので再度rails sしたらできた。
