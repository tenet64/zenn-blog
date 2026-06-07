---
title: "Rails7でActionCableを使用しリアルタイム性を実装"
emoji: "🐈"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Rails, ]
published: false
---

## 概要
ユーザーが起床ボタンを押した際、同じ「チャレンジ」に参加している他のユーザーの画面に、リロードなしで通知が表示されるようにする。

### ファイルの作成

```bash
docker compose exec web  rails g channel wake_up_channel
```
これで`wake_up_channnel.rb`と`wake_up_channnel.js`が作成される。


### wake_up_channnel.rbの実装
### wake_up_channnel.jsの実装