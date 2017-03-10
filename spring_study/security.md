[Spring Security](https://projects.spring.io/spring-security/)は認証・認可の機能を持つSpringのライブラリ。  
[Spring Session](http://projects.spring.io/spring-session/)を用いて **Redis** にセッションを格納する設定も試してみる。

ここでは、簡単なログイン画面でログインする機能を作成する。  
以下の構成で記載する。

- Java Configで設定する方法
  - Springの設定をJavaベースで設定する方法
- XML Configで設定する方法
  - Springの設定をXMLベースで設定する方法
- 共通作成物
  - Java Config、XML Configによらない共通の作成物

また、ここで紹介の作成物については以下参照のアプリケーションに対して、Thymeleaf画面・Spring Securityによる認証・Spring SessionによるRedisセッション管理機能を追加する。

[http://blog.pepese.com/entry/2016/12/19/182539:embed:cite]

[http://blog.pepese.com/entry/2016/12/22/173018:embed:cite]


# Java Configで設定する方法

## pom.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/pom.xml?footer=0"></script>

- ```org.springframework.boot:spring-boot-starter-security``` を使用しているのがポイント

## com.pepese.sample.config.WebSecurityConfig

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/src/main/java/com/pepese/sample/config/WebSecurityConfig.java?footer=0"></script>

- ```org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter``` を継承し、 ```@EnableWebSecurity``` アノテーションを付与する
- ```configure``` メソッドの引数により設定対象を変更できる
  - ```WebSecurity web``` ： Spring Security Filter Chain (springSecurityFilterChain) に関する設定が可能
    - ```web.Xxx()``` で様々設定できる。詳細は[ここ](http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/builders/WebSecurity.html)。
  - ```HttpSecurity http``` ： XML Configの ```http``` タグと同じ設定が可能
    - ```http.Xxx()``` で様々設定できる。詳細は[ここ](http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html)。
- ```configureGlobal``` メソッドで認証処理ロジックのサービスクラスをSpring Securityの認証マネージャに設定している    

## com.pepese.sample.config.SessionConfig

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/src/main/java/com/pepese/sample/config/SessionConfig.java?footer=0"></script>

- クラスに ```@EnableRedisHttpSession``` アノテーションを付与するとSpring Sessionの設定はほぼ完了
- ```connectionFactory``` メソッドで Redis へセッションを格納する設定
- ホスト名やポートは ```application.properties``` に設定する

# XML Configで設定する方法

## pom.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-thymeleaf-sample-xml/pom.xml?footer=0"></script>

## spring-mvc.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-thymeleaf-sample-xml/src/main/resources/META-INF/spring/spring-mvc.xml?footer=0"></script>

ThymeleafのView Resolverを設定。

## spring-security.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-thymeleaf-sample-xml/src/main/resources/META-INF/spring/spring-security.xml?footer=0"></script>

- ```<sec:http pattern="/resources/**" security="none" />```
  - Spring Session処理対象外の設定
- ```<sec:http>```
  - 認証周りの設定
- ユーザ認証処理ロジックのサービスクラス（ ```UserDetailsServiceImpl``` ）のBean定義
- ```<sec:authentication-manager>```
  - ```UserDetailsServiceImpl``` を認証マネージャに登録

## spring-session.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-thymeleaf-sample-xml/src/main/resources/META-INF/spring/spring-session.xml?footer=0"></script>

- ```RedisHttpSessionConfiguration``` のBean定義でSpring Sessionの設定
- ```LettuceConnectionFactory``` のBean定義でRedis接続の設定

## web.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springmvc-thymeleaf-sample-xml/src/main/webapp/WEB-INF/web.xml?footer=0"></script>

- ```contextConfigLocation``` に ```spring-security.xml``` 、 ```spring-session.xml``` を追加。
- Spring Securityの設定として ```springSecurityFilterChain``` という名前で ```org.springframework.web.filter.DelegatingFilterProxy``` をFilter定義しているのがポイント。
- Spring Sessionの設定として ```springSessionRepositoryFilter``` という名前で ```org.springframework.web.filter.DelegatingFilterProxy``` をFilter定義しているのがポイント。
  - さらに **Spring SecurityのFilter定義より上** に設定する必要がある

# 共通作成物

## com.pepese.sample.model.User

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/src/main/java/com/pepese/sample/model/User.java?footer=0"></script>

- 認証するユーザクラス
  - パスワードを固定にしているのはサンプルなので

## com.pepese.sample.service.security.UserDetails

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/src/main/java/com/pepese/sample/service/security/UserDetails.java?footer=0"></script>

- Spring Securityが提供するユーザ認証用のクラス（ ```org.springframework.security.core.userdetails.User``` ）を継承して作成
- 自作のUserクラスを認証対象のクラスとして設定する

## com.pepese.sample.service.security.UserDetailsServiceImpl

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/src/main/java/com/pepese/sample/service/security/UserDetailsServiceImpl.java?footer=0"></script>

- Spring Securityが提供するユーザ認証用サービスクラス（ ```org.springframework.security.core.userdetails.UserDetailsService``` ）を継承して作成
- 本来なら認証対象のユーザデータをDBなどから取得して突合するが、サンプルということで new してる

## application.properties

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/src/main/resources/application.properties?footer=0"></script>

## com.pepese.sample.controller.HelloController

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/src/main/java/com/pepese/sample/controller/HelloController.java?footer=0"></script>

- ```@AuthenticationPrincipal``` でログイン済みのユーザ情報を取得できる
- なお、 ```/logout``` のパス・ロジックは Spring Security にてもつ

## index.html

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/src/main/resources/templates/index.html?footer=0"></script>

Viewは、Java Configの場合は ```resources/templates``` 配下、XML Configの場合は ```WEB-INF/templates``` 配下に配置する。

## login.html

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/src/main/resources/templates/login.html?footer=0"></script>

## error.html

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-thymeleaf-sample-jar/src/main/resources/templates/error.html?footer=0"></script>
