# 依存関係の設定

以下の2通り。

- ```spring-boot-starter-parent```
    - parentに指定
- ```spring-boot-dependencies```
    - parentが他の用途で上記が使えない場合はこっち


```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.4.2.RELEASE</version>
</parent>
```

```xml
<dependencyManagement>
     <dependencies>
        <dependency>
            <!-- Import dependency management from Spring Boot -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>1.4.2.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

# XML Based Configuration

Springの設定にXMLファイルを使用したい場合は以下のように```@Configuration```と```@ImportResource```を使用する。

```java
@SpringBootApplication
@Configuration
@ImportResource("servlet-context.xml")
public class App
{
    public static void main( String[] args )
    {
        SpringApplication.run(App.class, args);
    }
}
```


# 小ネタ

## @EnableAutoConfiguration

```@EnableAutoConfiguration```を使用すると、ClassPath内にあるSpringパッケージの@Configurationが付いたクラスが全てロードされています。  
嫌なときは以下のように除外設定する。

```java
import org.springframework.boot.autoconfigure.*;
import org.springframework.boot.autoconfigure.jdbc.*;
import org.springframework.context.annotation.*;

@Configuration
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
public class MyConfiguration {
}
```

## 外部のプロパティを読み込む場合

```java
@ConfigurationProperties(prefix="my")
```

# jarビルド後に依存ライブラリが入ってない問題、起動しない問題を解決

http://www.saka-en.com/java/spring-boot-mainifest-error/
