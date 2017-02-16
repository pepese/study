# ブランチの作成からPR完了まで

```bash
$ git branch <branchname>
```

引数を指定せずに実行すると現在のブランチ（\*がついてる）が確認できる。  
また、ブランチの切り替えは以下。

```bash
$ git checkout <branch>
```

一連の流れは以下。

 ```bash
$ git branch issue1
$ git branch
  issues1
* master
$ git checkout issue1
* issues1
  master
```

PRマージ後の作業

```bash
$ git checkout master
$ git branch -d issue1      #ローカルブランチの削除
$ git push origin issue1   #リモートブランチの削除←これが必要か不明
```

# ファイルの変更を戻す

以下でコミットログのハッシュ値を取得。

```bash
git log [ファイルパス]
```

以下でコミットのハッシュ値を指定して戻す。

```bash
$ git checkout [コミット番号] [ファイルパス]
```

# 特定のコミットをマージする

以下でリモートリポジトリの変更をローカルリポジトリへ持ってくる。（ワーキングディレクトリの変更は行われない）

```bash
$ git fetch
```

コミットログのハッシュ値を指定してマージする。

```bash
$ git cherry-pick [ハッシュ値]
```

# ブランチをマージする

issue1ブランチにmasterをマージしたい場合。

```bash
$ git checkout issue1
$ git merge master
```

# ファイル、ディレクトリをgitの管理対象から外す

## ファイルをgitの管理対象から外して且削除

```sh
$ git rm [削除したいファイル]
```

## ファイルをgitの管理対象から外すがファイルを削除しない

```sh
$ git rm —-cached [削除したいファイル]
```

## ディレクトリをgitの管理対象から外して且削除

```sh
$ git rm -r [削除したいディレクトリ]
```
