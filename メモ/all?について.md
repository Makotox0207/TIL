# all?

[Enumerable#all? (Ruby 3.0.0 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/all=3f.html)

> すべての要素が真である場合に true を返します。
> 偽である要素があれば、ただちに false を返します。
> ブロックを伴う場合は、各要素に対してブロックを評価し、すべての結果が真である場合に true を返します。
> ブロックが偽を返した時点で、ただちに false を返します。

```ruby
# すべて正の数か？
p [5,  6, 7].each.all? {|v| v > 0 }   # => true
p [5, -1, 7].each.all? {|v| v > 0 }   # => false
p [].each.all? {|v| v > 0 }           # => true

```
