# letとlet!の違い

## let
 定義した定数が呼び出されたタイミングで初めて評価される（遅延評価）

例えば、`let(:user_a) { FactoryBot.create(:user, name: 'ユーザーA', email: 'a@example.com') }`は`let(:login_user) { user_a }`で初めて評価される。
`let(:login_user) { user_a }`については、beforeでのログイン処理で初めて評価される。

つまり、処理する流れとしては以下の通りになる。
1. 「最初のタスク」を、ユーザーAで作成し、ここで初めてuser_aは実行される。
2. ログインする画面に遷移する
3. `:login_user`が実行されると同時に`:user_a`も実行され、user_aのemailがsession_emailに入力される。
4. 2と同じ流れでletが実行され、user_aのpasswordがsession_passwordに入力される。
5. ログインに成功し、一覧画面に遷移する。
6. その画面で「最初のタスク」が表示されていることを期待して調べる。
7. 1と同様の流れを経て、ログイン画面に遷移
8. 3〜4と同じようにuser_bでログインし、一覧画面に遷移する。
9. その画面で「最初のタスク」が表示されていないことを期待して調べる。
```ruby
describe 'タスク管理機能', type: :system do
  let(:user_a) { FactoryBot.create(:user, name: 'ユーザーA', email: 'a@example.com') }
  let(:user_b) { FactoryBot.create(:user, name: 'ユーザーB', email: 'b@example.com') }

  before do
	  FactoryBot.create(:task, name: '最初のタスク', user: user_a)
    visit login_path
    fill_in 'session_email', with: login_user.email
    fill_in 'session_password', with: login_user.password
    click_button 'ログイン'
  end

  describe '一覧表示機能' do
    
    context 'ユーザーAがログインしているとき' do
      let(:login_user) { user_a }

      it 'ユーザーAが作成したタスクが表示される' do
			expect(page).to have_content '最初のタスク'
		end
    end

    context 'ユーザーBがログインしているとき' do
      let(:login_user) { user_b }

      it 'ユーザーAが作成したタスクが表示されない' do
        expect(page).to have_no_content '最初のタスク'
      end
    end
  end
```

## let!
各テスト実行前に評価される

例えば、`let!(:task_a) {FactoryBot.create(:task, name: '最初のタスク', user: user_a)}`はその場で実行される
```ruby
describe 'タスク管理機能', type: :system do
  let(:user_a) { FactoryBot.create(:user, name: 'ユーザーA', email: 'a@example.com') }
  let(:user_b) { FactoryBot.create(:user, name: 'ユーザーB', email: 'b@example.com') }
  let!(:task_a) {FactoryBot.create(:task, name: '最初のタスク', user: user_a)}

  before do
    visit login_path
    fill_in 'session_email', with: login_user.email
    fill_in 'session_password', with: login_user.password
    click_button 'ログイン'
  end

  shared_examples_for 'ユーザーAが作成したタスクが表示される' do
    it {  expect(page).to have_content '最初のタスク' }
  end

  describe '一覧表示機能' do
    
    context 'ユーザーAがログインしているとき' do
      let(:login_user) { user_a }

      it_behaves_like 'ユーザーAが作成したタスクが表示される'
    end

    context 'ユーザーBがログインしているとき' do
      let(:login_user) { user_b }

      it 'ユーザーAが作成したタスクが表示されない' do
        expect(page).to have_no_content '最初のタスク'
      end
    end
  end

  describe '詳細表示機能' do
    context 'ユーザーAがログインしているとき' do
      let (:login_user) {user_a}

      before do
        visit task_path(task_a)
      end

      it_behaves_like 'ユーザーAが作成したタスクが表示される'
    end
  end

  describe '新規作成機能' do
    let(:login_user) {user_a}

    before do
      visit new_task_path
      fill_in 'task_name', with: task_name
      click_button  '登録する'
    end

    context '新規作成画面で名称を入力したとき' do
      let(:task_name) { '新規作成のテストを書く'}

      it '正常に登録される' do
        # alert-successというcssクラスがついたテキストに「新規作成のテストを書く」が含まれるかどうか調べる
        expect(page).to have_selector '.alert-success', text: '新規作成のテストを書く'
      end
    end

    context '新規作成画面で名称を入力しなかったとき' do
      let(:task_name) {''}

      it 'エラーとなる' do
        # error_explanationという検証エラーを表示する領域内で「名称を入力してください」というメッセージが含まれるかどうか調べる
        within '#error_explanation' do
          expect(page).to have_content '名称を入力してください'
        end
      end
    end
  end

end

```
