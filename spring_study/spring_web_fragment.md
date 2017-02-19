Spring MVCでjarの中に入っているjspをよむ方法。

以下の２つのMavenプロジェクトがあるとする。

- jspを含んだjarを読み込む方のWebプロジェクト
    - Spring BootかSpring MVCのプロジェクト
    - warか実行可能jar（Spring Boot）にアーカイブされる
    - 以降、「Webプロジェクト」と呼ぶ
- jspを含んだjarのプロジェクト
    - jspが含まれるjarにアーカイブされる
    - 以降、「jspプロジェクト」と呼ぶ

# Webプロジェクトでの設定

Spring MVCの設定。XML Based Configurationで書く。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xmlns:util="http://www.springframework.org/schema/util"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
    ">

    <context:property-placeholder
        location="classpath*:/META-INF/spring/*.properties" />

    <mvc:annotation-driven>
        <mvc:argument-resolvers>
            <bean
                class="org.springframework.data.web.PageableHandlerMethodArgumentResolver" />
            <bean
                class="org.springframework.security.web.method.annotation.AuthenticationPrincipalArgumentResolver" />
        </mvc:argument-resolvers>
        <!-- workarround to CVE-2016-5007. -->
        <mvc:path-matching path-matcher="pathMatcher" />
    </mvc:annotation-driven>

    <mvc:default-servlet-handler />

    <context:component-scan base-package="com.pepese" />

    <mvc:resources mapping="/resources/**"
        location="/resources/,classpath:META-INF/resources/"
        cache-period="#{60 * 60}" />

    <!-- Settings View Resolver. -->
    <mvc:view-resolvers>
        <mvc:bean-name />
        <mvc:tiles />
        <mvc:jsp prefix="/WEB-INF/views/" />
    </mvc:view-resolvers>

    <mvc:tiles-configurer>
        <mvc:definitions location="/WEB-INF/tiles/tiles-definitions.xml" />
    </mvc:tiles-configurer>

    <!-- Setting PathMatcher. -->
    <bean id="pathMatcher" class="org.springframework.util.AntPathMatcher">
        <property name="trimTokens" value="false" />
    </bean>

</beans>
```

ポイントは**View Resolver**の**prefix**の設定。  
ここでは```/WEB-INF/views/```になっている。

# jspを含んだjarのプロジェクトの設定

ポイントはjspをどこのディレクトリに置くか。

先述の**View Resolver**の**prefix**の設定が```/WEB-INF/views/```の場合、Mavenディレクトリ構成だと```src/main/resources/META-INF/resources/WEB-INF/views/```ディレクトリにjspを置く、  
**View Resolver**の**prefix**の設定が```/templates/```の場合、Mavenディレクトリ構成だと```src/main/resources/META-INF/resources/templates/```ディレクトリにjspを置く、  
といった具合になる。

# 注意事項

- 以下のようにjsp名が被った場合、Webプロジェクトのjspが勝つ
    - Webプロジェクト/src/main/webapp/WEB-INF/views/xxx.jsp
    - jarプロジェクト/src/main/resources/META-INF/resources/WEB-INF/views/xxx.jsp
