# to_s


## to_sメソッドとは
to_sメソッドとは、**文字列以外のオブジェクトを文字列に変換するメソッド**。
例えば、数字を文字列に変換したり、小数を変換するのに使用する。
 Rubyでは数字と文字列をそのまま+演算子でくっつけることができないので、数字と文字列をくっつけて表示したい時などに利用する。

```ruby
オブジェクト.to_s

10.to_s # => "10"
```


## 2つの引数を受けて、文字列に変換して関数をつくる
```ruby
def join_two(a,b)
  return a.to_s + b.to_s
end

join_two("hello","world") #=> "helloworld"
join_two("hello",1) #=> "hello1"
join_two("test",1.111) #=> "test1.111"
join_two(100,1.1) #=> "10001.1"

```
