---
title: Ruby map with if next
categories: Ruby map if next
---

混用 map if next時造成的bug
今天文章好像寫太多了品質不佳，但這又最重要，決定簡單扼要講

下午都在抓一個奇怪的bug

發現array 經過一個map後會變成空陣列

抓來抓去發現是這個：

```ruby
["one", 2, "three"].map {|s|
  if s.is_a? String
    next
  end
  s*2
}
```

原本期望這種結果
```ruby
["one", 4, "three"]
```

但實際上，map是把整個block執行的結果配到對應的陣列中
next會把剩下的block跳過，結果變成沒有回傳值
所以真正的結果是：
```ruby
[nil, 4, nil]
```

如果想要期待的結果，應該在next後加上你想要填的東西
```ruby
["one", 2, "three"].map {|s|
  if s.is_a? String
    next s
  end
  s*2
}
```
這樣會正確得到：
```ruby
["one", 4, "three"]
```

更多例子：
```ruby
[1, 2, 3].map {|n| next if n.even?; n*2} # => [2, nil, 6]
[1, 2, 3].map {|n| next nil if n.even?; n*2} # => [2, nil, 6]
[1, 2, 3].map {|n| next "skipped" if n.even?; n*2} # => [2, "skipped", 6]
```
