# joinsとは

```ruby
モデル名.joins(:関連名)
```
has_manyメソッドやbelongs_toメソッドの引数で指定した関連名(:cats,:owner)をjoinsメソッドの引数で指定することでテーブルを内部結合する。
joinsメソッドの引数はテーブル名ではなく、アソシエーションで定義した関連名になる。
joinsメソッドは、モデルのレコードしか取らないのでテーブルのレコードをのみ取得した結果になっている。
これは、発行されるSQLのSELECT文でそのテーブルの全てのカラムが指定されているから。

## Select
joinsメソッドで取得したレコードカラムの情報を追加したい場合は、selectメソッドを使う。
関連先のモデルのレコードも取得する
```ruby
# selectメソッドの定義
select(取得したい列)
```

## Where
joinsメソッドでテーブルを内部結合する際に、whereメソッドを使って条件を抽出してテーブルを結合できる。
whereメソッドでは条件にあったレコードを取得する。
```ruby
モデル名.where(カラム名: 値)
```


### 結合先のテーブルに対して条件を追加する
```ruby
# joinsメソッドの引数には、アソシエーションで指定した関連名を指定しているが、whereメソッドでは結合先のテーブル名を指定している
モデル名.joins(:関連名).where(結合先のテーブル名: { カラム名: 値 })
```

## まとめ
```ruby
# articlesテーブルのstatusカラムの値がunpublishedを除外
Article.select('articles.*').where.not(status: 'unpublished')

# ログインしているユーザーのunpublishedのみを取得
Article.joins(:user).select('articles.*').where(users: {id: session[:user_id]}).where(status: 'unpublished')
```
