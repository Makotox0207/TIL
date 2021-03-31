# for 文

あらかじめ指定したオブジェクトから順に値を取り出しながら繰り返し処理を行う。

繰り返しが1回行われるたびにオブジェクトに対して「each」メソッドが実行し取得した要素などを変数に代入するため、*指定できるオブジェクトは「each」メソッドを持ったオブジェクトでなければならない*
(ex. 配列、ハッシュ、範囲オブジェクト)
```ruby
for 変数 in オブジェクト do
  実行する処理1
  実行する処理2
end
```

```ruby

array = [1,2,3,4,5]
 
for i in array do
  puts i
end

# 1
# 2
# 3
# 4
# 5
```

## next
```ruby

for i in 1..5 do
  if i == 4
    next
  end
  puts i
end

# 1
# 2
# 3
# 5
```

## break

```ruby
for i in 1..5 do
  if i == 4
    break
  end
  p i
end

# 1
# 2
# 3

```

# redo
iに4が入ったときにiに5が代入され、その後条件式を無視して、5を表示したためループは終わらない。
```
for i in 1..5 do
  if i == 4
    i = 5
    redo
  end
  p i
end

# 1
# 2
# 3
# 5
# 5

```
