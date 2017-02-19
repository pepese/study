- [Spring Bootの外部設定値の扱い方を理解する](http://qiita.com/kazuki43zoo/items/0ce92fce6d6f3b7bf8eb)

[Spring Bootでのプロパティ設定方法](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-external-config)は、以下の方法がある。

1. Devtools global settings properties on your home directory (~/.spring-boot-devtools.properties when devtools is active).
1. @TestPropertySource annotations on your tests.
1. @SpringBootTest#properties annotation attribute on your tests.
1. Command line arguments.
1. Properties from SPRING_APPLICATION_JSON (inline JSON embedded in an environment variable or system property)
1. ServletConfig init parameters.
1. ServletContext init parameters.
1. JNDI attributes from java:comp/env.
1. Java System properties (System.getProperties()).
1. OS environment variables.
1. A RandomValuePropertySource that only has properties in random.\*.
1. Profile-specific application properties outside of your packaged jar (application-{profile}.properties and YAML variants)
1. Profile-specific application properties packaged inside your jar (application-{profile}.properties and YAML variants)
1. Application properties outside of your packaged jar (application.properties and YAML variants).
1. Application properties packaged inside your jar (application.properties and YAML variants).
1. @PropertySource annotations on your @Configuration classes.
1. Default properties (specified using SpringApplication.setDefaultProperties).
