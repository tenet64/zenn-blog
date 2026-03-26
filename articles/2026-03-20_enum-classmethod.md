---
title: "RailsのEnumでのエラー：NoMethodError: undefined method `status' の原因と対策"
emoji: "🗂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Rails, Rspec]
published: true
---
## 現象
Railsでenumを定義しているモデルのテストを実行した際、以下のエラーが発生。
下記がモデルです。
```ruby
# model
  enum :status, { success: 0, failure: 1 }
```

```bash
NoMethodError:
  undefined method `status' for class WakeUpLog
```





