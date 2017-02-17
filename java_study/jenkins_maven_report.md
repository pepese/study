JenkinsでMavenレポートを出力する。

MavenとJenkinsを使ってJavaのレポート出力をやってみた。  
下記を読んだテイで書く。


[http://blog.pepese.com/entry/20150331/1427803582:embed:cite]


# Maven

```mvn clean test site``` をたたいてHTML形式のレポートを出力する親POMを下記に作った。

## 親POM

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/pom.xml?footer=0"></script>

※2017年2月17日時点での最新バージョンにしている。  
※ ```javancss-maven-plugin``` はJava8未対応だったためコメントアウト。

## プラグインの概要

```reporting``` タグの部分だけ記載する。

- ```org.apache.maven.plugins:maven-site-plugin```
  - ```mvn site``` コマンドの実装
  - 以下の「<strong>Project Information</strong>」を出力する
    - ```Dependencies``` ：依存ライブラリの一覧
    - ```Dependency Convergence``` ：同一依存ライブラリの全てのバージョンを集約して表示
    - ```Dependency Information``` ：このプロジェクトの依存の書き方
    - ```Dependency Management``` ：最終的に適用される依存ライブラリの一覧
    - ```Distribution Management``` ：配布するリポジトリの情報
    - ```About``` ：POMのdescriptionの記載
    - ```Plugin Management``` ：最終的に適用されるプラグインの一覧
    - ```Project Plugins``` ：buildおよびreportingに使用されるプラグインの一覧
    - ```Project Summary``` ：プロジェクト情報のサマリ
- ```org.apache.maven.plugins:maven-javadoc-plugin```
  - ```Project Reports``` の「JavaDocs」および「Test JavaDocs」を出力する
- ```org.apache.maven.plugins:maven-surefire-report-plugin```
  - ```Project Reports``` の「Surefire Report」を出力する
    - テストの件数、エラー数、失敗数等を出力する
- ```org.apache.maven.plugins:maven-jxr-plugin```
  - ```Project Reports``` の「Source Xref」および「Test Source Xref」を出力する
    - JavaソースコードをHTML形式で出力する
- ```org.apache.maven.plugins:maven-pmd-plugin```
  - ```Project Reports``` の「PMD」を出力する
    - [PMD](http://pmd.sourceforge.net/)はJavaコードを分析して潜在的なバグを探すツール
- ```org.apache.maven.plugins:javancss-maven-plugin```
  - ```Project Reports``` の「JavaNCSS」を出力する
    - [JavaNCSS](http://javancss.codehaus.org/)はJavaコードの品質や複雑度に関するメトリクスを出力する
- ```org.jacoco:jacoco-maven-plugin```
  - ```Project Reports``` の「JaCoCo」を出力する
    - **JaCoCo** による単体試験のカバレッジ測定結果
- ```org.apache.maven.plugins:maven-checkstyle-plugin```
  - ```Project Reports``` の「Checkstyle」を出力する
- ```org.codehaus.mojo:findbugs-maven-plugin```
  - ```Project Reports``` の「FindBugs」を出力する
- ```org.apache.maven.plugins:maven-project-info-reports-plugin```
  - プロジェクトの情報を出力する
    - cim、help、issue-tracking、license等

以上の情報がHTML形式で ```target/site``` 配下へ出力される。

## ```testFailureIgnore``` について

親POMのbuild⇒plugins⇒pluginのmaven-surefire-pluginの設定で ```testFailureIgnore``` を *true* にするとテストでコケてもレポート出力するようになる。  
と、同時に **```mvn test``` でコケてもSUCCESS** となってしまう。  
こうなってくると、Jenkinsで **DailyTestで失敗しても気づかない** ことになってしまう。  
なので、 ```testFailureIgnore``` はPOMで設定せず、以下のようにするといい。

- JenkinsによるDailyTest
  - ```mvn test -Dmaven.test.failure.ignore=false```
    - これでテストでコケたら **FAILURE** になる（ちなみにデフォルトfalse）
- Jenkinsによるレポート出力
  - ```mvn clean test site -Dmaven.test.failure.ignore=true```
    - これでテストでコケても **SUCCESS** になり、レポートが出力される


# Jenkins

Jenkinsのジョブで ```mvn clean test site``` をした後、Jenkinsの ```HTML Publisher Plugin``` で下記のように設定すればJenkinsのジョブのページにSiteで出力したレポートへのリンクが作成される。

- ```Publish HTML reports``` ：チェックボックスをオン
  - HTML directory to archive
    - 例） ```target/site/```
  - Index page[s]
    - 例） ```index.html```
  - Report title
    - 例） ```HTML Report```

レポートの表示が崩れている（CSS/JSが適用されていない様子）場合は下記を参照。  

[http://blog.pepese.com/entry/2016/02/02/095811:embed:cite]


また、今回Mavenで出力しなかったJavaコード解析情報について、下記のJenkinsプラグインを使用するとカバーできる。

- DRY Plugin
  - コピペコードのような重複したコードをチェックしてくれる
- Task Scanner Plugin
  - ソースコードを任意の文字列で検索するチェックをかけることができる
    - FIXME、TODO、XXX、deprecated、System.out.printlnとかスキャンしてくれる
- Warnings Plugin
  - コンパイラの警告をチェックしてくれる
- Step Counter Plugin
  - ファイル行数をカウントしてくれる

<table border="0">
<tr>
<td>
<iframe src="https://rcm-fe.amazon-adsystem.com/e/cm?t=tanakakns-22&o=9&p=8&l=as1&asins=4774174238&ref=qf_sp_asin_til&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
</td>
<td>
<iframe src="https://rcm-fe.amazon-adsystem.com/e/cm?t=tanakakns-22&o=9&p=8&l=as1&asins=4873115345&ref=qf_sp_asin_til&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
</td>
<td>
<iframe src="https://rcm-fe.amazon-adsystem.com/e/cm?t=tanakakns-22&o=9&p=8&l=as1&asins=404886324X&ref=qf_sp_asin_til&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
</td>
</tr>
</table>
