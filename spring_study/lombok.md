[Lombok](https://projectlombok.org)は、POJO（Jave Bean？）作成を省力化してくれるアレ。  

# 主要なアノテーション

- @AllArgsConstructor
  - クラスアノテーション
  - すべてのフィールドを引数に持つコンストラクタが自動生成される
- @Data
  - クラスアノテーション
  - @Getter @Setter @RequiredArgsConstructor @ToString @EqualsAndHashCode を全てつけたのと同じ
- @EqualsAndHashCode
  - クラスアノテーション
  - ```equals()``` 、 ```hashCode()``` が自動生成される
- @Getter / @Setter
  - クラス、フィールドアノテーション
  - ゲッター、セッターが自動生成される
- @NoArgsConstructor
  - クラスアノテーション
  - 引数無しのコンストラクタが自動生成される
- @NonNull
  - メソッド引数のアノテーション
  - nullチェックが自動生成される
- @RequiredArgsConstructor
  - クラスアノテーション
  - final修飾されたフィールドだけを引数に持つコンストラクタが自動生成される
- @ToString
  - クラスアノテーション
  - ```toString()``` が自動生成される

## @AllArgsConstructor

```java
public class Person {
  private String name;
  private Integer age;

  // 以降、自動作成されるコード

  public Person(String name, Integer age) {
    this.name = name;
    this.age = age;
  }
}
```

# ロガーのアノテーション

- @Slf4j
  - クラスアノテーション
  - log という名前の static final なロガーが自動生成される

```@Slf4j``` 以外にも以下がある。

|annotation|logger|
|:---|:---|
|@CommonsLog|org.apache.commons.logging.Log|
|@Log|org.apache.commons.logging.Log|
|@Log4j|org.apache.log4j.Logger|
|@Log4j2|org.apache.logging.log4j.Logger|
|@Slf4j|org.slf4j.Logger|
|@XSlf4j|org.slf4j.ext.XLogger|
