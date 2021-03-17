# Procオブジェクトとは

paizaででてきてわからなかったのでメモ。

ブロックは基本的にはオブジェクトではないが、ブロックをオブジェクト化する。
Rubyでは{}やdo endで書いたブロックを=でそのまま変数に代入できないため、その際に用いる？
仮引数の最後に & 付きパラメーターを追加することで、Ruby がこのパラメーター (&hoge) をブロックとして扱い、hoge は Proc オブジェクトへの参照を持つ。

```ruby
def sample_method(&hoge)
  p hoge      # hoge → Proc, &hoge → ブロック 
  p hoge.call # "test"
end
 
sample_method { "test" } # ブロックを引数に
```
