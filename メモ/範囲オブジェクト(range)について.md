# 範囲オブジェクト(range)について
チェリー本 p99~102


## 範囲オブジェクトとは
範囲オブジェクトとは、*「1から5まで」「文字’a’から文字’e’まで」のように、値の範囲を表すオブジェクトのことである。*

```ruby
# 最後の値を含む（以上以下）
最初の値..最後の値 

# 最後の値を含まない（以上未満）
最初の値...最後の値 
```

### example
```ruby
# ..を使うと5が範囲に含まれる（1以上5以下）
range = 1..5

range.include?(0) #=> false
range.include?(1) #=> true
range.include?(4.9) #=> true
range.include?(5) #=> true
range.include?(6) #=> false

# ...を使うと5が範囲に含まれない（1以上5未満）
range = 1...5

range.include?(0) #=> false
range.include?(1) #=> true
range.include?(4.9) #=> true
range.include?(5) #=> false
range.include?(6) #=> false
```

## 注意点
()で囲まずにメソッドを呼び出すとエラーになる。
これは、`..`や`...`は優先順位が低いからである。
```ruby
1..5.include?(1) #=> NoMethodError: undefined method `include?' for 5:Fixnum

# 次のように解釈されている
1..(5.include?(1))
```

そのため、エラーにならないようにするためには範囲メソッドを()で囲う必要がある。
```ruby
(1..5).include?(5) #=> true
```

## 使用例

### 配列や文字列の一部を抜き出す
```ruby
a = [1, 2, 3, 4, 5]
# 2番目から4番目までの要素を取得する
a[1..3] #=> [2, 3, 4]

b = 'abcdef'
# 2文字目から4文字目までを抜き出す
a[1..3] #=> "bcd"
```

### n以上m以下、n以上m未満の判定
```ruby
# 0度以上100度未満であればliquidと判定したい

def liquid?(temperture)
	(0...100).include?(temperture)
end

liquid?(-1) #=> false
liquid?(0) #=> true
liquid?(99) #=> true
liquid?(100) #=> false
```

### case文

```ruby
# 年齢に応じて料金を判定する
# 0歳から5歳までの場合は0円
# 6歳から12歳までの場合は300円
# 13歳から18歳までの場合600円
# それ以外の場合は1000円

def charge(age)
	case age
	# 0歳から5歳までの場合
	when 0..5
		0
	# 6歳から12歳までの場合
	when 6..12
		300
	# 13歳から18歳までの場合
	when 13..18
		600
	# それ以外の場合
	else
		1000
	end
end

charge(3) #=> 0
charge(12) #=> 300 
charge(16) #=> 600
charge(25) #=> 1000
```

### 値が連続する配列を作る

```ruby
(1..5).to_a #=> [1, 2, 3, 4, 5]
(1...5).to_a #=> [1, 2, 3, 4]

('a'..'e').to_a #=> ["a", "b", "c", "d", "e"]
('a'...'e').to_a #=> ["a", "b", "c", "d"]

('bad'..'bag').to_a #=> ["bad", "bae", "baf", "bag"]
('a'..'e').to_a #=> ["bad", "bae", "baf"]
```

### 繰り返し処理
```ruby
# 範囲オブジェクトを配列に変換して繰り返し処理を行う

numbers = (1..4).to_a
sum = 0
numbers.each { |n| sum += n }
sum #=> 10

# 範囲オブジェクトに対して直接eachメソッドを呼び出すこともできる
sum = 0
(1..4).each { |n| sum += n }
sum #=> 10

```






