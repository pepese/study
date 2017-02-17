Spring Bootで最小アプリケーションが動くまでの設定についてまとめる。  
以下の２種類について記載する。

- Spring Bootで実行可能jarの作成
- Spring BootでAPサーバにデプロイ可能なwarの作成

上記は両方HTTP GETするとHelloが返ってくる簡単たRESTアプリ。

# Spring Bootで実行可能jarの作成

以下を作成する。

- pom.xml
  - Maven設定ファイル／POM
  - POMのdependencyに ```spring-boot-starter-*``` が含まれること
  - POMのbuild -> pluginsに ```spring-boot-maven-plugin``` が含まれること
    - ```mvn package``` 実行時に適切に実行可能jarを作成してくれる
- src/main/java/com/pepese/sample/Application.java
  - Spring Bootアプリケーション起動クラス
  - ```@SpringBootApplication``` アノテーションを付与すること
    - ```@Configuration``` 、 ```@EnableAutoConfiguration``` 、 ```@ComponentScan``` をまとめたもの
- src/main/java/com/pepese/sample/config/AppConfig.java
  - Spring設定クラス
    - DIやAOPなどの設定に使用する
  - ```@Configuration``` アノテーションを付与すること
- src/main/java/com/pepese/sample/config/WebMvcConfig.java
  - Spring MVC設定クラス
    - コンテンツネゴシエーションなどSpring MVCの```DispatcherServlet```に関する設定に使用する
  - ```WebMvcConfigurerAdapter``` が継承されていること
  - ```@Configuration``` アノテーションを付与すること
  - Spring Bootの場合、 ```@EnableWebMvc``` を付与する必要はない
    - Spring Bootを利用しないSpring MVCアプリケーションを作成する場合には付与する
- src/main/java/com/pepese/sample/controller/HelloController.java
  - 動作確認用のコントローラクラス
- src/main/java/com/pepese/sample/service/HelloService.java
  - DIなどの動作確認用のサービスクラス

## pom.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-sample-jar/pom.xml?footer=0"></script>

## Application.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-sample-jar/src/main/java/com/pepese/sample/Application.java?footer=0"></script>

## AppConfig.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-sample-jar/src/main/java/com/pepese/sample/config/AppConfig.java?footer=0"></script>

## WebMvcConfig.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-sample-jar/src/main/java/com/pepese/sample/config/WebMvcConfig.java?footer=0"></script>

## HelloController.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-sample-jar/src/main/java/com/pepese/sample/controller/HelloController.java?footer=0"></script>

## HelloService.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-sample-jar/src/main/java/com/pepese/sample/service/HelloService.java?footer=0"></script>

## 実行

以下の２通り。

- ```mvn spring-boot:run```で実行して```http://localhost:8080/```へアクセス
- ```mvn package```でビルドして```java -jar target/springboot-sample-jar-0.0.1-SNAPSHOT.jar```で実行して```http://localhost:8080/```へアクセス

# Spring BootでAPサーバにデプロイ可能なwarの作成

意味あんの？なツッコミは無しで。  
準備されたTomcatにデプロイしないといけないルールなプロジェクトは存在するんです。  
以下を作成する。「Spring Bootで実行可能jarの作成」と異なるファイルだけ記載する。

- pom.xml　（変更）
  - Maven設定ファイル／POM
  - POMのdependencyに ```spring-boot-starter-*``` が含まれること
  - POMのbuild -> pluginsに ```spring-boot-maven-plugin``` が含まれること
	  - ```mvn package``` 実行時に適切にデプロイ可能warを作成してくれる
  - POMのpackagingをwarにすること
  - POMのspring-boot-starter-tomcatの組み込み系APサーバのscopeをprovidedにすること
	  - tomcatに限らずjetty等組み込み系APサーバは全て
- src/main/java/com/pepese/sample/ServletInitializer.java　（追加）
  - web.xmlのSpring Boot版
  - ```SpringBootServletInitializer```が継承されていること
    - web.xmlのSpring MVC版である ```WebApplicationInitializer``` を継承している
  - ```configure``` メソッドをオーバーライドし、Spring Bootアプリケーション起動クラスをロードすること

## pom.xml

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-sample-war/pom.xml?footer=0"></script>

## ServletInitializer.java

<script src="http://gist-it.appspot.com/https://github.com/pepese/spring-sample/blob/master/springboot-sample-war/src/main/java/com/pepese/sample/ServletInitializer.java?footer=0"></script>

## 実行

以下の２通り。

- ```mvn spring-boot:run```で実行して```http://localhost:8080/```へアクセス
- ```mvn package```でビルドしてできたwarをTomcatにデプロイして```http://localhost:8080/springboot-sample-war/```へアクセス
    - コンテキストパスは自分で調整して

## 補足

```SpringBootServletInitializer``` はSpring Bootの起動クラスに継承して作成してもよい。下記の通り。

```java
package com.pepese.sample;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.context.web.SpringBootServletInitializer;

@SpringBootApplication
public class Application extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }
}
```
