Controllerの処理の中でジョブを非同期実行する仕組み。

Active Jobの作成方法は以下。

```ruby
rails generate job data_cleanup
```

```ruby
class GuestsCleanupJob < ApplicationJob
  queue_as :default

  def perform(*args)
    # 後で実行したい作業をここに書く
  end
end
```

以下のように任意のオンライン処理の中で実行登録可能。

```ruby
# 「キューイングシステムが空いたらジョブを実行する」とキューに登録する
MyJob.perform_later record
```


```ruby
# 明日正午に実行したいジョブをキューに登録するß
MyJob.set(wait_until: Date.tomorrow.noon).perform_later(record)
```


```ruby
# 一週間後に実行したいジョブをキューに登録する
MyJob.set(wait: 1.week).perform_later(record)
```
