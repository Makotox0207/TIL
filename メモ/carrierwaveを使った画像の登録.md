# carrierwave画像の登録

---
*【目次】*
1. [gemファイルにcarrierwaveを追加](https://github.com/Makotox0207/TIL/new/main/%E3%83%A1%E3%83%A2#gem%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%ABcarrierwave%E3%82%92%E8%BF%BD%E5%8A%A0)
2. [アップローダークラスの生成](https://github.com/Makotox0207/TIL/new/main/%E3%83%A1%E3%83%A2#%E3%82%A2%E3%83%83%E3%83%97%E3%83%AD%E3%83%BC%E3%83%80%E3%83%BC%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%AE%E7%94%9F%E6%88%90)
3. [アップロード画像のカラムを追加](https://github.com/Makotox0207/TIL/new/main/%E3%83%A1%E3%83%A2#%E3%82%A2%E3%83%83%E3%83%97%E3%83%AD%E3%83%BC%E3%83%89%E7%94%BB%E5%83%8F%E3%81%AE%E3%82%AB%E3%83%A9%E3%83%A0%E3%82%92%E8%BF%BD%E5%8A%A0)
4. [カラムとアップローダクラスの紐づけ](https://github.com/Makotox0207/TIL/new/main/%E3%83%A1%E3%83%A2#%E3%82%AB%E3%83%A9%E3%83%A0%E3%81%A8%E3%82%A2%E3%83%83%E3%83%97%E3%83%AD%E3%83%BC%E3%83%80%E3%82%AF%E3%83%A9%E3%82%B9%E3%81%AE%E7%B4%90%E3%81%A5%E3%81%91)
5. [画像の登録・更新](https://github.com/Makotox0207/TIL/new/main/%E3%83%A1%E3%83%A2#%E7%94%BB%E5%83%8F%E3%81%AE%E7%99%BB%E9%8C%B2%E6%9B%B4%E6%96%B0)
---

## gemファイルにcarrierwaveを追加
1. gemファイルにcarrierwaveを追加する
```ruby
gem 'carrierwave', '~> 2.0'
```

2. bundle installをする
```
bundle install
```

3. サーバーを再起動させる

## アップローダークラスの生成
1. アップローダークラスの生成
```
bundle exec rails g uploader アップローダー名

# bundle exec rails g uploader Avatar

```

2. app /uploaders /アップローダー名_uploader.rbが生成される
	* 作成したアップローダークラス(AvatarUploader)では、アップロードするファイルの拡張子やサイズ、保存するパスを指定できる。
	* storage :fileがデフォルトで指定されているため、アップロードしたファイルはpublic/配下に保存される。
	* 保存されるディレクトリは、store_dirで設定される。

## アップロード画像のカラムを追加
1. アップロード画像の情報を保存するカラムを追加するための、マイグレーションファイルを追加する。
```
bundle exec rails g migration add_avatar_to_users avatar:string
```

```ruby
# マイグレーションファイル

class AddAvatarToUsers < ActiveRecord::Migration
  def change
    add_column :users, :avatar, :string
  end
end

```

2. `bundle exec rails db:migrate`をする
	* このカラムには、「画像のデータ」ではなく「画像のファイル名」を保存する。
		* 画像のファイル名で保存すると、データベースサーバーの要領が圧迫することを防げる
		* このカラムには、どの画像ファイルか確認できるようにファイル名だけを保存し、画像を表示するときに「画像が格納されてるパス」と「DBに保存されているファイル名」を使う

3. データベースに保存出来るように、対象のコントローラの画像のファイル名を保存するカラムを追加する
```ruby
def user_params
  params.require(:user).permit(:nickname, :age, :avatar) 
end
```

## カラムとアップローダクラスの紐づけ
1. 「アップロード用のカラム」と「アップローダークラス」をモデルで紐づける
```ruby
class モデル名 < ActiveRecord::Base
  mount_uploader [:カラム名], [アップローダークラス]
end

# class User < ActiveRecord::Base
#   mount_uploader :avatar, AvatarUploader
# end

```
	* `AvatarUploader`クラスでは、画像の保存場所が`storage :file`と指定しているので、アップロードした画像は`public/uploads`配下に保存される。

## 画像の登録・更新
1. `file_field`でファイル選択ボックスを作成する
```ruby
.field
  = form.label :avatar 
    = form.file_field :avatar 
```
	* アップロードした画像は`public/uploads/モデル名/画像のカラム名/id`配下に保存される。

## 画像の表示
1. アップロードした画像を、 imgタグを生成する`image_tag`を使って表示する
```ruby
- if @user.avatar? 
    # userインスタンスの画像ファイルのURLを取得し表示
    # image_tag 'ファイル名', オプション
    = image_tag @user.avatar.url 
```
	 * アバター画像を表示させるには、まずは画像が保存されている場所(パス)を取得する必要がある
		 * メソッド / 内容 / ex
		 * url / ファイルのURLを取得 / `userインスタンス.avatar.url`
		 * current_path / カレントパス取得 / `userインスタンス.avatar.current_path`
		 * avatar_identifier / ファイル名を取得 / `userインスタンス.avatar.avatar_identifier`

### 保存できる理由
* public以下に置かれるファイル(画像)は、Railsのデフォルトでブラウザに直接送信される
* →アドレスバーにpublic以下のパス(url)を指定する
* →その画像をブラウザで表示できる

## 参考
* https://pikawaka.com/rails/carrierwave




