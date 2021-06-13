# JSON形式で出力する

- データベースから取得したデータをJSON形式で返したい場合、`render`メソッドを使ってJSON形式で利用者へ結果を返す。

- オブジェクトをJSON形式に変換した上で利用者へ返す。
```ruby

render :json => オブジェクト

```

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
```

## 参考
- [JSON形式で出力](https://www.javadrive.jp/rails/controller/index7.html#section1)
