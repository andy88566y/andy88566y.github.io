---
title: Leetcode Ruby Practice(1)
subtitle: Ruby is fun
---
Ruby

ruby has no increment operators(x++)
simply write x += 1


Rails

1.
destroy 是刪除model，並不會刪除資料庫，要.save後才會把結果存進資料庫(??)
delete 是刪除資料庫中的紀錄

不太懂

2.
Model Scopes是一項非常酷的功能，它可以將常用的查詢條件宣告起來，讓程式變得乾淨易讀，更厲害的是可以串接使用。
https://ihower.tw/rails/activerecord-query.html#sec6

3.
class Order::Ingroup < ApplicationRecord
  has_many :fees_ingroups, dependent: :delete_all

:nullify 這是預設值，不會幫忙刪除attendees，但會把attendees的外部鍵event_id都設成NULL
