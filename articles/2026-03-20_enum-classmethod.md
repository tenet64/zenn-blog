---
title: "RSpecのモデルスペックでenumを単数形で記述したことによるエラーと解決策"
emoji: "🗂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Rails,RSpec]
published: true
---
## エラー内容
Railsでenumを定義しているモデルのテストを実行した際、突然以下のエラーが発生。

```Bash
NoMethodError:
  undefined method `status' for class WakeUpLog
```

該当のモデルがこちら
```rb
# 該当のmodel
enum :status, { success: 0, failure: 1 }
```
テスト内容はこちら

```rb
  # マルチ: 全員が成功した場合のみ、全員に1ポイントを付与
  def try_grant_multi_points
    # Challenge#membersが「ホスト + 参加者」を返す前提
    members = challenge.members
    all_member = members.size
    # メンバーが0ならここで終了
    return if all_member.zero?

    all_success = members.all? do |member|
      member.wake_up_logs.exists?(
        challenge_id: challenge.id,
        target_date: target_date,
        status: WakeUpLog.status[:success]
      )
    end
```

## 調査
git diff で過去の変更履歴を遡ったところ、以下の修正が原因っぽい。

```Diff
- status: WakeUpLog.statuses[:success]
+ status: WakeUpLog.status[:success]
```

## 原因【なぜエラーになったのか？】
Railsの enum を定義すると、モデルクラスに対して statuses（複数形） というクラスメソッドが自動的に生成。

今回誤って単数形で書き直したこと原因。

## 解決
クラスメソッドとして呼び出すために、複数形の statuses を使用するように再修正
```rb
  # マルチ: 全員が成功した場合のみ、全員に1ポイントを付与
  def try_grant_multi_points
    # Challenge#membersが「ホスト + 参加者」を返す前提
    members = challenge.members
    all_member = members.size
    # メンバーが0ならここで終了
    return if all_member.zero?

    all_success = members.all? do |member|
      member.wake_up_logs.exists?(
        challenge_id: challenge.id,
        target_date: target_date,
        status: WakeUpLog.statuses[:success]
      )
    end
```

## まとめ
Railsの規約を再確認　→　enum は複数形のメソッドを作るという仕様を理解しておく。