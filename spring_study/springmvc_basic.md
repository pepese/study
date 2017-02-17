Spring MVCで最小アプリケーションが動くまでの設定についてまとめる。  
以下の２種類で同じアプリを実装する。

- Java Based Configuration
- XML Based Configuration

上記は両方HTTP GETするとHelloが返ってくる簡単たRESTアプリ。  
Spring BOM 2.0.8.RELEASEで動確した。

# Java Based Configuration

以下を作成する。

- pom.xml
- src/main/java/com/pepese/sample/config/AppConfig.java
  - Spring設定クラス
    - DIやAOPなどの設定に使用する
  - ```@Configuration``` アノテーションを付与すること
  - 必要に応じて```@ComponentScan```を指定してコンポーネントスキャン領域を指定すること
- src/main/java/com/pepese/sample/config/WebMvcConfig.java
  - Spring MVC設定クラス
    - コンテンツネゴシエーションなどSpring MVCの```DispatcherServlet```に関する設定に使用する
  - ```WebMvcConfigurerAdapter``` が継承されていること
  - ```@Configuration``` アノテーションを付与すること
  - ```@EnableWebMvc``` アノテーションを付与すること
- src/main/java/com/pepese/sample/config/WebInitializer.java
  - web.xmlのJava Based Configuration
  - ```AbstractAnnotationConfigDispatcherServletInitializer```を継承すること
    - ```WebApplicationInitializer```の実装クラス
- src/main/java/com/pepese/sample/controller/HelloController.java
  - 動作確認用のコントローラクラス
- src/main/java/com/pepese/sample/service/HelloService.java
  - DIなどの動作確認用のサービスクラス

## pom.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-java/pom.xml?footer=0"></script>

## AppConfig.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-java/src/main/java/com/pepese/sample/config/AppConfig.java?footer=0"></script>

## WebMvcConfig.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-java/src/main/java/com/pepese/sample/config/WebMvcConfig.java?footer=0"></script>

## WebInitializer.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-java/src/main/java/com/pepese/sample/config/WebInitializer.java?footer=0"></script>

## HelloController.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-java/src/main/java/com/pepese/sample/controller/HelloController.java?footer=0"></script>

## HelloService.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-java/src/main/java/com/pepese/sample/service/HelloService.java?footer=0"></script>

```mvn clean package``` してTomcatにデプロイすれば動く。

# XML Based Configuration

以下を作成する。Java Based Configurationと異なるファイルだけ記載する。  
以下は不要になるファイル。

- src/main/java/com/pepese/sample/config/AppConfig.java
- src/main/java/com/pepese/sample/config/WebMvcConfig.java
- src/main/java/com/pepese/sample/config/WebInitializer.java

以下は追加するファイル。

- src/main/resources/META-INF/spring/applicationContext.xml
- src/main/resources/META-INF/spring/spring-mvc.xml
- src/main/webapp/WEB-INF/web.xml

## applicationContext.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-xml/src/main/resources/META-INF/spring/applicationContext.xml?footer=0"></script>

## spring-mvc.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-xml/src/main/resources/META-INF/spring/spring-mvc.xml?footer=0"></script>

## web.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-sample-xml/src/main/webapp/WEB-INF/web.xml?footer=0"></script>

```mvn clean package``` してTomcatにデプロイすれば動く。
