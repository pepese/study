Spring MVCのテストについて書く。  
spring-testを使用するとコントローラのJUnitテストも可能になる。  
テストやコードインスペクションレポートのMaven設定は以下を参照。



[http://blog.pepese.com/entry/2017/02/17/161526:embed:cite]



# テスト対象アプリ

以下の記事で紹介した入門アプリをテスト対象とする。

[http://blog.pepese.com/entry/2016/12/22/173018:embed:cite]

# テストの作成

以下を作成する。

- com.pepese.sample.service.HelloServiceTest
    - コントローラへDIされるサービスクラスのテスト
- com.pepese.sample.controller.HelloControllerTest
    - コントローラのテスト

## サービスのテスト（com.pepese.sample.service.HelloServiceTest）

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-java/src/test/java/com/pepese/sample/service/HelloServiceTest.java?footer=0"></script>

ポイントは以下。

- ```@RunWith(SpringJUnit4ClassRunner.class)``` アノテーション
    - SpringでJUnitテストするときはコレをつける
- ```@ContextConfiguration``` アノテーション
    - SpringのJava/XML Based Configurationを指定する
    - ```@ContextConfiguration(classes = {AppConfig.class, WebMvcConfig.class})```
    - ```@ContextConfiguration(locations = { "file:src/main/resources/META-INF/spring/spring-mvc.xml" })```
- ```@WebAppConfiguration``` アノテーション
    - Webアプリケーションのテストの際に付与する

Springの設定（Java/XML Based Configuration）を読み込んでいるのでDIでテスト対象のインスタンスを取得できる。

## コントローラのテスト（com.pepese.sample.controller.HelloControllerTest）

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-java/src/test/java/com/pepese/sample/controller/HelloControllerTest.java?footer=0"></script>

ポイントは以下。

- テスト対象となるコントローラクラスを指定して、コントローラのモック（ ```MockMvc``` ）を作成できる
- モック（**Mockito**）を使用するためにルール（```@Rule```）を宣言する
- ```@Mock``` でモックオブジェクトがDIされる
- ```when``` メソッド等（**Mockito**）を使用してコントローラ内にDIされたモジュールの挙動を指定できる
- **Hamcrest**のMatchersを使用してassertする

## テストの実行

基本的には ```mvn test``` で実行可能だが、特定のクラスを指定してテストしたい場合は ```mvn clean test -Dtest=*.HelloControllerTest``` のように指定して実行できる（正規表現の指定可能）。  
また、 ```mvn clean test -Dtest=*.HelloControllerTest,*HelloServiceTest``` のようにカンマ区切りで複数指定することもできる。

# 参考

http://docs.spring.io/spring/docs/current/spring-framework-reference/html/mvc.html
