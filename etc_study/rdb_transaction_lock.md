RDBの排他制御について
===

# トランザクション

**トランザクション分離レベル**　と起こりうる現象は以下の表の通り。

|分離レベル|DRTIY READ|NON-REPEATABLE READ|PHANTOM READ|
|:---|:---|:---|:---|
|READ UNCOMMITTED|○|○|○|
|READ COMMITTED  |×|○|○|
|REPEATALBE READ |×|×|○|
|SERIALIZABLE    |×|×|×|

## トランザクション分離レベル

- READ UNCOMMITTED
  - REPEATALBE READの制約に加え、未コミットのデータを読み込む
- READ COMMITTED
  - REPEATALBE READの制約に加え、コミット済のデータを読み込む
- REPEATALBE READ
  - あるトランザクションの更新（既に存在するレコードの削除を除く変更）処理が完了・コミットするまで、他のトランザクションは更新処理できない
  - 他のトランザクションからのレコード追加・削除が可能なため、PHANTOM READが発生し得る
- SERIALIZABLE
  - トランザクションは並列実行不可能
  - あるトランザクション（CRUD処理）が完了するまで他のトランザクションは待ち

## 起こりうる現象

- ダーティ・リード（DRITY READ）
  - クエリ発行済みだが、まだコミットされていないデータを読む
- 再読込不可能読取（NON-REPEATABLE READ）
  - 同一トランザクション内で同じデータを2回読み込む場合、1回目と2回目で読み込むデータが異なってしまう
- ファントム・リード（PHANTOM READ）
  - 同一トランザクション内で同じデータを2回読み込む場合、1回目と2回目で読み込むデータのレコード数が異なってしまう
