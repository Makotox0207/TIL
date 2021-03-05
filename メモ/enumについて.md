# enumについて

## enum
 enumを使う。
 enumは、*1つのカラムに指定した複数個の定数を保存できるようにするためのもの*である。
また、enumはハッシュで複数個の定数を定義するので、定数の数だけハッシュの要素を増やすことでリストに登録している定数を保存できる。

* 2個以上の定数を紐付けたい場合は型はinteger型で定義する。
* 2個の定数を紐付けたい場合かつ真偽値を保存したい時はboolean型で定義する。
* null: falseの場合は初期値を付ける。

### integerの場合
```ruby
# db/migrate/create_users.rb
class CreateUsers < ActiveRecord::Migration[5.2]
  def change
    create_table :users do |t|
      t.string     :name,           null: false, default: ""
      t.integer   :age,              null: false
      # 新しくenum用のinteger型のblood_typeカラムを追加
      t.integer   :blood_type, null: false, default: 0
      t.timestamps
      t.timestamps
    end
  end
end
```

```ruby
# app/models/user.rb
class User < ApplicationRecord
  enum blood_type: { A: 0, B: 1, O: 2, AB: 3 }
end
```

この場合、blood_typeカラムに*0が保存されていればA*という定数として扱える。
同じく、*1が保存されていればB*、*2が保存されていればO*、*3が保存されていればAB*として、モデルに定数と整数をhashの形で紐付けることによって１つのカラムと複数個の定数を紐づけられる。

配列で定義もすることができる。
```ruby
class User < ApplicationRecord
  # シンボルで定義する場合
  enum blood_type: [ :A, :B, :O, :AB ]

  # 文字列で定義する場合
  enum blood_type: [ "A", "B", "O", "AB" ]
end

```
配列のインデックスは先頭から順に0から数字が入って定数と紐づくので、
* User.create(blood_type: ‘A’) → DBに0が保存される
* User.create(blood_type: ‘B’) → DBに1が保存される
* User.create(blood_type: ‘O’) → DBに2が保存される
* User.create(blood_type: ‘AB’) → DBに3が保存される


### booleanの場合

```ruby

# db/migrate/create_users.rb

class CreateUsers < ActiveRecord::Migration[5.2]
  def change
    create_table :users do |t|
      t.string     :name,           null: false, default: ""
      t.integer   :age,              null: false
      t.integer   :blood_type, null: false, default: 0
      # 新しくenum用のboolean型のis_marriedカラムを追加
      t.boolean      :is_married,   null: false, default: false
      t.timestamps
      t.timestamps
    end
  end
end
```

```ruby
class User < ApplicationRecord
  enum blood_type: { A: 0, B: 1, O: 2, AB: 3 }
  enum is_married: { single: false, married: true } 
end
```

booleanのenumはintegerで定義したように、定数とそれに紐づく真偽値を設定して、定数を保存したらそれに紐づく真偽値を保存され、falseであれば0、trueの場合は1として保存される。

