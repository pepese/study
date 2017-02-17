Spring Bootのテストについて書く。  
spring-boot-starter-testを使用するとコントローラのJUnitテストも可能になる。  
テストやコードインスペクションレポートのMaven設定は以下を参照。



[http://blog.pepese.com/entry/2017/02/17/161526:embed:cite]



# テスト対象アプリ

以下の記事で紹介した入門アプリをテスト対象とする。


[http://blog.pepese.com/entry/2016/12/19/182539:embed:cite]


# テストの作成

以下を作成する。

- com.pepese.sample.service.HelloServiceTest
    - コントローラへDIされるサービスクラスのテスト
- com.pepese.sample.controller.HelloControllerTest
    - コントローラのテスト

## サービスのテスト（com.pepese.sample.service.HelloServiceTest）

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-sample-jar/src/test/java/com/pepese/sample/service/HelloServiceTest.java?footer=0"></script>


ポイントは以下。

- ```@RunWith(SpringRunner.class)``` アノテーション
    - Spring BootでJUnitテストするときはコレをつける
- ```@SpringBootTest``` アノテーション
    - SpringのJava/XML Based Configurationなどの設定を読み込んでくれる

Springの設定（Java/XML Based Configuration）を読み込んでいるのでDIでテスト対象のインスタンスを取得できる。

## コントローラのテスト（com.pepese.sample.controller.HelloControllerTest）

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-sample-jar/src/test/java/com/pepese/sample/controller/HelloControllerTest.java?footer=0"></script>

ポイントは以下。

- ```@AutoConfigureMockMvc``` アノテーションをつけるとコントローラ層のモック（ ```MockMvc``` ）を作成でき、これでコントローラのJUnitテストが可能になる
   - ```@WebMvcTest(HelloController.class)``` としてコントローラクラスを指定することもできる
- ```@MockBean``` でコントローラ内でDIされるモジュールのモック（**Mockito**）を作成できる
- ```when``` メソッド等（**Mockito**）を使用してコントローラ内にDIされたモジュールの挙動を指定できる
- **Hamcrest**のMatchersを使用してassertする

## テストの実行

基本的には ```mvn test``` で実行可能だが、特定のクラスを指定してテストしたい場合は ```mvn clean test -Dtest=*.HelloControllerTest``` のように指定して実行できる（正規表現の指定可能）。  
また、 ```mvn clean test -Dtest=*.HelloControllerTest,*HelloServiceTest``` のようにカンマ区切りで複数指定することもできる。
