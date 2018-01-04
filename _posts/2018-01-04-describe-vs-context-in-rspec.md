---
title: describe vs context in Rspec
subtitle: 釐清Rspec中describe context的用法差異
categories: Ruby Rspec
---

釐清Rspec中describe context的用法差異

要寫RoR一定要寫 Rspec，確保程式的正確性以及節省測試的時間
說來慚愧，寫到現在還是看這tsn的code學習模仿複製貼上
所以一直沒搞懂describe context之間的差異
今天好好來研究一下

看一下以下這段code:
```ruby
describe "launch the rocket" do
  context "all ready" do
  end

  context "not ready" do
  end
end
```

那麼“describe” 和 “context” 的差異到底在哪裡？?

根據rspec source code: “context” is just a alias method of “describe”

所以兩者其實“功能上”是沒差的，但“可讀性”就差很多了，用得好可以讓別人甚至自己看spec的時候更好懂

"context" 用在同一個狀態下，去測一系列功能
"describe" 用在單一功能，裡面會有一系列相關的測試

這裡的功能並非是methods，而是看你想測試目標的行為表現

舉一個工作上實際的例子：

```ruby
describe "合併物品成為單一費用" do
  before(:each) do
    #prepare two items for all of the tests
    @bed = Item.new # NTD 10,000
    @pillow = Item.new # NTD 500
    @sample_items = Item.new(fee: 10,500)
  end

  context "皆已付費" do
    before(:each) do
      #under the state of ready
      @bed.ready = true
      @pillow.ready = true
    end
    it "可以合併" do
      @total_items = @bed.merge(@pillow)
      expect (@total_items).should be(@sample_items)
    end
  end

  context "其中有商品未付費" do
    before(:each) do
      #under the state of NOT ready
      @bed.ready = true
      @pillow.ready = false
    end
    it "不可合併，引發錯誤" do
      expect { @total_items = @bed.merge(@pillow)}.to raise_error
    end
  end
end
```

是的，測試可以寫中文
畢竟中文是母語，比較不會混淆（還可以寫科科、幹、太神啦...等等）
如果團隊中有人英文沒那麼好，寫中文真的不是壞事

以上這樣寫，比起你寫這樣寫好得多：

```ruby
describe "合併物品成為單一費用，皆已付費，可以合併" do
  ...
end

describe "合併物品成為單一費用，有些未付費，不能合併" do
  ...
end
```

當我們在讀測試的時候，看到describe就知道以下所有context都在測試同一件事
而context是在創造不同的環境，去測試結果是否符合預期。

原文：[thanks to](http://lmws.net/describe-vs-context-in-rspec)

