# SpringBoot

## 1. SpringBoot简介

### 1.1  Spring优缺点

#### 1.1.1 优点

依赖注入(IOC+DI)和面向切面(AOP)编程,使代码解耦,更加的灵活和易于扩展

#### 1.1.2 缺点

* Spring代码是轻量级的，但它的配置(依赖+applicationContext.xml)却是重量级的。
* 项目的依赖管理耗时耗力

### 1.2 SpringBoot概述

SpringBoot就是Spring,它做了那些没有它你也会去做得配置和添加依赖.让开发者专注于应用程序的逻辑处理.

#### 1.2.2 特点

- 为基于Spring的开发提供更快的入门体验
- 开箱即用，没有代码生成，也无需XML配置。同时也可以修改默认值来满足特定的需求
- 提供了一些大型项目中常见的非功能性特性，如嵌入式服务器、安全、指标，健康检测、外部配置等
- SpringBoot不是对Spring功能上的增强，而是提供了一种快速使用Spring的方式

#### 1.2.3 核心功能

- 起步依赖

  起步依赖本质上是一个Maven项目对象模型（Project Object Model，POM），定义了对其他库的传递依赖，这些东西加在一起即支持某项功能。

  简单的说，起步依赖就是将具备某种功能的坐标打包到一起，并提供一些默认的功能。

- 自动配置

  Spring Boot的自动配置是一个运行时（更准确地说，是应用程序启动时）的过程，考虑了众多因素，才决定Spring配置应该用哪个，不该用哪个。该过程是Spring自动完成的。

## 2. SpringBoot快速入门

### 2.1 Maven创建

#### 2.1.1 添加SpringBoot的起步依赖

项目要继承SpringBoot的起步依赖spring-boot-starter-parent

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.1.RELEASE</version>
</parent>
```

SpringBoot要集成SpringMVC进行Controller的开发，需要导入web的启动依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

#### 2.1.2 编写SpringBoot引导类

要通过SpringBoot提供的引导类起步SpringBoot才可以进行访问

```java
package com.goes;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MySpringBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class);
    }

}
```

#### 2.1.3 编写Controller

在引导类MySpringBootApplication同级包或者子级包中创建QuickStartController

```java
package com.goes.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class QuickStartController {
    
    @RequestMapping("/quick")
    @ResponseBody
    public String quick(){
        return "springboot 访问成功!";
    }
    
}
```

#### 2.1.5 测试

执行SpringBoot起步类的主方法，控制台打印日志如下：

```
.   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.0.1.RELEASE)

2018-05-08 14:29:59.714  INFO 5672 --- [           main] com.itheima.MySpringBootApplication      : Starting MySpringBootApplication on DESKTOP-RRUNFUH with PID 5672 (C:\Users\muzimoo\IdeaProjects\IdeaTest\springboot_quick\target\classes started by muzimoo in C:\Users\muzimoo\IdeaProjects\IdeaTest)
... ... ...
o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-05-08 14:30:03.126  INFO 5672 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2018-05-08 14:30:03.196  INFO 5672 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2018-05-08 14:30:03.206  INFO 5672 --- [           main] com.itheima.MySpringBootApplication      : Started MySpringBootApplication in 4.252 seconds (JVM running for 5.583)
```

访问:[http://localhost:8080/quick](http://localhost:8080/quick)



### 2.2 快速入门解析

#### 2.2.1 SpringBoot代码解析

- @SpringBootApplication：标注SpringBoot的启动类，该注解具备多种功能（后面详细剖析）
- SpringApplication.run(MySpringBootApplication.class) 代表运行SpringBoot的启动类，参数为SpringBoot启动类的字节码对象

#### 2.2.2 SpringBoot工程热部署

在开发中反复修改类、页面等资源，每次修改后都是需要重新启动才生效，因此想要在修改代码后不重启就能生效，在 pom.xml 中添加如下配置就可以实现这样的功能，我们称之为热部署。

```xml
<!--热部署配置-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

注意：IDEA进行SpringBoot热部署失败原因

其根本原因是因为Intellij IEDA默认情况下不会自动编译，需要对IDEA进行自动编译的设置。

![image-20240503170801855](images/image-20240503170801855.png)

#### 2.2.3 使用idea快速创建SpringBoot项目

在SpringBoot项目的pom.xml中已经导入web的起步依赖的坐标

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.goes</groupId>
	<artifactId>springboot_quick</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>springboot_quick</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.1.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>9</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>


</project>

```



### 2.3 打包部署

添加`spring-boot-maven-plugin`插件

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

#### 2.3.1 打成可执行jar包

1. 打包

```shell
mvn package
```

2. 执行jar

```shell
java -jar xxx.jar
```

#### 2.3.2 打成war包

1. 在pom.xml里设置 `<packaging>war</packaging>`

2. 移除嵌入式tomcat插件

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
       <!-- 移除嵌入式tomcat插件 -->
       <exclusions>
           <exclusion>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-tomcat</artifactId>
           </exclusion>
       </exclusions>
   </dependency>
   ```

3. 添加servlet-api依赖

   ```xml
   <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>javax.servlet-api</artifactId>
       <version>3.1.0</version>
       <scope>provided</scope>
   </dependency>
   ```

4. 修改启动类，并填写初始化方法

   ```java
   public class SpringBootStartApplication extends SpringBootServletInitializer {
    
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
           // 注意这里要指向原先用main方法执行的Application启动类
           return builder.sources(DemoApplication.class);
       }
   }
   ```

   ```java
   @SpringBootApplication
   public class DemoApplication {
   
   	public static void main(String[] args) {
   		SpringApplication.run(DemoApplication.class, args);
   	}
   
   }
   ```

5. 打包部署

   ```shell
   mvn package
   ```

> 注意:打war包时，原先的启动类上的注解@SpringBootApplication是需要的!



## 3. SpringBoot原理分析

### 3.1 起步依赖原理分析

#### 3.1.1 分析spring-boot-starter-parent

spring-boot-starter-parent的pom.xml，xml配置如下（只摘抄部分重点配置）：

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-dependencies</artifactId>
  <version>2.0.1.RELEASE</version>
  <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

spring-boot-starter-dependencies的pom.xml，xml配置如下（只摘抄部分重点配置）：

```xml
<properties>
  	<activemq.version>5.15.3</activemq.version>
  	<antlr2.version>2.7.7</antlr2.version>
  	<appengine-sdk.version>1.9.63</appengine-sdk.version>
  	<artemis.version>2.4.0</artemis.version>
  	<aspectj.version>1.8.13</aspectj.version>
  	<assertj.version>3.9.1</assertj.version>
  	<atomikos.version>4.0.6</atomikos.version>
  	<bitronix.version>2.1.4</bitronix.version>
  	<build-helper-maven-plugin.version>3.0.0</build-helper-maven-plugin.version>
  	<byte-buddy.version>1.7.11</byte-buddy.version>
  	... ... ...
</properties>
<dependencyManagement>
  	<dependencies>
      	<dependency>
        	<groupId>org.springframework.boot</groupId>
        	<artifactId>spring-boot</artifactId>
        	<version>2.0.1.RELEASE</version>
      	</dependency>
      	<dependency>
        	<groupId>org.springframework.boot</groupId>
        	<artifactId>spring-boot-test</artifactId>
        	<version>2.0.1.RELEASE</version>
      	</dependency>
      	... ... ...
	</dependencies>
</dependencyManagement>
<build>
  	<pluginManagement>
    	<plugins>
      		<plugin>
        		<groupId>org.jetbrains.kotlin</groupId>
        		<artifactId>kotlin-maven-plugin</artifactId>
        		<version>${kotlin.version}</version>
      		</plugin>
      		<plugin>
        		<groupId>org.jooq</groupId>
        		<artifactId>jooq-codegen-maven</artifactId>
        		<version>${jooq.version}</version>
      		</plugin>
      		<plugin>
        		<groupId>org.springframework.boot</groupId>
        		<artifactId>spring-boot-maven-plugin</artifactId>
        		<version>2.0.1.RELEASE</version>
      		</plugin>
          	... ... ...
    	</plugins>
  	</pluginManagement>
</build>
```

从上面的spring-boot-starter-dependencies的pom.xml中可以发现，一部分坐标的版本、依赖管理、插件管理已经定义好，所以SpringBoot工程继承spring-boot-starter-parent后已经具备版本锁定等配置。因此起步依赖的作用就是进行依赖的传递。



#### 3.1.2 分析spring-boot-starter-web

spring-boot-starter-web的pom.xml，xml配置如下（只摘抄部分重点配置）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  	<modelVersion>4.0.0</modelVersion>
  	<parent>
    	<groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starters</artifactId>
    	<version>2.0.1.RELEASE</version>
  	</parent>
  	<groupId>org.springframework.boot</groupId>
  	<artifactId>spring-boot-starter-web</artifactId>
  	<version>2.0.1.RELEASE</version>
  	<name>Spring Boot Web Starter</name>
  
  	<dependencies>
    	<dependency>
      		<groupId>org.springframework.boot</groupId>
      		<artifactId>spring-boot-starter</artifactId>
      		<version>2.0.1.RELEASE</version>
      		<scope>compile</scope>
    	</dependency>
    	<dependency>
      		<groupId>org.springframework.boot</groupId>
      		<artifactId>spring-boot-starter-json</artifactId>
      		<version>2.0.1.RELEASE</version>
      		<scope>compile</scope>
    	</dependency>
    	<dependency>
      		<groupId>org.springframework.boot</groupId>
      		<artifactId>spring-boot-starter-tomcat</artifactId>
      		<version>2.0.1.RELEASE</version>
      		<scope>compile</scope>
    	</dependency>
    	<dependency>
      		<groupId>org.hibernate.validator</groupId>
      		<artifactId>hibernate-validator</artifactId>
      		<version>6.0.9.Final</version>
      		<scope>compile</scope>
    	</dependency>
    	<dependency>
      		<groupId>org.springframework</groupId>
      		<artifactId>spring-web</artifactId>
      		<version>5.0.5.RELEASE</version>
      		<scope>compile</scope>
    	</dependency>
    	<dependency>
      		<groupId>org.springframework</groupId>
      		<artifactId>spring-webmvc</artifactId>
      		<version>5.0.5.RELEASE</version>
      		<scope>compile</scope>
    	</dependency>
  	</dependencies>
</project>
```

从上面的spring-boot-starter-web的pom.xml中可以发现，spring-boot-starter-web就是将web开发要使用的spring-web、spring-webmvc等坐标进行“打包”，如此自己创建的工程只要引入spring-boot-starter-web起步依赖的坐标就可以进行web开发，同样体现依赖传递的作用。



### 3.2 自动配置原理解析

查看启动类MySpringBootApplication上的注解@SpringBootApplication

```java
@SpringBootApplication
public class MySpringBootApplication {
    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class);
    }
}
```

注解@SpringBootApplication的源码

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
		@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

	/**
	 * Exclude specific auto-configuration classes such that they will never be applied.
	 * @return the classes to exclude
	 */
	@AliasFor(annotation = EnableAutoConfiguration.class)
	Class<?>[] exclude() default {};

	... ... ...

}
```

其中

- @SpringBootConfiguration：等同与@Configuration，既标注该类是Spring的一个配置类

- @EnableAutoConfiguration：SpringBoot自动配置功能开启


查看注解@EnableAutoConfiguration

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
	... ... ...
}
```

其中@Import(AutoConfigurationImportSelector.class) 导入AutoConfigurationImportSelector类

查看AutoConfigurationImportSelector源码

```java
public String[] selectImports(AnnotationMetadata annotationMetadata) {
        ... ... ...
        List<String> configurations = getCandidateConfigurations(annotationMetadata,
                                                                   attributes);
        configurations = removeDuplicates(configurations);
        Set<String> exclusions = getExclusions(annotationMetadata, attributes);
        checkExcludedClasses(configurations, exclusions);
        configurations.removeAll(exclusions);
        configurations = filter(configurations, autoConfigurationMetadata);
        fireAutoConfigurationImportEvents(configurations, exclusions);
        return StringUtils.toStringArray(configurations);
}


protected List<String> getCandidateConfigurations(AnnotationMetadata metadata,
			AnnotationAttributes attributes) {
		List<String> configurations = SpringFactoriesLoader.loadFactoryNames(
				getSpringFactoriesLoaderFactoryClass(), getBeanClassLoader());
		
		return configurations;
}

```

其中SpringFactoriesLoader.loadFactoryNames 方法的作用就是从META-INF/spring.factories文件中读取指定类对应的类名称列表 

spring.factories 文件中有关自动配置的配置信息如下：

```
... ... ...

org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\

... ... ...
```

上面配置文件存在大量的以Configuration为结尾的类名称，这些类就是存有自动配置信息的类，而SpringApplication在获取这些类名后再加载

以ServletWebServerFactoryAutoConfiguration为例来分析源码：

```java
@Configuration
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE)
@ConditionalOnClass(ServletRequest.class)
@ConditionalOnWebApplication(type = Type.SERVLET)
@EnableConfigurationProperties(ServerProperties.class)
@Import({ ServletWebServerFactoryAutoConfiguration.BeanPostProcessorsRegistrar.class,
		ServletWebServerFactoryConfiguration.EmbeddedTomcat.class,
		ServletWebServerFactoryConfiguration.EmbeddedJetty.class,
		ServletWebServerFactoryConfiguration.EmbeddedUndertow.class })
public class ServletWebServerFactoryAutoConfiguration {
	... ... ...
}
```

其中@EnableConfigurationProperties(ServerProperties.class) 代表加载ServerProperties服务器配置属性类

进入ServerProperties.class源码如下：

```java
@ConfigurationProperties(prefix = "server", ignoreUnknownFields = true)
public class ServerProperties {

	/**
	 * Server HTTP port.
	 */
	private Integer port;

	/**
	 * Network address to which the server should bind.
	 */
	private InetAddress address;
  
  	... ... ...
  
}
```

其中 prefix = "server" 表示SpringBoot配置文件中的前缀，SpringBoot会将配置文件中以server开始的属性映射到该类的字段中。



## 4. SpringBoot的配置文件

SpringBoot是基于约定的，所以很多配置都有默认值。如果可以需要修改配置，可以通过配置文件进行配置

1. application.properties
2. application.yml



### 4.1 配置文件的类型

#### 4.1.1 application.properties

​	语法:key=value

#### 4.1.2 application.yml

##### 4.1.2.1 yml配置文件简介

YAML 是专门用来写配置文件的语言，非常简洁和强大，远比 JSON 格式方便。

YML文件的扩展名可以使用 `.yml` 或者 `.yaml`



##### 4.1.2.2 yml配置文件的语法

- 大小写敏感
- 使用缩进表示层级关系
- 缩进时不允许使用Tab键，只允许使用空格。
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- "#"号表示注释



###### 4.1.2.2.1 配置普通数据

- 语法： key: value

- 示例代码：

```yaml
name: haohao
```

- 注意：value之前有一个空格



###### 4.1.2.2.2 配置对象数据或Map数据

- 语法： 

  ​	key: 

  ​		key1: value1

  ​		key2: value2

  ​	或者：

  ​	key: {key1: value1,key2: value2}

- 示例代码：

```yaml
person:
  name: haohao
  age: 31
  addr: beijing
#或者
person: {name: haohao,age: 31,addr: beijing}
```

- 注意：key1前面的空格个数不限定，在yml语法中，相同缩进代表同一个级别



###### 4.1.2.2.3 配置集合数据

- 语法： 

  ​	key: 

  ​		- value1

  ​		- value2

  或者：

  ​	key: [value1,value2]

- 示例代码：

 ```yaml
city:
  - beijing
  - tianjin
  - shanghai
  - chongqing
  
#或者

city: [beijing,tianjin,shanghai,chongqing]

#集合中的元素是对象形式
student:
  - name: zhangsan
    age: 18
    score: 100
  - name: lisi
    age: 28
    score: 88
  - name: wangwu
    age: 38
    score: 90
    
# 或者
student: [{name: tom,age: 18,addr: beijing},{name: lucy,age: 17,addr: tianjin}]
 ```

- 注意：value1与之间的 - 之间存在一个空格



#### 4.1.3 SpringBoot配置信息的查询

SpringBoot的配置文件主要的目的就是对配置信息进行修改。

查阅SpringBoot的官方文档：查阅配置时的key

文档URL：https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#common-application-properties

常用的配置摘抄如下：

```properties
# QUARTZ SCHEDULER (QuartzProperties)
spring.quartz.jdbc.initialize-schema=embedded # Database schema initialization mode.
spring.quartz.jdbc.schema=classpath:org/quartz/impl/jdbcjobstore/tables_@@platform@@.sql # Path to the SQL file to use to initialize the database schema.
spring.quartz.job-store-type=memory # Quartz job store type.
spring.quartz.properties.*= # Additional Quartz Scheduler properties.

# ----------------------------------------
# WEB PROPERTIES
# ----------------------------------------

# EMBEDDED SERVER CONFIGURATION (ServerProperties)
server.port=8080 # Server HTTP port.
server.servlet.context-path= # Context path of the application.
server.servlet.path=/ # Path of the main dispatcher servlet.

# HTTP encoding (HttpEncodingProperties)
spring.http.encoding.charset=UTF-8 # Charset of HTTP requests and responses. Added to the "Content-Type" header if not set explicitly.

# JACKSON (JacksonProperties)
spring.jackson.date-format= # Date format string or a fully-qualified date format class name. For instance, `yyyy-MM-dd HH:mm:ss`.

# SPRING MVC (WebMvcProperties)
spring.mvc.servlet.load-on-startup=-1 # Load on startup priority of the dispatcher servlet.
spring.mvc.static-path-pattern=/** # Path pattern used for static resources.
spring.mvc.view.prefix= # Spring MVC view prefix.
spring.mvc.view.suffix= # Spring MVC view suffix.

# DATASOURCE (DataSourceAutoConfiguration & DataSourceProperties)
spring.datasource.driver-class-name= # Fully qualified name of the JDBC driver. Auto-detected based on the URL by default.
spring.datasource.password= # Login password of the database.
spring.datasource.url= # JDBC URL of the database.
spring.datasource.username= # Login username of the database.

# JEST (Elasticsearch HTTP client) (JestProperties)
spring.elasticsearch.jest.password= # Login password.
spring.elasticsearch.jest.proxy.host= # Proxy host the HTTP client should use.
spring.elasticsearch.jest.proxy.port= # Proxy port the HTTP client should use.
spring.elasticsearch.jest.read-timeout=3s # Read timeout.
spring.elasticsearch.jest.username= # Login username.

```

通过配置application.poperties 或者 application.yml 来修改SpringBoot的默认配置

例如：

application.properties文件

```properties
server.port=8888
server.servlet.context-path=demo
```

application.yml文件

```yaml
server:
  port: 8888
  servlet:
    context-path: /demo
```



### 4.2 配置文件与配置类的属性映射方式

#### 4.2.1 使用注解@Value映射

我们可以通过@Value注解将配置文件中的值映射到一个Spring管理的Bean的字段上

例如：

application.properties配置如下：

```properties
person.name=zhangsan
person.age=18
```

或者，application.yml配置如下：

```yaml
person:
  name: zhangsan
  age: 18
```

实体Bean代码如下：

```java
@Controller
public class QuickStartController {

    @Value("${person.name}")
    private String name;
    @Value("${person.age}")
    private Integer age;


    @RequestMapping("/quick")
    @ResponseBody
    public String quick(){
        return "springboot 访问成功! name="+name+",age="+age;
    }

}
```

> 注意@Value注解不能注入集合类型数据



#### 4.2.2 使用注解@ConfigurationProperties映射

通过注解@ConfigurationProperties(prefix="配置文件中的key的前缀")可以将配置文件中的配置自动与实体进行映射

application.properties配置如下：

```properties
person.name=zhangsan
person.age=18
```

或者，application.yml配置如下：

```yaml
person:
  name: zhangsan
  age: 18
```

实体Bean代码如下：

```java
@Controller
@ConfigurationProperties(prefix = "person")
public class QuickStartController {

    private String name;
    private Integer age;

    @RequestMapping("/quick")
    @ResponseBody
    public String quick(){
        return "springboot 访问成功! name="+name+",age="+age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}
```

注意：使用@ConfigurationProperties方式可以进行配置文件与实体字段的自动映射，但需要字段必须提供set方法才可以，而使用@Value注解修饰的字段不需要提供set方法

#### 4.2.3 集合类型的映射

* 配置文件

```yml
my:
	servers:
	  - dev.example.com
	  - another.example.com
	
	map:
	  key1: value1
	  key2: value2
	  key3: value3
	  
	lMap:
	  - name: tom
	    age: 18
	    addr: beijing
	  - name: lucy
	    age: 17
	    addr: tianjin
```

* 配置类

```java
@Component
@ConfigurationProperties(prefix = "my")
public class Config {
    private List<String> servers = new ArrayList<String>();
    private Map<String,String> map = new HashMap<String,String>();
    private List<Map<String,String>> lMap;
    
    //seter...
}
```

* 使用配置类

```java
@RestController
public class HelloServlet {
    @Resource
    Config config;
}
```

## 5. SpringBoot与整合其他技术

### 5.1 SpringBoot整合Mybatis

#### 5.1.1 添加Mybatis的起步依赖

```xml
<!--mybatis起步依赖-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>
```

#### 5.1.2 添加数据库驱动坐标

```xml
<!-- MySQL连接驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

#### 5.1.3 添加数据库连接信息

在application.properties中添加数据量的连接信息

```properties
#DB Configuration:
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=root
```

#### 5.1.4 创建user表

在test数据库中创建user表

```sql
-- ----------------------------
-- Table structure for `user`
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(50) DEFAULT NULL,
  `password` varchar(50) DEFAULT NULL,
  `name` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES ('1', 'zhangsan', '123', '张三');
INSERT INTO `user` VALUES ('2', 'lisi', '123', '李四');
```

#### 5.1.5 创建实体Bean

```java
public class User {
    // 主键
    private Long id;
    // 用户名
    private String username;
    // 密码
    private String password;
    // 姓名
    private String name;
  
    //此处省略getter和setter方法 .. ..
    
}
```

#### 5.1.6 编写Mapper

```java
@Mapper
public interface UserMapper {
	public List<User> queryUserList();
}
```

注意：@Mapper标记该类是一个mybatis的mapper接口，可以被spring boot自动扫描到spring上下文中

#### 5.1.7 配置Mapper映射文件

在src\main\resources\mapper路径下加入UserMapper.xml配置文件"

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.goes.mapper.UserMapper">
    <select id="queryUserList" resultType="user">
        select * from user
    </select>
</mapper>
```

#### 5.1.8 在application.properties中添加mybatis的信息

```properties
#spring集成Mybatis环境
#pojo别名扫描包
mybatis.type-aliases-package=com.goes.domain
#加载Mybatis映射文件
mybatis.mapper-locations=classpath:mapper/*Mapper.xml
```

#### 5.1.9 编写测试Controller

```java
@Controller
public class MapperController {

    @Autowired
    private UserMapper userMapper;

    @RequestMapping("/queryUser")
    @ResponseBody
    public List<User> queryUser(){
        List<User> users = userMapper.queryUserList();
        return users;
    }
}
```



### 5.2 SpringBoot整合Junit

#### 5.2.1 添加Junit的起步依赖

```xml
<!--测试的起步依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

#### 5.2.2 编写测试类

```java
import com.goes.MySpringBootApplication;
import com.goes.domain.User;
import com.goes.mapper.UserMapper;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.List;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = MySpringBootApplication.class)
public class MapperTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void test() {
        List<User> users = userMapper.queryUserList();
        System.out.println(users);
    }

}
```

其中，SpringRunner继承自SpringJUnit4ClassRunner，使用哪一个Spring提供的测试测试引擎都可以

```java
public final class SpringRunner extends SpringJUnit4ClassRunner 
```

@SpringBootTest的属性指定的是引导类的字节码对象



### 5.3 SpringBoot整合Spring Data JPA

#### 5.3.1 添加Spring Data JPA的起步依赖

```xml
<!-- springBoot JPA的起步依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

#### 5.3.2 添加数据库驱动依赖

```xml
<!-- MySQL连接驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

#### 5.3.3 在application.properties中配置数据库和jpa的相关属性

```properties
#DB Configuration:
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=root

#JPA Configuration:
spring.jpa.database=MySQL
spring.jpa.show-sql=true
spring.jpa.generate-ddl=true
spring.jpa.hibernate.ddl-auto=update
spring.jpa.hibernate.naming_strategy=org.hibernate.implicit_naming_strategy
```

#### 5.3.4 创建实体配置实体

```java
@Entity
public class User {
    // 主键
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    // 用户名
    private String username;
    // 密码
    private String password;
    // 姓名
    private String name;
 
    //此处省略setter和getter方法... ...
}
```

#### 5.3.5 编写UserRepository

```java
public interface UserRepository extends JpaRepository<User,Long>{
    public List<User> findAll();
}
```

#### 5.3.6 编写测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes=MySpringBootApplication.class)
public class JpaTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    public void test(){
        List<User> users = userRepository.findAll();
        System.out.println(users);
    }

}
```

注意：如果是jdk9会执行报错

原因：jdk缺少相应的jar

解决方案：手动导入对应的maven坐标，如下：

```xml
<!--jdk9需要导入如下坐标-->
<dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>2.3.0</version>
</dependency>
```



### 5.4 SpringBoot整合Redis

#### 5.4.1 添加redis的起步依赖

```xml
<!-- 配置使用redis启动器 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

#### 5.4.2 配置redis的连接信息

```properties
#Redis
spring.redis.host=127.0.0.1
spring.redis.port=6379
```

#### 5.4.3 注入RedisTemplate测试redis操作

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = SpringbootJpaApplication.class)
public class RedisTest {

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private RedisTemplate<String, String> redisTemplate;

    @Test
    public void test() throws JsonProcessingException {
        //从redis缓存中获得指定的数据
        String userListData = redisTemplate.boundValueOps("user.findAll").get();
        //如果redis中没有数据的话
        if(null==userListData){
            //查询数据库获得数据
            List<User> all = userRepository.findAll();
            //转换成json格式字符串
            ObjectMapper om = new ObjectMapper();
            userListData = om.writeValueAsString(all);
            //将数据存储到redis中，下次在查询直接从redis中获得数据，不用在查询数据库
            redisTemplate.boundValueOps("user.findAll").set(userListData);
            System.out.println("===============从数据库获得数据===============");
        }else{
            System.out.println("===============从redis缓存中获得数据===============");
        }
        System.out.println(userListData);
    }

}
```



## SpringBoot基础篇

### 1、SpringBoot简介

SpringBoot简化操作主要在4个方面进行体现

- parent
- starter
- 引导类
- 内嵌tomcat

#### parent

<font color="#ff0000"><b>parent</b></font>自身具有很多个版本，每个<font color="#ff0000"><b>parent</b></font>版本中包含有几百个其他技术的版本号，不同的parent间使用的各种技术的版本号有可能会发生变化。当开发者使用某些技术时，直接使用SpringBoot提供的<font color="#ff0000"><b>parent</b></font>就行了，由<font color="#ff0000"><b>parent</b></font>帮助开发者统一的进行各种技术的版本管理。

​		比如你现在要使用Spring配合MyBatis开发，没有parent之前怎么做呢？选个Spring的版本，再选个MyBatis的版本，再把这些技术使用时关联的其他技术的版本逐一确定下来。当你Spring的版本发生变化需要切换时，你的MyBatis版本有可能也要跟着切换，关联技术呢？可能都要切换，而且切换后还可能出现其他问题。现在这一切工作都可以交给parent来做了。你无需关注这些技术间的版本冲突问题，你只需要关注你用什么技术就行了，冲突问题由<font color="#ff0000"><b>parent</b></font>负责处理。

​		有人可能会提出来，万一<font color="#ff0000"><b>parent</b></font>给我导入了一些我不想使用的依赖怎么办？记清楚，这一点很关键，<font color="#ff0000"><b>parent</b></font>仅仅帮我们进行版本管理，它不负责帮你导入坐标，说白了用什么还是你自己定，只不过版本不需要你管理了。整体上来说，<font color="#ff0000"><b>使用parent可以帮助开发者进行版本的统一管理。</b></font>

​		<font color="#ff0000"><b>关注</b></font>：parent定义出来以后，并不是直接使用的，仅仅给了开发者一个说明书，但是并没有实际使用，这个一定要确认清楚。



- 项目中的pom.xml中继承一个坐标

```XML
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.4</version>
</parent>
```

- 打开后可以查阅到其中又继承了一个坐标

```XML
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.5.4</version>
</parent>
```

- 此坐标中定义两组信息

第一组是各式各样的依赖版本号属性，下面列出依赖版本属性的局部，可以看的出来，定义了若干个技术的依赖版本号。

```XML
<properties>
    <activemq.version>5.16.3</activemq.version>
    <aspectj.version>1.9.7</aspectj.version>
    <assertj.version>3.19.0</assertj.version>
    <commons-codec.version>1.15</commons-codec.version>
    <commons-dbcp2.version>2.8.0</commons-dbcp2.version>
    <commons-lang3.version>3.12.0</commons-lang3.version>
    <commons-pool.version>1.6</commons-pool.version>
    <commons-pool2.version>2.9.0</commons-pool2.version>
    <h2.version>1.4.200</h2.version>
    <hibernate.version>5.4.32.Final</hibernate.version>
    <hibernate-validator.version>6.2.0.Final</hibernate-validator.version>
    <httpclient.version>4.5.13</httpclient.version>
    <jackson-bom.version>2.12.4</jackson-bom.version>
    <javax-jms.version>2.0.1</javax-jms.version>
    <javax-json.version>1.1.4</javax-json.version>
    <javax-websocket.version>1.1</javax-websocket.version>
    <jetty-el.version>9.0.48</jetty-el.version>
    <junit.version>4.13.2</junit.version>
</properties>
```

第二组是各式各样的依赖坐标信息，可以看出依赖坐标定义中没有具体的依赖版本号，而是引用了第一组信息中定义的依赖版本属性值.

```XML
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>${hibernate.version}</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

​		<font color="#ff0000"><b>关注</b></font>：上面的依赖坐标定义是出现在<dependencyManagement>标签中的，是对引用坐标的依赖管理，并不是实际使用的坐标。因此当你的项目中继承了这组parent信息后，在不使用对应坐标的情况下，前面的这组定义是不会具体导入某个依赖的。

​		<font color="#ff0000"><b>关注</b></font>：因为在maven中继承机会只有一次，上述继承的格式还可以切换成导入的形式进行，并且在阿里云的starter创建工程时就使用了此种形式。

```XML
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>${spring-boot.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

**总结**

1. 开发SpringBoot程序要继承spring-boot-starter-parent
2. spring-boot-starter-parent中定义了若干个依赖管理
3. 继承parent模块可以避免多个依赖使用相同技术时出现依赖版本冲突
4. 继承parent的形式也可以采用引入依赖的形式实现效果

**思考**

​		parent中定义了若干个依赖版本管理，但是也没有使用，那这个设定也就不生效啊，究竟谁在使用这些定义呢？



#### starter

​		SpringBoot关注到实际开发时，开发者对于依赖坐标的使用往往都有一些固定的组合方式，比如使用spring-webmvc就一定要使用spring-web。每次都要固定搭配着写，非常繁琐，而且格式固定，没有任何技术含量。

​		SpringBoot一看这种情况，看来需要给开发者带来一些帮助了。安排，把所有的技术使用的固定搭配格式都给开发出来，以后你用某个技术，就不用每次写一堆依赖了，还容易写错，我给你做一个东西，代表一堆东西，开发者使用的时候，直接用我做好的这个东西就好了，对于这样的固定技术搭配，SpringBoot给它起了个名字叫做<font color="#ff0000"><b>starter</b></font>。

​		starter定义了使用某种技术时对于依赖的固定搭配格式，也是一种最佳解决方案，<font color="#ff0000"><b>使用starter可以帮助开发者减少依赖配置</b></font>。

​		这个东西其实在入门案例里面已经使用过了，入门案例中的web功能就是使用这种方式添加依赖的。可以查阅SpringBoot的配置源码，看到这些定义。

- 项目中的pom.xml定义了使用SpringMVC技术，但是并没有写SpringMVC的坐标，而是添加了一个名字中包含starter的依赖

```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

- 在spring-boot-starter-web中又定义了若干个具体依赖的坐标

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-json</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.3.9</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.9</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

​		之前提到过开发SpringMVC程序需要导入spring-webmvc的坐标和spring整合web开发的坐标，就是上面这组坐标中的最后两个了。

​		但是我们发现除了这两个坐标，还有其他的坐标。比如第二个，叫做spring-boot-starter-json。看名称就知道，这个是与json有关的坐标了，但是看名字发现和最后两个又不太一样，它的名字中也有starter，打开看看里面有什么？

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.3.9</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.12.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.datatype</groupId>
        <artifactId>jackson-datatype-jdk8</artifactId>
        <version>2.12.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.datatype</groupId>
        <artifactId>jackson-datatype-jsr310</artifactId>
        <version>2.12.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.module</groupId>
        <artifactId>jackson-module-parameter-names</artifactId>
        <version>2.12.4</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

​		我们可以发现，这个starter中又包含了若干个坐标，其实就是使用SpringMVC开发通常都会使用到Json，使用json又离不开这里面定义的这些坐标，看来还真是方便，SpringBoot把我们开发中使用的东西能用到的都给提前做好了。你仔细看完会发现，里面有一些你没用过的。的确会出现这种过量导入的可能性，没关系，可以通过maven中的排除依赖剔除掉一部分。不过你不管它也没事，大不了就是过量导入呗。

​	到这里基本上得到了一个信息，使用starter可以帮开发者快速配置依赖关系。以前写依赖3个坐标的，现在写导入一个就搞定了，就是加速依赖配置的。

**starter与parent的区别**

​		朦朦胧胧中感觉starter与parent好像都是帮助我们简化配置的，但是功能又不一样，梳理一下。

​		<font color="#ff0000"><b>starter</b></font>是一个坐标中定了若干个坐标，以前写多个的，现在写一个，<font color="#ff0000"><b>是用来减少依赖配置的书写量的</b></font>。

​		<font color="#ff0000"><b>parent</b></font>是定义了几百个依赖版本号，以前写依赖需要自己手工控制版本，现在由SpringBoot统一管理，这样就不存在版本冲突了，<font color="#ff0000"><b>是用来减少依赖冲突的</b></font>。

**实际开发应用方式**

- 实际开发中如果需要用什么技术，先去找有没有这个技术对应的starter

  - 如果有对应的starter，直接写starter，而且无需指定版本，版本由parent提供
  - 如果没有对应的starter，手写坐标即可

- 实际开发中如果发现坐标出现了冲突现象，确认你要使用的可行的版本号，使用手工书写的方式添加对应依赖，覆盖SpringBoot提供给我们的配置管理

  - 方式一：直接写坐标
  - 方式二：覆盖<properties>中定义的版本号，就是下面这堆东西了，哪个冲突了覆盖哪个就OK了

  ```XML
  <properties>
      <activemq.version>5.16.3</activemq.version>
      <aspectj.version>1.9.7</aspectj.version>
      <assertj.version>3.19.0</assertj.version>
      <commons-codec.version>1.15</commons-codec.version>
      <commons-dbcp2.version>2.8.0</commons-dbcp2.version>
      <commons-lang3.version>3.12.0</commons-lang3.version>
      <commons-pool.version>1.6</commons-pool.version>
      <commons-pool2.version>2.9.0</commons-pool2.version>
      <h2.version>1.4.200</h2.version>
      <hibernate.version>5.4.32.Final</hibernate.version>
      <hibernate-validator.version>6.2.0.Final</hibernate-validator.version>
      <httpclient.version>4.5.13</httpclient.version>
      <jackson-bom.version>2.12.4</jackson-bom.version>
      <javax-jms.version>2.0.1</javax-jms.version>
      <javax-json.version>1.1.4</javax-json.version>
      <javax-websocket.version>1.1</javax-websocket.version>
      <jetty-el.version>9.0.48</jetty-el.version>
      <junit.version>4.13.2</junit.version>
  </properties>
  ```

<font color="#f0f"><b>温馨提示</b></font>

​		SpringBoot官方给出了好多个starter的定义，方便我们使用，而且名称都是如下格式

```JAVA
命名规则：spring-boot-starter-技术名称
```

​	所以后期见了spring-boot-starter-aaa这样的名字，这就是SpringBoot官方给出的starter定义。那非官方定义的也有吗？有的，具体命名方式到整合技术的章节再说。

**总结**

1. 开发SpringBoot程序需要导入坐标时通常导入对应的starter
2. 每个不同的starter根据功能不同，通常包含多个依赖坐标
3. 使用starter可以实现快速配置的效果，达到简化配置的目的



#### 引导类

​		配置说完了，我们发现SpringBoot确实帮助我们减少了很多配置工作，下面说一下程序是如何运行的。目前程序运行的入口就是SpringBoot工程创建时自带的那个类，也就是带有main方法的那个类，运行这个类就可以启动SpringBoot工程的运行。

```java
@SpringBootApplication
public class Springboot0101QuickstartApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot0101QuickstartApplication.class, args);
    }
}
```

​		SpringBoot本身是为了加速Spring程序的开发的，而Spring程序运行的基础是需要创建Spring容器对象（IoC容器）并将所有的对象放置到Spring容器中管理，也就是一个一个的Bean。现在改用SpringBoot加速开发Spring程序，这个容器还在吗？这个疑问不用说，一定在。其实当前这个类运行后就会产生一个Spring容器对象，并且可以将这个对象保存起来，通过容器对象直接操作Bean。

```JAVA
@SpringBootApplication
public class QuickstartApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext ctx = SpringApplication.run(QuickstartApplication.class, args);
        BookController bean = ctx.getBean(BookController.class);
        System.out.println("bean======>" + bean);
    }
}
```

​		通过上述操作不难看出，其实SpringBoot程序启动还是创建了一个Spring容器对象。当前运行的这个类在SpringBoot程序中是所有功能的入口，称为<font color="#ff0000"><b>引导类</b></font>。

​		作为一个引导类最典型的特征就是当前类上方声明了一个注解<font color="#ff0000"><b>@SpringBootApplication</b></font>。

**总结**

1. SpringBoot工程提供引导类用来启动程序
2. SpringBoot工程启动后创建并初始化Spring容器

**思考**

​		程序现在已经运行了，通过引导类的main方法运行了起来。但是运行java程序不应该是执行完就结束了吗？但是我们现在明显是启动了一个web服务器啊，不然网页怎么能正常访问呢？这个服务器是在哪里写的呢？



#### 内嵌tomcat

​		当前我们做的SpringBoot入门案例勾选了Spring-web的功能，并且导入了对应的starter。

```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

​		SpringBoot发现，既然你要做web程序，肯定离不开使用web服务器，这样吧，帮人帮到底，送佛送到西，我帮你搞一个web服务器，你要愿意用的，直接使用就好了。SpringBoot又琢磨，提供一种服务器万一不满足开发者需要呢？干脆我再多给你几种选择，你随便切换。万一你不想用我给你提供的，也行，你可以自己搞。

​		由于这个功能不属于程序的主体功能，可用可不用，于是乎SpringBoot将其定位成辅助功能，别小看这么一个辅助功能，它可是帮我们开发者又减少了好多的设置性工作。

​		下面就围绕着这个内置的web服务器，也可以说是内置的tomcat服务器来研究几个问题：

1. 这个服务器在什么位置定义的
2. 这个服务器是怎么运行的
3. 这个服务器如果想换怎么换？虽然这个需求很垃圾，搞得开发者会好多web服务器一样，用别人提供好的不香么？非要自己折腾

**内嵌Tomcat定义位置**

​		说到定义的位置，我们就想，如果我们不开发web程序，用的着web服务器吗？肯定用不着啊。那如果这个东西被加入到你的程序中，伴随着什么技术进来的呢？肯定是web相关的功能啊，没错，就是前面导入的web相关的starter做的这件事。

```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

​		打开web对应的starter查看导入了哪些东西。

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-json</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.3.9</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.9</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

​		第三个依赖就是tomcat对应的东西了，居然也是一个starter，再打开看看。

```XML
<dependencies>
    <dependency>
        <groupId>jakarta.annotation</groupId>
        <artifactId>jakarta.annotation-api</artifactId>
        <version>1.3.5</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-core</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
        <exclusions>
            <exclusion>
                <artifactId>tomcat-annotations-api</artifactId>
                <groupId>org.apache.tomcat</groupId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-el</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-websocket</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
        <exclusions>
            <exclusion>
                <artifactId>tomcat-annotations-api</artifactId>
                <groupId>org.apache.tomcat</groupId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

​		这里面有一个核心的坐标，tomcat-embed-core，叫做tomcat内嵌核心。就是这个东西把tomcat功能引入到了我们的程序中的。目前解决了第一个问题，找到根儿了，谁把tomcat引入到程序中的？spring-boot-starter-web中的spring-boot-starter-tomcat做的。之所以你感觉很奇妙的原因就是，这个东西是默认加入到程序中了，所以感觉很神奇，居然什么都不做，就有了web服务器对应的功能。再来说第二个问题，这个服务器是怎么运行的。

**内嵌Tomcat运行原理**

​		Tomcat服务器是一款软件，而且是一款使用java语言开发的软件，熟悉tomcat的话应该知道tomcat安装目录中保存有很多jar文件。

​		下面的问题来了，既然是使用java语言开发的，运行的时候肯定符合java程序运行的原理，java程序运行靠的是什么？对象呀，一切皆对象，万物皆对象。那tomcat运行起来呢？也是对象啊。

​		如果是对象，那Spring容器是用来管理对象的，这个对象能交给Spring容器管理吗？把吗去掉，是个对象都可以交给Spring容器管理，行了，这下通了，tomcat服务器运行其实是以对象的形式在Spring容器中运行的。怪不得我们没有安装这个tomcat但是还能用，闹了白天这东西最后是以一个对象的形式存在，保存在Spring容器中悄悄运行的。具体运行的是什么呢？其实就是上前面提到的那个tomcat内嵌核心。

```XML
<dependencies>
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-core</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

​		那既然是个对象，如果把这个对象从Spring容器中去掉是不是就没有web服务器的功能呢？是这样的，通过依赖排除可以去掉这个web服务器功能。

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

​		上面对web-starter做了一个操作，使用maven的排除依赖去掉了使用tomcat的starter。这下好了，容器中肯定没有这个对象了，重新启动程序可以观察到程序运行了，但是并没有像之前那样运行后是一个一直运行的服务，而是直接停掉了，就是这个原因。

**更换内嵌Tomcat**

​		那根据上面的操作我们思考是否可以换个服务器呢？必须的嘛。根据SpringBoot的工作机制，用什么技术，加入什么依赖就行了。SpringBoot提供了3款内置的服务器：

- tomcat(默认)：apache出品，粉丝多，应用面广，负载了若干较重的组件

- jetty：更轻量级，负载性能远不及tomcat

- undertow：负载性能勉强跑赢tomcat

  想用哪个，加个坐标就OK。前提是把tomcat排除掉，因为tomcat是默认加载的。

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

​		现在就已经成功替换了web服务器，核心思想就是用什么加入对应坐标就可以了。如果有starter，优先使用starter。

**总结**

1. 内嵌Tomcat服务器是SpringBoot辅助功能之一
2. 内嵌Tomcat工作原理是将Tomcat服务器作为对象运行，并将该对象交给Spring容器管理
3. 变更内嵌服务器思想是去除现有服务器，添加全新的服务器



​		到这里第一章快速上手SpringBoot就结束了，这一章我们学习了两大块知识

1. 使用了4种方式制作了SpringBoot的入门程序，不管是哪一种，其实内部都是一模一样的

2. 学习了入门程序的工作流程，知道什么是parent，什么是starter，这两个东西是怎么配合工作的，以及我们的程序为什么启动起来是一个tomcat服务器等等

   第一章到这里就结束了，再往下学习就要去基于会创建SpringBoot工程的基础上，研究SpringBoot工程的具体细节了。



### 2、SpringBoot基础配置

#### 2-1.属性配置

SpringBoot默认配置文件是application.properties

**关闭运行日志图表（banner)**

```properties
spring.main.banner-mode=off
```

**设置运行日志的显示级别**

```properties
logging.level.root=debug
```



SpringBoot官方文档，打开查看附录中的Application Properties就可以获取到对应的配置项

> 配置文档地址：https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties

<font color="#f0f"><b>温馨提示</b></font>

SpringBoot所有的starter中都会依赖 `spring-boot-starter`，是所有SpringBoot的starter的基础依赖，汇集SpringBoot相关的基础配置。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>2.5.4</version>
    <scope>compile</scope>
</dependency>
```

**总结**

1. SpringBoot中导入对应starter后，提供对应配置属性
2. 书写SpringBoot配置采用关键字+提示形式书写



#### 2-2.配置文件分类

SpringBoot提供3种配置文件的格式

- properties（传统格式/默认格式）
- **yml**（主流格式）
- yaml



##### 配置文件优先级

1. 配置文件间的加载优先级	properties（最高）>  yml  >  yaml（最低）
2. 不同配置文件中相同配置按照加载优先级相互覆盖，不同配置文件中不同配置全部保留 



**配置文件自动提示功能消失解决方案**

指定SpringBoot配置文件

- Setting → Project Structure → Facets
- 选中对应项目/工程
- Customize Spring Boot
- 选择配置文件



#### 2-3.yaml文件

SpringBoot的配置以后主要使用yml结尾的这种文件格式，并且在书写时可以通过提示的形式加载正确的格式。

YAML，一种数据序列化格式，具有容易阅读、容易与脚本语言交互、以数据为核心，重数据轻格式的特点。

常见的文件扩展名有两种：

- .yml格式（主流）

- .yaml格式

具体的语法格式要求如下：

1. 大小写敏感
2. 属性层级关系使用多行描述，**每行结尾使用冒号结束**
3. 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）
4. 属性值前面添加空格（属性名与属性值之间使用冒号+空格作为分隔）
5. #号 表示注释

核心的一条规则要记住，<font color="#ff0000"><b>数据前面要加空格与冒号隔开</b></font>。

常见的数据书写格式

```YAML
boolean: TRUE  						#TRUE,true,True,FALSE,false，False均可
float: 3.14    						#6.8523015e+5  #支持科学计数法
int: 123       						#0b1010_0111_0100_1010_1110    #支持二进制、八进制、十六进制
null: ~        						#使用~表示null
string: HelloWorld      			#字符串可以直接书写
string2: "Hello World"  			#可以使用双引号包裹特殊字符
date: 2018-02-17        			#日期必须使用yyyy-MM-dd格式
datetime: 2018-02-17T15:02:31+08:00  #时间和日期之间使用T连接，最后使用+代表时区
```

yaml格式中也可以表示数组，在属性名书写位置的下方使用减号作为数据开始符号，每行书写一个数据，减号与数据间空格分隔。

```YAML
subject:
	- Java
	- 前端
	- 大数据
enterprise:
	name: itcast
    age: 16
    subject:
    	- Java
        - 前端
        - 大数据
likes: [王者荣耀,刺激战场]			#数组书写缩略格式
users:							 #对象数组格式一
  - name: Tom
   	age: 4
  - name: Jerry
    age: 5
users:							 #对象数组格式二
  -  
    name: Tom
    age: 4
  -   
    name: Jerry
    age: 5			    
users2: [ { name:Tom , age:4 } , { name:Jerry , age:5 } ]	#对象数组缩略格式
```

**总结**

1. yaml语法规则
   - 大小写敏感
   - 属性层级关系使用多行描述，每行结尾使用冒号结束
   - 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）
   - 属性值前面添加空格（属性名与属性值之间使用冒号+空格作为分隔）
   - #号 表示注释
2. 注意属性名冒号后面与数据之间有一个**空格**
3. 字面值、对象数据格式、数组数据格式



#### 2-4.yaml数据读取

##### 读取单一数据

yaml中保存的单个数据，可以使用Spring中的注解@Value读取单个数据，属性名引用方式：<font color="#ff0000"><b>${一级属性名.二级属性名……}</b></font>

![](images/image-20211126180433356-171453523205026.png)

使用@Value注解时，要将该注解写在某一个指定的Spring管控的bean的属性名上方。

1. 使用@Value配合SpEL读取单个数据
2. 如果数据存在多层级，依次书写层级名称即可



##### 读取全部数据

SpringBoot提供一个对象，叫Environment，能把所有的数据都封装到此对象中，使用自动装配注解可将所有的yaml数据封装到该对象。

![](images/image-20211126180738569.png)

数据封装到Environment对象中，获取属性时，通过接口操作进行，具体方法是getProperties（String），参数填写属性名即可

**总结**

1. 使用Environment对象封装全部配置信息
2. 使用@Autowired自动装配数据到Environment对象中



##### 读取对象数据

SpringBoot也提供了可以将一组yaml对象数据封装一个Java对象的操作

首先定义对象，并将其纳入Spring管控的范围，即定义成bean。使用注解@ConfigurationProperties指定加载哪一组yaml中配置的信息

![](images/image-20211126181126382.png)

@ConfigurationProperties必须配置加载数据的前缀，并且数据属性名要与对象的变量名完全匹配

![](images/image-20211126181423432.png)

**总结**

1. 使用@ConfigurationProperties注解绑定配置信息到封装类中
2. 封装类需要定义为Spring管理的bean，否则无法进行属性注入



##### yaml文件中的数据引用

在书写yaml数据时，经常出现如下现象：

```YAML
center:
	dataDir: /usr/local/fire/data
	tmpDir: /usr/local/fire/tmp
	logDir: /usr/local/fire/log
	msgDir: /usr/local/fire/msgDir
```

或者

```YAML
center:
	dataDir: D:/usr/local/fire/data
	tmpDir: D:/usr/local/fire/tmp
	logDir: D:/usr/local/fire/log
	msgDir: D:/usr/local/fire/msgDir
```

此时可使用引用格式来定义数据，格式如下：

```YAML
baseDir: /usr/local/fire
center:
    dataDir: ${baseDir}/data
    tmpDir: ${baseDir}/tmp
    logDir: ${baseDir}/log
    msgDir: ${baseDir}/msgDir
```



注意事项：在书写字符串时，如果需要使用转义字符，需要将数据字符串使用双引号包裹起来

```YAML
lesson: "Spring\tboot\nlesson"
```

**总结**

1. 在配置文件中可以使用${属性名}方式引用属性值
2. 如果属性中出现特殊字符，可以使用双引号包裹起来作为字符解析



### 3、基于SpringBoot实现SSMP整合

#### 3-1.整合JUnit

引入依赖

```xml
<!--测试的起步依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

Spring整合JUnit的制作方式

```JAVA
//加载spring整合junit专用的类运行器
@RunWith(SpringJUnit4ClassRunner.class)
//指定对应的配置信息
@ContextConfiguration(classes = SpringConfig.class)
public class AccountServiceTestCase {
    //注入你要测试的对象
    @Autowired
    private AccountService accountService;
    @Test
    public void testGetById(){
        //执行要测试的对象对应的方法
        System.out.println(accountService.findById(2));
    }
}
```

SpringBoot整合JUnit，具体格式如下：

```JAVA
@SpringBootTest
/**
 * 	//加载spring整合junit专用的类运行器
 *  @RunWith(SpringJUnit4ClassRunner.class)
 *  //指定对应的配置信息
 *  @ContextConfiguration(classes = SpringConfig.class)
 */
class SpringbootJunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("TEST");
    }
}
```

如果想手工指定引导类有两种方式

- 使用属性的形式：在注解@SpringBootTest中添加classes属性指定配置类

  ```java
  @SpringBootTest(classes = SpringbootJunitApplication.class)
  class SpringbootJunitApplicationTests {
      //注入你要测试的对象
      @Autowired
      private BookDao bookDao;
      @Test
      void contextLoads() {
          //执行要测试的对象对应的方法
          bookDao.save();
          System.out.println("TEST");
      }
  }
  ```

- 回归原始配置方式：使用@ContextConfiguration注解进行

  ```java
  @SpringBootTest
  @ContextConfiguration(classes = SpringbootJunitApplication.class)
  class SpringbootJunitApplicationTests {
      //注入你要测试的对象
      @Autowired
      private BookDao bookDao;
      @Test
      void contextLoads() {
          //执行要测试的对象对应的方法
          bookDao.save();
          System.out.println("TEST");
      }
  }
  ```



**总结**

1. 导入测试对应的starter
2. 测试类使用@SpringBootTest修饰
3. 使用自动装配的形式添加要测试的对象
4. 测试类如果存在于引导类所在包或子包中无需指定引导类
5. 测试类如果不存在于引导类所在的包或子包中需要通过classes属性指定引导类



#### 3-2.整合MyBatis

##### 回顾Spring整合MyBatis

导入Mybatis的坐标、数据库操作的JDBC坐标、以及MySQL驱动坐标

```XML
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.16</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.6</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
    <!--1.导入mybatis与spring整合的jar包-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.3.0</version>
    </dependency>
    <!--导入spring操作数据库必选的包-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>
</dependencies>
```

Spring核心配置

```JAVA
@Configuration
@ComponentScan("com.goes")
@PropertySource("jdbc.properties")
public class SpringConfig {
}
```

MyBatis要交给Spring接管的bean

```JAVA
// 定义mybatis专用的配置类
@Configuration
public class MyBatisConfig {
	// 定义创建SqlSessionFactory对应的bean
    @Bean
    public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
        //SqlSessionFactoryBean是由mybatis-spring包提供的，专用于整合用的对象
        SqlSessionFactoryBean sfb = new SqlSessionFactoryBean();
        //设置数据源替代原始配置中的environments的配置
        sfb.setDataSource(dataSource);
        //设置类型别名替代原始配置中的typeAliases的配置
        sfb.setTypeAliasesPackage("com.goes.domain");
        return sfb;
    }
	
    // 定义加载所有的映射配置
    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("com.goes.dao");
        return msc;
    }

}
```

数据源对应的bean，此处使用Druid数据源

```JAVA
@Configuration
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String userName;
    @Value("${jdbc.password}")
    private String password;

    @Bean("dataSource")
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}
```

数据库连接信息（properties格式）

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/spring_db?useSSL=false
jdbc.username=root
jdbc.password=root
```



##### SpringBoot整合MyBaits

导入对应技术的starter及对应数据库的坐标

```XML
<dependencies>
    <!--1.导入对应的starter-->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.2.0</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

配置数据源相关信息

```yaml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
```



下面写一个MyBatis程序运行需要的Dao（或者Mapper）就可以运行

**实体类**

```JAVA
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```

**映射接口（Dao）**

```JAVA
@Mapper
public interface BookDao {
    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);
}
```

**测试类**

```JAVA
@SpringBootTest
class SpringbootMybatisApplicationTests {
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        System.out.println(bookDao.getById(1));
    }
}
```



⚠️**注意：**当前使用的SpringBoot版本是2.5.4，对应的坐标设置中Mysql驱动使用的是8x版本。使用SpringBoot2.4.3（不含）之前版本会出现BUG（MySQL驱动升级到8以后要求强制配置时区），如果不设置会出问题。

解决方案：驱动 URL 上面添加对应设置

```YAML
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
```

设置的UTC是全球标准时间，也可修改配置为Asia/Shanghai。

```YAML
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=Asia/Shanghai
    username: root
    password: root
```



也还可修改mysql中的配置文件mysql.ini，在mysqld项中添加 `default-time-zone=+8:00` 即可解决。

此外在运行程序时还会有提示说数据库驱动过时的警告：弃用**com.mysql.jdbc.Driver**，换用<font color="#ff0000"><b>com.mysql.cj.jdbc.Driver</b></font>。



**总结**

1. 整合操作需要勾选MyBatis技术，也就是导入MyBatis对应的starter

2. 数据库连接相关信息转换成配置

3. 数据库SQL映射需要添加@Mapper被容器识别到

4. MySQL 8.X驱动强制要求设置时区

   - 修改url，添加serverTimezone设定
   - 修改MySQL数据库配置

5. 驱动类过时，提醒更换为com.mysql.cj.jdbc.Driver

   

#### 3-3.整合MyBatis-Plus

整合MyBaitsPlus（简称MP，国人开发的技术）

导入对应的starter

```XML
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
```

> <font color="#f0f"><b>温馨提示</b></font>
>
> 自定义starter命名规则  ：
>
> | starter所属 |                          命名规则                           |                           示例                            |
> | :---------: | :---------------------------------------------------------: | :-------------------------------------------------------: |
> |  官方提供   |                spring-boot-starter-技术名称                 |   spring-boot-starter-web <br/>spring-boot-starter-test   |
> | 第三方提供  |             第三方技术名称-spring-boot-starter              | mybatis-spring-boot-starter<br/>druid-spring-boot-starter |
> | 第三方提供  | 第三方技术名称-boot-starter（第三方技术名称过长，简化命名） |                 mybatis-plus-boot-starter                 |
>
> MyBaitsPlus坐标问题：
>
> 截止目前，SpringBoot官网还未收录此坐标，而Idea创建模块时读取的是SpringBoot官网的Spring Initializr，因此也没有。如果换用阿里云的url创建项目可以找到对应的坐标。

配置数据源相关信息

```yaml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
```



**映射接口（Dao）**

```JAVA
@Mapper
public interface BookDao extends BaseMapper<Book> {
}
```

核心在于Dao接口继承一个BaseMapper的接口，该接口中预定若干个常用的API接口，简化通用API接口的开发工作。

<img src="images/image-20211129100313919.png" alt="image-20211129100313919" style="zoom:80%;" />



设置所有表名的通用前缀名：配置application.yml文件，添加如下配置即可，

```yaml
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_		#设置所有表的通用前缀名称为tbl_
```

**总结**

1. 手工添加MyBatis-Plus对应的starter
2. 数据层接口使用BaseMapper简化开发
3. 需要使用的第三方技术无法通过勾选确定时，需要手工添加坐标



#### 3-4.整合Druid

在没有指定数据源时，配置如下：

```YAML
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=Asia/Shanghai
    username: root
    password: root
```

此时虽没有指定数据源，但是SpringBoot的默认数据源为：HiKari



Spring整合Druid

导入对应的坐标

```XML
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.16</version>
    </dependency>
</dependencies>
```

修改配置：在数据源配置中有一个type属性，专用于指定数据源类型

```YAML
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource
```

SpringBoot整合Druid

导入对应的starter

```XML
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.6</version>
    </dependency>
</dependencies>
```

修改配置

```YAML
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
```

**总结**

1. 整合Druid需要导入Druid对应的starter
2. 根据Druid提供的配置方式进行配置
3. 整合第三方技术通用方式
   - 导入对应的starter
   - 根据提供的配置格式，配置非默认值对应的配置项



## SpringBoot运维实用篇

- SpringBoot程序的打包与运行
- 配置高级
- 多环境开发
- 日志

### 1、打包与运行

##### 程序打包

SpringBoot程序是基于Maven创建的，在Maven中提供有打包的指令：package

```JAVA
mvn package
```

打包后会产生一个与工程名类似的jar文件，其名称是由模块名+版本号+.jar组成



##### 程序运行

在程序包所在路径下执行指令

```shell
java -jar 工程包名.jar
```

注意事项：

- 如果你的计算机中没有安装java的jdk环境，是无法正确执行上述操作的，因为程序执行使用的是java指令。

- 在使用向导创建SpringBoot工程时，pom.xml文件中会有如下配置，这一段配置千万不能删除，否则打包后无法正常执行程序。

  ```xml
  <build>
      <plugins>
          <plugin>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-maven-plugin</artifactId>
          </plugin>
      </plugins>
  </build>
  ```

**总结**

1. SpringBoot工程可以基于java环境下独立运行jar文件启动服务
2. SpringBoot工程执行mvn命令package进行打包
3. 执行jar命令：java –jar 工程名.jar



##### SpringBoot程序打包失败处理

在SpringBoot工程的pom.xml中有下面这组配置，这组配置决定打包出来的程序包是否可以执行。

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

开启和注释该段配置分别执行打包，共有3处比较明显的特征

- 打包后文件的大小不同
- 打包后所包含的内容不同
- 打包程序中个别文件内容不同

打包出来的两个Jar包仅有一个目录是一样的，叫做META-INF。打开容量大的程序包中的BOOT-INF目录下的classes目录，其中的内容和容量小的程序包中的内容完全一样。打开容量大的程序包中的BOOT-INF目录下，打开lib目录，里面显示了很多个jar文件。

META-INF下都有一个叫做MANIFEST.MF的文件，但是大小不同，打开文件，比较内容区别

- 小容量文件的MANIFEST.MF

  ```JAVA
  Manifest-Version: 1.0
  Implementation-Title: springboot_08_ssmp
  Implementation-Version: 0.0.1-SNAPSHOT
  Build-Jdk-Spec: 1.8
  Created-By: Maven Jar Plugin 3.2.0
  ```

- 大容量文件的MANIFEST.MF

  ```JAVA
  Manifest-Version: 1.0
  Spring-Boot-Classpath-Index: BOOT-INF/classpath.idx
  Implementation-Title: springboot_ssmp
  Implementation-Version: 0.0.1-SNAPSHOT
  Spring-Boot-Layers-Index: BOOT-INF/layers.idx
  Start-Class: com.goes.SSMPApplication
  Spring-Boot-Classes: BOOT-INF/classes/
  Spring-Boot-Lib: BOOT-INF/lib/
  Build-Jdk-Spec: 1.8
  Spring-Boot-Version: 2.5.4
  Created-By: Maven Jar Plugin 3.2.0
  Main-Class: org.springframework.boot.loader.JarLauncher
  ```

> **Main-Class: org.springframework.boot.loader.JarLauncher**
>
> 如果使用java -jar执行此程序包，将执行Main-Class属性配置的类。而org.springframework.boot.loader.<font color="#ff0000"><b>JarLauncher</b></font>类内部要查找Start-Class属性中配置的类，并执行对应的类，即引导类。

总结

1. SpringBoot程序添加配置后会打出一个特殊的包，包含Spring框架部分功能，原始工程内容，原始工程依赖的jar包
2. 首先读取MANIFEST.MF文件中的Main-Class属性，用来标记执行java -jar命令后运行的类
3. JarLauncher类执行时会找到Start-Class属性，也就是启动类类名
4. 运行启动类时会运行当前工程的内容
5. 运行当前工程时会使用依赖的jar包，从lib目录中查找

普通的jar包在MANIFEST.MF文件中没有Main-Class对应的属性，因此运行时提示找不到主清单属性。

**总结**

1. spring-boot-maven-plugin插件用于将当前程序打包成一个可以独立运行的程序包



##### 命令行启动常见问题及解决方案

遇到端口占用的问题：

```JAVA
# 查询端口
netstat -ano
# 查询指定端口
netstat -ano |findstr "端口号"
# 根据进程PID查询进程名称
tasklist |findstr "进程PID号"
# 根据PID杀死任务
taskkill /F /PID "进程PID号"
# 根据进程名称杀死任务
taskkill -f -t -im "进程名称"
```



##### SpringBoot项目快速启动（Linux版）

对于Linux系统下的程序运行与Windows系统下的程序运行差别不大，命令基本相同。



### 2、配置高级

#### 2-1.临时属性设置

临时修改端口号：

```JAVA
java –jar springboot.jar –-server.port=80
```

临时修改日志级别：

```JAVA
java –jar springboot.jar –-server.port=80 --logging.level.root=debug
```



属性加载优先级

临时属性的加载优先级要高于配置文件

官方文档查看配置读取的优先顺序：

- 官方地址：[https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html)

<img src="images/image-20211206100859236.png" alt="image-20211206100859236" style="zoom:67%;" />

**总结**

1. 使用jar命令启动SpringBoot工程时可以使用临时属性替换配置文件中的属性
2. 临时属性添加方式：java –jar 工程名.jar –-属性名=值
3. 多个临时属性之间使用空格分隔
4. 临时属性必须是当前boot工程支持的属性，否则设置无效



#### 2-2.配置文件分类

SpringBoot提供的4级配置文件：

- 类路径下配置文件（即resources目录中的application.yml文件）
- 类路径下config目录下配置文件
- 程序包所在目录中配置文件
- 程序包所在目录中config目录下配置文件

加载优先顺序：

1. file ：config/application.yml **【最高】**
2. file ：application.yml
3. classpath：config/application.yml
4. classpath：application.yml  **【最低】**

**总结**

1. 配置文件分为4种

   - 项目类路径配置文件：服务于开发人员本机开发与测试
   - 项目类路径config目录中配置文件：服务于项目经理整体调控
   - 工程路径配置文件：服务于运维人员配置涉密线上环境
   - 工程路径config目录中配置文件：服务于运维经理整体调控

2. 多层级配置文件间的属性采用叠加并覆盖的形式作用于程序



#### 2-3.自定义配置文件

自定义配置文件方式有如下两种：

**方式一：使用临时属性设置配置文件名（注意仅仅是名称，不要带扩展名）**

```shell
Environment
	Program arguments: --spring.config.name=ebnak
```

**方式二：使用临时属性设置配置文件路径（全路径名）**

`spring.config.name`

```shell
Environment
	Program arguments: --spring.config.name=classpath:/ebnak.properties
```

也可以设置加载多个配置文件

`spring.config.location`

```
Environment
	Program arguments: --spring.config.location=classpath:/ebnak.properties,classpath:/ebnak-server.properties
```



**总结**

1. 配置文件可以修改名称，通过启动参数设定
2. 配置文件可以修改路径，通过启动参数设定
3. 微服务开发中配置文件通过配置中心进行设置



### 3、多环境开发

常见的多环境开发主要兼顾3种环境设置

- 开发环境——个人电脑使用
- 测试环境——公司使用
- 生产环境——甲方使用



#### 3-1.多环境开发（yaml单一文件版）

```yaml
spring:
	profiles:
		active: pro		# 启动pro
---
spring:
	profiles: pro
server:
	port: 80
---
spring:
	profiles: dev
server:
	port: 81
---
spring:
	profiles: test
server:
	port: 82
```

其中关于环境名称定义上述格式是过时格式，标准格式如下

```yaml
spring:
	config:
    	activate:
        	on-profile: pro
```

**总结**

1. 多环境开发需要设置若干种常用环境，例如开发、生产、测试环境
2. yaml格式中设置多环境使用---区分环境设置边界
3. 每种环境的区别在于加载的配置属性不同
4. 启用某种环境时需要指定启动时使用该环境



#### 3-2.多环境开发（yaml多文件版）

文件的命名规则为：application-环境名.yml

- 主配置文件中设置公共配置（全局）
- 环境分类配置文件中常用于设置冲突属性（局部）

**主配置文件**

```yaml
spring:
	profiles:
		active: pro		# 启动pro
```

**环境配置文件**

```yaml
server:
	port: 80
```

**application-pro.yaml**

```yaml
server:
	port: 80
```

**application-dev.yaml**

```yaml
server:
	port: 81
```

**总结**

1. 可以使用独立配置文件定义环境属性

2. 独立配置文件便于线上系统维护更新并保障系统安全性

   

#### 3-3.多环境开发（properties多文件版）

> SpringBoot最早期提供的配置文件格式是properties格式

文件的命名规则为：application-环境名.properties

**主配置文件**

```properties
spring.profiles.active=pro
```

**环境配置文件**

**application-pro.properties**

```properties
server.port=80
```

**application-dev.properties**

```properties
server.port=81
```



**总结**

1. properties文件多环境配置仅支持多文件格式



#### 3-4.多环境开发独立配置文件书写技巧

将所有的配置根据功能对配置文件中的信息进行拆分，并制作成独立的配置文件，命名规则如下

- application-devDB.yml
- application-devRedis.yml
- application-devMVC.yml

使用include属性在激活指定环境的情况下，同时对多个环境进行加载使其生效，多个环境间使用逗号分隔

```yaml
spring:
	profiles:
    	active: dev
        include: devDB,devRedis,devMVC
```

**注意：**当主环境dev与其他环境有相同属性时，主环境属性生效；其他环境中有相同属性时，最后加载的环境属性生效

**改良：**SpringBoot从2.4版开始使用group属性替代include属性，降低配置书写量

```yaml
spring:
	profiles:
		active: dev
		group:
			"dev": devDB,devRedis,devMVC
			"pro": proDB,proRedis,proMVC
			"test": testDB,testRedis,testMVC
```

**总结**

1. 多环境开发使用group属性设置配置文件分组，便于线上维护管理



#### 3-5.多环境开发控制

maven和springboot同时设置多环境时谁生效？

大体思想如下：

- 先在maven环境中设置用什么具体的环境
- 在SpringBoot中读取maven设置的环境即可

**maven中设置多环境（使用属性方式区分环境）**

```xml
<profiles>
    <profile>
        <id>env_dev</id>
        <properties>
            <profile.active>dev</profile.active>
        </properties>
        <activation>
            <activeByDefault>true</activeByDefault>		<!--默认启动环境-->
        </activation>
    </profile>
    <profile>
        <id>env_pro</id>
        <properties>
            <profile.active>pro</profile.active>
        </properties>
    </profile>
</profiles>
```

**SpringBoot中读取maven设置值**

```yaml
spring:
	profiles:
    	active: @profile.active@
```

读取maven中配置的属性值的语法格式：@属性名@

**总结**

1. 当Maven与SpringBoot同时对多环境进行控制时，以Mavn为主，SpringBoot使用@..@占位符读取Maven对应的配置属性值
2. 基于SpringBoot读取Maven配置属性的前提下，如果在Idea下测试工程时pom.xml每次更新需要手动compile方可生效



### 4、日志

- 编程期调试代码
- 运营期记录信息
- 记录日常运营重要信息（峰值流量、平均响应时长……）
- 记录应用报错信息（错误堆栈）
- 记录运维过程数据（扩容、宕机、报警……）

#### 4-1.代码中使用日志工具记录日志

**步骤①**：添加日志记录操作

```JAVA
@RestController
@RequestMapping("/books")
public class BookController extends BaseClass{
    private static final Logger log = LoggerFactory.getLogger(BookController.class);
    @GetMapping
    public String getById(){
        log.debug("debug...");
        log.info("info...");
        log.warn("warn...");
        log.error("error...");
        return "springboot is running...2";
    }
}
```

**步骤②**：设置日志输出级别

日志的级别分为6种，分别是：

- TRACE：运行堆栈信息，使用率低
- DEBUG：程序员调试代码使用
- INFO：记录运维过程数据
- WARN：记录运维过程报警数据
- ERROR：记录错误堆栈信息
- FATAL：灾难信息，合并计入ERROR

一般情况下，开发时候使用DEBUG，上线后使用INFO，运维信息记录使用WARN即可。

简单设置日志级别：

```yaml
# 开启debug模式，输出调试信息，常用于检查系统运行状况
debug: true
```

日志系统通常都提供细粒度的控制

```yaml
# 开启debug模式，输出调试信息，常用于检查系统运行状况
debug: true

# 设置日志级别，root表示根节点，即整体应用日志级别
logging:
	level:
    	root: debug
```

**步骤③**：设置日志组，控制指定包对应的日志输出级别，也可以直接控制指定包对应的日志输出级别

```yaml
logging:
	# 设置日志组
    group:
    	# 自定义组名，设置当前组中所包含的包
        ebank: com.goes.controller
    level:
    	root: warn
        # 为对应组设置日志级别
        ebank: debug
    	# 为对包设置日志级别
        com.goes.controller: debug
```

**总结**

1. 日志用于记录开发调试与运维过程消息
2. 日志的级别共6种，通常使用4种即可，分别是DEBUG，INFO,WARN,ERROR
3. 可以通过日志组或代码包的形式进行日志显示级别的控制



#### 优化日志对象创建代码

未导入lombok前：使用代码手动创建日志对象

```JAVA
@RestController
@RequestMapping("/books")
public class BookController extends BaseClass{
    private static final Logger log = LoggerFactory.getLogger(BookController.class);
}
```

导入lombok后：使用注解搞定，日志对象名为log

```JAVA
@Slf4j // 开启日志功能
@RestController
@RequestMapping("/books")
public class BookController extends BaseClass{
    // private static final Logger log = LoggerFactory.getLogger(BookController.class);
}
```

**总结**

1. 基于lombok提供的@Slf4j注解为类快速添加日志对象



#### 4-2.日志输出格式控制

![image-20211206123431222](images/image-20211206123431222.png)



模拟的官方日志模板的书写格式：

```yaml
logging:
	pattern:
    	console: "%d %clr(%p) --- [%16t] %clr(%-40.40c){cyan} : %m %n"
```

**总结**

1. 日志输出格式设置规则



#### 4-3.日志文件

日志不能仅显示在控制台上，要把日志记录到文件中，方便后期维护查阅。

对于日志文件的使用存在各种各样的策略，例如每日记录，分类记录，报警后记录等。

记录日志到文件中格式非常简单，设置日志文件名即可。

```YAML
logging:
	file:
    	name: server.log
```

一个文件记录是无法满足运维要求的，通常会每天记录日志文件，同时为了便于维护，还要限制每个日志文件的大小。

日志文件的常用配置方式：

```YAML
logging:
	logback:
    	rollingpolicy:
        	max-file-size: 3KB
            file-name-pattern: server.%d{yyyy-MM-dd}.%i.log # %d是标识日期，%i是递增变量 => 用于区分日志文件
```

上述格式是基于logback日志技术设置每日日志文件的设置格式，要求容量到达3KB后转存信息到下个文件中。

**总结**

1. 日志记录到文件
2. 日志文件格式设置



## SpringBoot开发实用篇

开发实用篇具体包含的内容如下：

- 热部署
- 配置高级
- 测试
- 数据层解决方案
- 整合第三方技术
- 监控

### 1、热部署

**非springboot项目热部署实现原理**

实现热部署的效果有两种做法：核心思想一致：使用服务器去监控其中加载的应用，发现产生变化就重新加载一次。

- 一种是在tomcat服务器的配置文件中进行配置

- 另一种做法是通过IDE工具进行配置，比如在IDEA工具中进行设置。

  

**springboot项目热部署实现原理**

#### 1-1.手动启动热部署

**步骤①**：导入开发者工具对应的坐标

```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

**步骤②**：构建项目，可以使用快捷键激活此功能

```CMD
<CTRL>+<F9>
```



**重启与重载**

一个springboot项目在运行时实际上是分两个过程进行的，根据加载的东西不同，划分成base类加载器与restart类加载器。

- base类加载器：用来加载jar包中的类，jar包中的类和配置文件由于不会发生变化，因此不管加载多少次，加载的内容不会发生变化
- restart类加载器：用来加载开发者自己开发的类、配置文件、页面等信息（该类文件受开发者影响）



当springboot项目启动时，base类加载器执行，加载jar包中的信息后，restart类加载器执行，加载开发者制作的内容。当执行构建项目后，由于jar中的信息不会变化，因此base类加载器无需再次执行，因此仅仅运行restart类加载即可，也就是将开发者制作的内容重新加载就行，如此就完成一次热部署的过程，也可以说热部署的过程实际上是重新加载restart类加载器中的信息。

**总结**

1. 使用开发者工具可以为当前项目开启热部署功能
2. 使用构建项目操作对工程进行热部署



#### 1-2.自动启动热部署

**步骤①**：设置自动构建项目

打开【File】-> 选择【settings...】-> 在面板左侧的菜单中找到【Compile】选项 -> 勾选【Build project automatically】

![](E:\StudyNode\myData\SpringBoot\image\image-20240503170801855-17148256392193.png)



**步骤②**：允许在程序运行时进行自动构建

使用快捷键【Ctrl】+【Alt】+【Shit】+【/】打开维护面板，选择第1项【Registry...】

![image-20220222124006910](images/image-20220222124006910.png)

在选项中搜索comple，然后勾选对应项即可

<img src="images/image-20220222124240069.png" alt="image-20220222124240069" style="zoom:80%;" />

**注意：**idea设置当idea工具失去焦点5秒后进行热部署。也就是从idea工具中切换到其他工具时进行热部署。

**总结**

1. 自动热部署要开启自动构建项目
2. 自动热部署要开启在程序运行时自动构建项目



#### 1-3.参与热部署监控的文件范围配置

配置中默认不参与热部署的目录信息如下

- /META-INF/maven
- /META-INF/resources
- /resources
- /static
- /public
- /templates

如果想修改配置，可以通过application.yml文件进行设定哪些文件不参与热部署操作

```yaml
spring:
  devtools:
    restart:
      # 设置不参与热部署的文件或文件夹
      exclude: static/**,public/**,config/application.yml
```

**总结**

1. 通过配置可以修改不参与热部署的文件或目录



#### 1-4.关闭热部署

线上环境运行时是不可能使用热部署功能的，因此需要强制关闭此功能，通过配置可以关闭此功能。

```yaml
spring:
  devtools:
    restart:
      enabled: false
```

如果当心配置文件层级过多导致相符覆盖最终引起配置失效，可以提高配置的层级，在更高层级中配置关闭热部署。

例如在启动容器前通过系统属性设置关闭热部署功能。

```JAVA
@SpringBootApplication
public class SSMPApplication {
    public static void main(String[] args) {
        System.setProperty("spring.devtools.restart.enabled","false");
        SpringApplication.run(SSMPApplication.class);
    }
}
```

线上环境运行时禁用热部署功能：唯一的作用是降低资源消耗

**总结**

1. 通过配置可以关闭热部署功能降低线上程序的资源消耗



### 2、配置高级

#### 2-1.@ConfigurationProperties

@ConfigurationProperties注解的作用是用来为bean绑定属性的。

1、在yml配置文件中以对象的格式添加若干属性

```YML
servers:
  ip-address: 192.168.0.1 
  port: 2345
  timeout: -1
```

2、开发用来封装数据的实体类：注意要提供属性对应的setter方法

```JAVA
@Component
@Data
public class ServerConfig {
    private String ipAddress;
    private int port;
    private long timeout;
}
```

3、使用@ConfigurationProperties注解将配置中的属性值关联到开发的模型类上

```JAVA
@Component
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    private String ipAddress;
    private int port;
    private long timeout;
}
```



使用@ConfigurationProperties注解为第三方bean加载属性：

**步骤①**：使用@Bean注解定义第三方bean

```JAVA
@Bean
public DruidDataSource datasource(){
    DruidDataSource ds = new DruidDataSource();
    return ds;
}
```

**步骤②**：在yml中定义要绑定的属性：注意datasource此时全小写

```YAML
datasource:
  driverClassName: com.mysql.jdbc.Driver
```

**步骤③**：使用@ConfigurationProperties注解为第三方bean进行属性绑定：注意前缀是全小写的datasource

```JAVA
@Bean
@ConfigurationProperties(prefix = "datasource")
public DruidDataSource datasource(){
    DruidDataSource ds = new DruidDataSource();
    return ds;
}
```



@EnableConfigurationProperties：专门标注使用@ConfigurationProperties注解绑定属性的bean是哪些。

**步骤①**：在配置类上开启@EnableConfigurationProperties注解，并标注要使用@ConfigurationProperties注解绑定属性的类

```java
@SpringBootApplication
@EnableConfigurationProperties(ServerConfig.class)
public class SpringbootConfigurationApplication {
}
```

**步骤②**：在对应的类上直接使用@ConfigurationProperties进行属性绑定

```JAVA
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    private String ipAddress;
    private int port;
    private long timeout;
}
```

现在绑定属性的ServerConfig类并没有声明@Component注解。当使用@EnableConfigurationProperties注解时，spring会默认将其标注的类定义为bean，因此无需再次声明@Component注解。

> 使用@ConfigurationProperties注解时会出现一个提示信息
>
> ![image-20220222145535749](images/image-20220222145535749.png)
>
> 只需要添加一个坐标此提醒即可消失
>
> ```XML
> <dependency>
>  <groupId>org.springframework.boot</groupId>
>  <artifactId>spring-boot-configuration-processor</artifactId>
> </dependency>
> ```

**总结**

1. 使用@ConfigurationProperties可以为使用@Bean声明的第三方bean绑定属性
2. 当使用@EnableConfigurationProperties声明进行属性绑定的bean后，无需使用@Component注解再次进行bean声明



#### 2-2.宽松绑定/松散绑定

什么是宽松绑定？即配置文件中的命名格式与变量名的命名格式可以进行格式上的最大化兼容（几乎主流的命名格式都支持）。

例如：在ServerConfig中的ipAddress属性名

```JAVA
@Component
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    private String ipAddress;
}
```

可以与下面的配置属性名规则全兼容

```YML
servers:
  ipAddress: 192.168.0.2       # 驼峰模式
  ip_address: 192.168.0.2      # 下划线模式
  ip-address: 192.168.0.2      # 烤肉串模式
  IP_ADDRESS: 192.168.0.2      # 常量模式
```

springboot官方推荐使用烤肉串模式，也就是中划线模式。

**总结**

1. @ConfigurationProperties绑定属性时支持属性名宽松绑定，这个宽松体现在属性名的命名规则上
2. @Value注解不支持松散绑定规则
3. 绑定前缀名推荐采用烤肉串命名规则，即使用中划线做分隔符



#### 2-3.常用计量单位绑定

配置值：超时时间timeout->描述服务器操作超时时间，当前值是-1表示永不超时。

```YML
servers:
  ip-address: 192.168.0.1 
  port: 2345
  timeout: -1
```

以下模型类中添加两个JDK8中新增的类，分别是Duration和DataSize

```JAVA
@Component
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    @DurationUnit(ChronoUnit.HOURS)
    private Duration serverTimeOut;
    @DataSizeUnit(DataUnit.MEGABYTES)
    private DataSize dataSize;
}

```

**Duration**：表示时间间隔，可以通过@DurationUnit注解描述时间单位，例如上例中描述的单位为小时（ChronoUnit.HOURS）

**DataSize**：表示存储空间，可以通过@DataSizeUnit注解描述存储空间单位，例如上例中描述的单位为MB（DataUnit.MEGABYTES）

Druation常用单位如下：

<img src="images/image-20220222173911102.png" alt="image-20220222173911102" style="zoom:50%;" />

DataSize常用单位如下：

<img src="images/image-20220222174130102.png" alt="image-20220222174130102" style="zoom:67%;" />



#### 2-4.校验

**步骤①**：开启校验框架

```xml
<!--1.导入JSR303规范-->
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
</dependency>
<!--使用hibernate框架提供的校验器做实现-->
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
</dependency>
```

**步骤②**：在需要开启校验功能的类上使用注解@Validated开启校验功能

```java
@Component
@Data
@ConfigurationProperties(prefix = "servers")
// 开启对当前bean的属性注入校验
@Validated
public class ServerConfig {
}
```

**步骤③**：对具体的字段设置校验规则

```JAVA
@Component
@Data
@ConfigurationProperties(prefix = "servers")
// 开启对当前bean的属性注入校验
@Validated
public class ServerConfig {
    // 设置具体的规则
    @Max(value = 8888,message = "最大值不能超过8888")
    @Min(value = 202,message = "最小值不能低于202")
    private int port;
}
```

**总结**

1. 开启Bean属性校验功能一共3步：导入JSR303与Hibernate校验框架坐标、使用@Validated注解启用校验功能、使用具体校验规则规范数据校验格式

   

#### 2-5.数据类型转换

在整数相关知识中：**支持二进制，八进制，十六进制**

<img src="images/image-20220222225748370.png" alt="image-20220222225748370" style="zoom:80%;" />

因此数字0127在开发者眼中是一个字符串“0127”，但是在springboot看来是一个数字，而且是一个八进制的数字。

两个注意点:

1. 第一、字符串标准书写加上引号包裹；
2. 第二、遇到0开头的数据多注意吧。

**总结**

1. yaml文件中对于数字的定义支持进制书写格式，如需使用字符串请使用引号明确标注



### 3、测试

#### 3-1.加载测试专用属性

**临时属性**

在测试用例程序中，可以通过对注解@SpringBootTest添加属性来模拟临时属性，具体如下：

```JAVA
//properties属性可以为当前测试用例添加临时的属性配置
@SpringBootTest(properties = {"test.prop=testValue1"})
public class PropertiesAndArgsTest {

    @Value("${test.prop}")
    private String msg;
    
    @Test
    void testProperties(){
        System.out.println(msg);
    }
}
```

使用注解@SpringBootTest的properties属性就可以为当前测试用例添加临时的属性，覆盖源码配置文件中对应的属性值进行测试。



**临时参数**

通过命令行参数也可以设置属性值。而且线上启动程序时，通常会通过注解@SpringBootTes添加一些专用的配置信息。

```JAVA
// args属性可以为当前测试用例添加临时的命令行参数
@SpringBootTest(args={"--test.prop=testValue2"})
public class PropertiesAndArgsTest {
    
    @Value("${test.prop}")
    private String msg;
    
    @Test
    void testProperties(){
        System.out.println(msg);
    }
}
```

使用注解@SpringBootTest的args属性就可以为当前测试用例模拟命令行参数并进行测试。

在属性加载的优先级设定中：明确规定命令行参数的优先级排序是11，而配置属性的优先级是3，即args属性配置优先于properties属性配置加载。

<img src="images/image-20211206100859236.png" alt="image-20211206100859236" style="zoom:67%;" />

**总结**

1. 加载测试临时属性可以通过注解@SpringBootTest的properties和args属性进行设定，此设定应用范围仅适用于当前测试用例



#### 3-2.加载测试专用配置

一个spring环境中可以设置若干个配置文件或配置类，若干个配置信息可以同时生效。

添加一个配置类仅在测试环境中生效此配置，具体操作步骤如下：

**步骤①**：在测试包test中创建专用的测试环境配置类

```java
@Configuration
public class MsgConfig {
    @Bean
    public String msg(){
        return "bean msg";
    }
}
```

**步骤②**：在启动测试环境时，导入测试环境专用的配置类，使用@Import注解即可实现

```java
@SpringBootTest
@Import({MsgConfig.class})
public class ConfigurationTest {

    @Autowired
    private String msg;

    @Test
    void testConfiguration(){
        System.out.println(msg);
    }
}
```

**总结**

1. 定义测试环境专用的配置类，然后通过@Import注解在具体的测试中导入临时的配置，例如测试用例，方便测试过程，且上述配置不影响其他的测试类环境



#### 3-3.Web环境模拟测试

在测试用例中测试表现层接口该项工作的两件主要任务：

- 如何在测试类中启动web测试
- 如何在测试类中发送web请求

**测试类中启动web环境**

每一个springboot的测试类上方都会标准@SpringBootTest注解，而注解带有一个属性，叫做webEnvironment。通过该属性就可以设置在测试用例中启动web环境，具体如下：

```JAVA
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class WebTest {	
}
```

测试类中启动web环境时，可以指定启动的Web环境对应的端口，springboot提供4种设置值，分别如下：

<img src="images/image-20220223125453317.png" alt="image-20220223125453317" style="zoom:80%;" />

- MOCK：根据当前设置确认是否启动web环境，例如使用了Servlet的API就启动web环境，属于适配性的配置
- DEFINED_PORT：使用自定义的端口作为web服务器端口
- RANDOM_PORT：使用随机端口作为web服务器端口
- NONE：不启动web环境



**测试类中发送请求**

springboot为便于开发者进行对应的功能开发，简化开发步骤，具体操作如下：

**步骤①**：在测试类中开启web虚拟调用功能，通过注解@AutoConfigureMockMvc实现此功能的开启

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
// 开启虚拟MVC调用
@AutoConfigureMockMvc
public class WebTest {
}
```

**步骤②**：定义发起虚拟调用的对象MockMVC，通过自动装配的形式初始化对象

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
// 开启虚拟MVC调用
@AutoConfigureMockMvc
public class WebTest {

    @Test
    void testWeb(@Autowired MockMvc mvc) {
    }
}
```

**步骤③**：创建一个虚拟请求对象，封装请求的路径，并使用MockMVC对象发送对应请求

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
//开启虚拟MVC调用
@AutoConfigureMockMvc
public class WebTest {

    @Test
    void testWeb(@Autowired MockMvc mvc) throws Exception {
        //http://localhost:8080/books
        //创建虚拟请求，当前访问/books
        MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
        //执行对应的请求
        mvc.perform(builder);
    }
}
```

注意访问路径不要写http://localhost:8080/books，因为服务器IP地址和端口使用的是当前虚拟的web环境，无需指定，仅指定请求的具体路径即可。

**总结**

1. 在测试类中测试web层接口要保障测试类启动时启动web容器，使用@SpringBootTest注解的webEnvironment属性可以虚拟web环境用于测试
2. 为测试方法注入MockMvc对象，通过MockMvc对象可以发送虚拟请求，模拟web请求调用过程



**web环境请求结果比对**

解决发送请求后如何比对发送结果的问题。常见的比对内容如下：

- 响应状态匹配

  ```JAVA
  @Test
  void testStatus(@Autowired MockMvc mvc) throws Exception {
      MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
      ResultActions action = mvc.perform(builder);
      //设定预期值 与真实值进行比较，成功测试通过，失败测试失败
      //定义本次调用的预期值
      StatusResultMatchers status = MockMvcResultMatchers.status();
      //预计本次调用时成功的：状态200
      ResultMatcher ok = status.isOk();
      //添加预计值到本次调用过程中进行匹配
      action.andExpect(ok);
  }
  ```

- 响应体匹配（非json数据格式）

  ```JAVA
  @Test
  void testBody(@Autowired MockMvc mvc) throws Exception {
      MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
      ResultActions action = mvc.perform(builder);
      //设定预期值 与真实值进行比较，成功测试通过，失败测试失败
      //定义本次调用的预期值
      ContentResultMatchers content = MockMvcResultMatchers.content();
      ResultMatcher result = content.string("springboot2");
      //添加预计值到本次调用过程中进行匹配
      action.andExpect(result);
  }
  ```

- 响应体匹配（json数据格式，开发中的主流使用方式）

  ```JAVA
  @Test
  void testJson(@Autowired MockMvc mvc) throws Exception {
      MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
      ResultActions action = mvc.perform(builder);
      //设定预期值 与真实值进行比较，成功测试通过，失败测试失败
      //定义本次调用的预期值
      ContentResultMatchers content = MockMvcResultMatchers.content();
      ResultMatcher result = content.json("{\"id\":1,\"name\":\"springboot2\",\"type\":\"springboot\"}");
      //添加预计值到本次调用过程中进行匹配
      action.andExpect(result);
  }
  ```

- 响应头信息匹配

  ```JAVA
  @Test
  void testContentType(@Autowired MockMvc mvc) throws Exception {
      MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
      ResultActions action = mvc.perform(builder);
      //设定预期值 与真实值进行比较，成功测试通过，失败测试失败
      //定义本次调用的预期值
      HeaderResultMatchers header = MockMvcResultMatchers.header();
      ResultMatcher contentType = header.string("Content-Type", "application/json");
      //添加预计值到本次调用过程中进行匹配
      action.andExpect(contentType);
  }
  ```

以下范例就是三种信息同时进行匹配校验，也是一个完整的信息匹配过程。

```JAVA
@Test
void testGetById(@Autowired MockMvc mvc) throws Exception {
    MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
    ResultActions action = mvc.perform(builder);

    StatusResultMatchers status = MockMvcResultMatchers.status();
    ResultMatcher ok = status.isOk();
    action.andExpect(ok);

    HeaderResultMatchers header = MockMvcResultMatchers.header();
    ResultMatcher contentType = header.string("Content-Type", "application/json");
    action.andExpect(contentType);

    ContentResultMatchers content = MockMvcResultMatchers.content();
    ResultMatcher result = content.json("{\"id\":1,\"name\":\"springboot\",\"type\":\"springboot\"}");
    action.andExpect(result);
}
```

**总结**

1. web虚拟调用可以对本地虚拟请求的返回响应信息进行比对，分为响应头信息比对、响应体信息比对、响应状态信息比对



#### 3-4.数据层测试回滚

在原始测试用例中添加注解@Transactional即可实现当前测试用例的事务不提交。当程序运行后，只要注解@Transactional出现的位置存在注解@SpringBootTest，springboot就会认为这是一个测试程序，无需提交事务，所以也就可以避免事务的提交。

```JAVA
@SpringBootTest
@Transactional
@Rollback(true)
public class DaoTest {
    @Autowired
    private BookService bookService;

    @Test
    void testSave(){
        Book book = new Book();
        book.setName("springboot3");
        book.setType("springboot3");
        book.setDescription("springboot3");

        bookService.save(book);
    }
}
```

如果开发者想提交事务，只需再添加一个@RollBack的注解，设置回滚状态为false即可正常提交事务。

**总结**

1. 在springboot的测试类中通过添加注解@Transactional来阻止测试用例提交事务
2. 通过注解@Rollback控制springboot测试类执行结果是否提交事务，需要配合注解@Transactional使用



#### 3-5.测试用例数据设定

springboot提供在配置中使用随机值的机制，确保每次运行程序加载的数据都是随机的。具体如下：

```yaml
testcase:
  book:
    id: ${random.int}
    id2: ${random.int(10)}
    type: ${random.int!5,10!}
    name: ${random.value}
    uuid: ${random.uuid}
    publishTime: ${random.long}
```

当前配置就可以在每次运行程序时创建一组随机数据，避免每次运行时数据都是固定值的尴尬现象发生，有助于测试功能的进行。

数据的加载按照之前加载数据的形式，使用@ConfigurationProperties注解即可

```JAVA
@Component
@Data
@ConfigurationProperties(prefix = "testcase.book")
public class BookCase {
    private int id;
    private int id2;
    private int type;
    private String name;
    private String uuid;
    private long publishTime;
}
```

对于随机值的产生，还有一些小的限定规则，比如产生的数值性数据可以设置范围等，具体如下：

<img src="images/image-20220223135454862.png" alt="image-20220223135454862" style="zoom:80%;" />

- ${random.int}表示随机整数
- ${random.int(10)}表示10以内的随机数
- ${random.int(10,20)}表示10到20的随机数
- 其中()可以是任意字符，例如[]，!!均可

**总结**

1. 使用随机数据可以替换测试用例中书写的固定数据，提高测试用例中的测试数据有效性



### 4、数据层解决方案

#### 4-1.SQL

MySQL数据库、MyBatisPlus框架、Druid数据源的配置，三个技术分别对应数据层操作的三个层面：

- 数据源技术：Druid
- 持久化技术：MyBatisPlus
- 数据库技术：MySQL

##### 数据源技术

springboot提供3款内嵌数据源技术，分别如下：

- HikariCP

  springboot官方推荐的数据源技术，作为默认内置数据源使用。

- Tomcat提供DataSource

  如果不想用HikartCP，并且使用tomcat作为web服务器进行web程序的开发（web技术导入starter后，默认使用内嵌tomcat）

- Commons DBCP

  既不使用HikartCP也不使用tomcat的DataSource时，默认使用。

  

配置druid时使用druid的starter对应的配置如下：

```YAML
spring:
  datasource:
    druid:	
   	  url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root
      password: root
```

配置默认的数据源HikariCP，如下：（删除druid即可）

```YAML
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: root
```

或者对url地址单独配置，如下：

```YAML
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    hikari:
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root
      password: root
```

对hikari数据源配置独立的属性。例如：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    hikari:
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root
      password: root
      maximum-pool-size: 50
```

**总结**

1. springboot技术提供了3种内置的数据源技术，分别是Hikari、tomcat内置数据源、DBCP



##### 持久化技术

springboot充分发挥其最强辅助的特征，给开发者提供一套现成的数据层技术，叫做JdbcTemplate。

操作步骤：

**步骤①**：导入jdbc对应的坐标，记得是starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency
```

**步骤②**：自动装配JdbcTemplate对象

```java
@SpringBootTest
class Springboot15SqlApplicationTests {
    @Test
    void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){
    }
}
```

**步骤③**：使用JdbcTemplate实现查询操作（非实体类封装数据的查询操作）

```java
@Test
void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){
    String sql = "select * from tbl_book";
    List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
    System.out.println(maps);
}
```

**步骤④**：使用JdbcTemplate实现查询操作（实体类封装数据的查询操作）

```java
@Test
void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){

    String sql = "select * from tbl_book";
    RowMapper<Book> rm = new RowMapper<Book>() {
        @Override
        public Book mapRow(ResultSet rs, int rowNum) throws SQLException {
            Book temp = new Book();
            temp.setId(rs.getInt("id"));
            temp.setName(rs.getString("name"));
            temp.setType(rs.getString("type"));
            temp.setDescription(rs.getString("description"));
            return temp;
        }
    };
    List<Book> list = jdbcTemplate.query(sql, rm);
    System.out.println(list);
}
```

**步骤⑤**：使用JdbcTemplate实现增删改操作

```java
@Test
void testJdbcTemplateSave(@Autowired JdbcTemplate jdbcTemplate){
    String sql = "insert into tbl_book values(3,'springboot1','springboot2','springboot3')";
    jdbcTemplate.update(sql);
}
```

​		如果想对JdbcTemplate对象进行相关配置，可以在yml文件中进行设定，具体如下：

```yaml
spring:
  jdbc:
    template:
      query-timeout: -1   # 查询超时时间
      max-rows: 500       # 最大行数
      fetch-size: -1      # 缓存行数
```

**总结**

1. SpringBoot内置JdbcTemplate持久化解决方案
2. 使用JdbcTemplate需要导入spring-boot-starter-jdbc的坐标



##### 数据库技术

springboot提供了3款内置的数据库，分别是

- H2
- HSQL
- Derby

以H2数据库为例，操作步骤：

**步骤①**：导入H2数据库对应的坐标（2个）

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

**步骤②**：将工程设置为web工程，启动工程时启动H2数据库

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**步骤③**：通过配置开启H2数据库控制台访问程序，也可以使用其他的数据库连接软件操作

```yaml
spring:
  h2:
    console:
      enabled: true
      path: /h2
```

web端访问路径/h2，访问密码123456，如果访问失败，先配置下列数据源，启动程序运行后再次访问/h2路径就可以正常访问了

```yaml
datasource:
  url: jdbc:h2:~/test
  hikari:
    driver-class-name: org.h2.Driver
    username: sa
    password: 123456
```

**步骤④**：使用JdbcTemplate或MyBatisPlus技术操作数据库

（略）



注意事项：项目上线时，把内存级数据库关闭，采用MySQL数据库作为数据持久化方案，关闭方式就是设置enabled属性为false即可。

**总结**

1. H2内嵌式数据库启动方式，添加坐标，添加配置
2. H2数据库线上运行时请务必关闭



**SQL总结：**

- 数据源技术：Druid、Hikari、tomcat DataSource、DBCP
- 持久化技术：MyBatisPlus、MyBatis、JdbcTemplate
- 数据库技术：MySQL、H2、HSQL、Derby



#### 4-2.NoSQL

> NoSQL - 非关系型数据库

国内市场上最流行的几款NoSQL数据库整合方案：Redis、MongoDB、ES。

##### SpringBoot整合Redis

> 安装：windows版安装包下载地址：https://github.com/tporadowski/redis/releases

**启动服务器**

```CMD
redis-server.exe redis.windows.conf
```

**启动客户端**

```CMD
redis-cli.exe
```



springboot整合redis，操作步骤如下：

**步骤①**：导入springboot整合redis的starter坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

**步骤②**：进行基础配置

```yaml
spring:
  redis:
    host: localhost
    port: 6379
```

**步骤③**：使用springboot整合redis的专用客户端接口操作，此处使用的是RedisTemplate

```java
@SpringBootTest
class Springboot16RedisApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;
    @Test
    void set() {
        ValueOperations ops = redisTemplate.opsForValue();
        ops.set("age",41);
    }
    @Test
    void get() {
        ValueOperations ops = redisTemplate.opsForValue();
        Object age = ops.get("name");
        System.out.println(age);
    }
    @Test
    void hset() {
        HashOperations ops = redisTemplate.opsForHash();
        ops.put("info","b","bb");
    }
    @Test
    void hget() {
        HashOperations ops = redisTemplate.opsForHash();
        Object val = ops.get("info", "b");
        System.out.println(val);
    }
}

```

各种类型的数据操作接口如下：

<img src="images/image-20220224103104908.png" alt="image-20220224103104908" style="zoom:80%;" />

**总结**

1. springboot整合redis步骤
   1. 导入springboot整合redis的starter坐标
   2. 进行基础配置
   3. 使用springboot整合redis的专用客户端接口RedisTemplate操作



**StringRedisTemplate**

springboot整合redis时提供专用的API接口StringRedisTemplate。

```JAVA
@SpringBootTest
public class StringRedisTemplateTest {
    @Autowired
    private StringRedisTemplate stringRedisTemplate;
    @Test
    void get(){
        ValueOperations<String, String> ops = stringRedisTemplate.opsForValue();
        String name = ops.get("name");
        System.out.println(name);
    }
}
```



**redis客户端选择**

springboot整合redis技术提供了多种客户端兼容模式，默认提供的是lettucs客户端技术，也可以根据需要切换成指定客户端技术。

例如jedis客户端技术，切换成jedis客户端技术操作步骤如下：

**步骤①**：导入jedis坐标（jedis坐标受springboot管理，无需提供版本号）

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

**步骤②**：配置客户端技术类型，设置为jedis

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    client-type: jedis
```

**步骤③**：根据需要设置对应的配置

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    client-type: jedis
    lettuce:
      pool:
        max-active: 16
    jedis:
      pool:
        max-active: 16
```

**lettcus与jedis区别**

- jedis连接Redis服务器是直连模式，当多线程模式下使用jedis会存在线程安全问题，解决方案可以通过配置连接池使每个连接专用，如此整体性能就大受影响
- lettcus基于Netty框架进行与Redis服务器连接，底层设计中采用StatefulRedisConnection。 StatefulRedisConnection自身是线程安全的，可以保障并发访问安全问题，因此一个连接可以被多线程复用。

**总结**

1. springboot整合redis提供了StringRedisTemplate对象，以字符串的数据格式操作redis
2. 如果需要切换redis客户端实现技术，可以通过配置的形式进行



##### SpringBoot整合MongoDB

MongoDB是一个开源、高性能、无模式的文档型数据库，它是NoSQL数据库产品中的一种，是最像关系型数据库的非关系型数据库。

使用MongoDB的场景：

- 淘宝用户数据
  - 存储位置：数据库
  - 特征：永久性存储，修改频度极低
- 游戏装备数据、游戏道具数据
  - 存储位置：数据库、Mongodb
  - 特征：永久性存储与临时存储相结合、修改频度较高
- 直播数据、打赏数据、粉丝数据
  - 存储位置：数据库、Mongodb
  - 特征：永久性存储与临时存储相结合，修改频度极高
- 物联网数据
  - 存储位置：Mongodb
  - 特征：临时存储，修改频度飞速



> 安装：windows版安装包下载地址：https://www.mongodb.com/try/download
>
> 下载Mongdb缺少一些文件的解决方案：在浏览器中搜索下载提示缺少的名称对应的文件，将下载的文件拷贝到windows安装目录的system32目录下，然后在命令行中执行regsvr32命令注册此文件。
>
> ```CMD
> regsvr32 vcruntime140_1.dll
> ```
>
> 注意：根据下载的文件名不同，执行命令前更改对应名称。



**启动服务器**

启动服务器时需要指定数据存储位置，通过参数--dbpath进行设置，可以根据需要自行设置数据存储路径。默认服务端口27017。

```CMD
mongod --dbpath=..\data\db
```

**启动客户端**

```CMD
mongo --host=127.0.0.1 --port=27017
```



**基本操作**

> 操作MongoDB数据库客户端软件：Robo3t，是一款绿色软件，无需安装，解压缩即可。

新增文档：（文档是一种类似json格式的数据，初学者可以先把数据理解为就是json数据）	

```CMD
db.集合名称.insert/save/insertOne(文档)
```

删除文档：

```CMD
db.集合名称.remove(条件)
```

修改文档：

```cmd
db.集合名称.update(条件，{操作种类:{文档}})
```

查询文档：

```CMD
基础查询
查询全部：		   db.集合.find();
查第一条：		   db.集合.findOne()
查询指定数量文档：	db.集合.find().limit(10)					//查10条文档
跳过指定数量文档：	db.集合.find().skip(20)					//跳过20条文档
统计：			  	db.集合.count()
排序：				db.集合.sort({age:1})						//按age升序排序
投影：				db.集合名称.find(条件,{name:1,age:1})		 //仅保留name与age域

条件查询
基本格式：			db.集合.find({条件})
模糊查询：			db.集合.find({域名:/正则表达式/})		  //等同SQL中的like，比like强大，可以执行正则所有规则
条件比较运算：		   db.集合.find({域名:{$gt:值}})				//等同SQL中的数值比较操作，例如：name>18
包含查询：			db.集合.find({域名:{$in:[值1，值2]}})		//等同于SQL中的in
条件连接查询：		   db.集合.find({$and:[{条件1},{条件2}]})	   //等同于SQL中的and、or
```



springboot整合MongoDB，操作步骤如下：

**步骤①**：导入springboot整合MongoDB的starter坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

**步骤②**：进行基础配置

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://localhost/test_db
```

**步骤③**：使用springboot整合MongoDB的专用客户端接口MongoTemplate来进行操作

```java
@SpringBootTest
class Springboot17MongodbApplicationTests {
    @Autowired
    private MongoTemplate mongoTemplate;
    @Test
    void contextLoads() {
        Book book = new Book();
        book.setId(2);
        book.setName("springboot2");
        book.setType("springboot2");
        book.setDescription("springboot2");
        mongoTemplate.save(book);
    }
    @Test
    void find(){
        List<Book> all = mongoTemplate.findAll(Book.class);
        System.out.println(all);
    }
}
```

**总结**

springboot整合MongoDB步骤

1. 导入springboot整合MongoDB的starter坐标
2. 进行基础配置
3. 使用springboot整合MongoDB的专用客户端接口MongoTemplate操作



##### SpringBoot整合ES

> ES（Elasticsearch）是一个分布式全文搜索引擎，重点是全文搜索。

要实现全文搜索的效果，不可能使用数据库中like操作去进行比对，效率太低。ES设计了一种全新的思想，来实现全文搜索。

> 安装：windows版安装包下载地址：[https://](https://www.elastic.co/cn/downloads/elasticsearch)[www.elastic.co/cn/downloads/elasticsearch](https://www.elastic.co/cn/downloads/elasticsearch)
>
> 目录结构：
>
> - bin目录：包含所有的可执行命令
> - config目录：包含ES服务器使用的配置文件
> - jdk目录：包含一个完整的jdk工具包，版本17，当ES升级时，使用最新版本的jdk确保不会出现版本支持性不足的问题
> - lib目录：包含ES运行的依赖jar文件
> - logs目录：包含ES运行后产生的所有日志文件
> - modules目录：包含ES软件中所有的功能模块（jar包）。和jar目录不同，jar目录是ES运行期间依赖的jar包，modules是ES软件自己的功能jar包
> - plugins目录：包含ES软件安装的插件，默认为空



**启动服务器**

双击elasticsearch.bat文件即可启动ES服务器，默认服务端口9200。

```CMD
elasticsearch.bat
```

通过浏览器访问http://localhost:9200看到如下信息视为ES服务器正常启动

```CMD
{
  "name" : "CZBK-**********",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "j137DSswTPG8U4Yb-0T1Mg",
  "version" : {
    "number" : "7.16.2",
    "build_flavor" : "default",
    "build_type" : "zip",
    "build_hash" : "2b937c44140b6559905130a8650c64dbd0879cfb",
    "build_date" : "2021-12-18T19:42:46.604893745Z",
    "build_snapshot" : false,
    "lucene_version" : "8.10.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```



**基本操作**

创建索引：books是索引名称（不能重复创建）

```CMD
PUT请求	http://localhost:9200/books
```

查询索引（若查询不存在的索引，会返回错误信息）

```CMD
GET请求	http://localhost:9200/books
```

删除索引（若重复删除，会给出错误信息）

```CMD
DELETE请求	http://localhost:9200/books
```

创建索引并指定分词器

> IK分词器下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases
>
> 分词器下载后解压到ES安装目录的plugins目录中即可。
>
> 安装分词器后需要重新启动ES服务器。

使用IK分词器创建索引格式：

```json
PUT请求		http://localhost:9200/books

请求参数如下（注意是json格式的参数）
{
    "mappings":{							#定义mappings属性，替换创建索引时对应的mappings属性		
        "properties":{						 #定义索引中包含的属性设置
            "id":{							#设置索引中包含id属性
                "type":"keyword"			 #当前属性可以被直接搜索
            },
            "name":{						#设置索引中包含name属性
                "type":"text",                #当前属性是文本信息，参与分词  
                "analyzer":"ik_max_word",     #使用IK分词器进行分词             
                "copy_to":"all"				 #分词结果拷贝到all属性中
            },
            "type":{
                "type":"keyword"
            },
            "description":{
                "type":"text",	                
                "analyzer":"ik_max_word",                
                "copy_to":"all"
            },
            "all":{							#定义属性，用来描述多个字段的分词结果集合，当前属性可以参与查询
                "type":"text",	                
                "analyzer":"ik_max_word"
            }
        }
    }
}
```

ES中称数据为文档，下面进行文档操作。

- 添加文档，有三种方式

  ```json
  POST请求	http://localhost:9200/books/_doc		#使用系统生成id
  POST请求	http://localhost:9200/books/_create/1	#使用指定id
  POST请求	http://localhost:9200/books/_doc/1		#使用指定id，不存在创建，存在更新（版本递增）
  
  文档通过请求参数传递，数据格式json
  {
      "name":"springboot",
      "type":"springboot",
      "description":"springboot"
  }  
  ```

- 查询文档

  ```json
  GET请求	http://localhost:9200/books/_doc/1		 #查询单个文档 		
  GET请求	http://localhost:9200/books/_search		 #查询全部文档
  ```

- 条件查询

  ```json
  GET请求	http://localhost:9200/books/_search?q=name:springboot	# q=查询属性名:查询属性值
  ```

- 删除文档

  ```json
  DELETE请求	http://localhost:9200/books/_doc/1
  ```

- 修改文档（全量更新）

  ```json
  PUT请求	http://localhost:9200/books/_doc/1
  
  文档通过请求参数传递，数据格式json
  {
      "name":"springboot",
      "type":"springboot",
      "description":"springboot"
  }
  ```

- 修改文档（部分更新）

  ```json
  POST请求	http://localhost:9200/books/_update/1
  
  文档通过请求参数传递，数据格式json
  {			
      "doc":{						#部分更新并不是对原始文档进行更新，而是对原始文档对象中的doc属性中的指定属性更新
          "name":"springboot"		 #仅更新提供的属性值，未提供的属性值不参与更新操作
      }
  }
  ```



springboot整合ES，操作步骤如下：

**步骤①**：导入springboot整合ES的starter坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

**步骤②**：进行基础配置

```yaml
spring:
  elasticsearch:
    rest:
      uris: http://localhost:9200
```

**步骤③**：使用springboot整合ES的专用客户端接口ElasticsearchRestTemplate来进行操作

```java
@SpringBootTest
class Springboot18EsApplicationTests {
    @Autowired
    private ElasticsearchRestTemplate template;
}
```



ES客户端种类：

- 低级别客户端：Low Level Client
- 高级别客户端：High Level Client

下面使用高级别客户端方式进行springboot整合ES，操作步骤如下：

**步骤①**：导入springboot整合ES高级别客户端的坐标，此种形式目前没有对应的starter

```xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
</dependency>
```

**步骤②**：使用编程的形式设置连接的ES服务器，并获取客户端对象

```java
@SpringBootTest
class Springboot18EsApplicationTests {
    private RestHighLevelClient client;
      @Test
      void testCreateClient() throws IOException {
          HttpHost host = HttpHost.create("http://localhost:9200");
          RestClientBuilder builder = RestClient.builder(host);
          client = new RestHighLevelClient(builder);
  
          client.close();
      }
}
```

**步骤③**：使用客户端对象操作ES，例如创建索引

```java
@SpringBootTest
class Springboot18EsApplicationTests {
    private RestHighLevelClient client;
      @Test
      void testCreateIndex() throws IOException {
          HttpHost host = HttpHost.create("http://localhost:9200");
          RestClientBuilder builder = RestClient.builder(host);
          client = new RestHighLevelClient(builder);
          
          CreateIndexRequest request = new CreateIndexRequest("books");
          client.indices().create(request, RequestOptions.DEFAULT); 
          
          client.close();
      }
}
```



由于无论进行ES何种操作，第一步永远是获取RestHighLevelClient对象，最后一步永远是关闭该对象的连接。而在测试中可以使用测试类的特性去帮助开发者一次性的完成上述操作，但是在业务书写时，还需要自行管理。

将上述代码格式转换成使用测试类的初始化方法和销毁方法进行客户端对象的维护。

```JAVA
@SpringBootTest
class Springboot18EsApplicationTests {
    @BeforeEach		//在测试类中每个操作运行前运行的方法
    void setUp() {
        HttpHost host = HttpHost.create("http://localhost:9200");
        RestClientBuilder builder = RestClient.builder(host);
        client = new RestHighLevelClient(builder);
    }

    @AfterEach		//在测试类中每个操作运行后运行的方法
    void tearDown() throws IOException {
        client.close();
    }

    private RestHighLevelClient client;

    @Test
    void testCreateIndex() throws IOException {
        CreateIndexRequest request = new CreateIndexRequest("books");
        client.indices().create(request, RequestOptions.DEFAULT);
    }
}
```

**创建索引（IK分词器）**：

```java
@Test
void testCreateIndexByIK() throws IOException {
    CreateIndexRequest request = new CreateIndexRequest("books");
    String json = "{\n" +
            "    \"mappings\":{\n" +
            "        \"properties\":{\n" +
            "            \"id\":{\n" +
            "                \"type\":\"keyword\"\n" +
            "            },\n" +
            "            \"name\":{\n" +
            "                \"type\":\"text\",\n" +
            "                \"analyzer\":\"ik_max_word\",\n" +
            "                \"copy_to\":\"all\"\n" +
            "            },\n" +
            "            \"type\":{\n" +
            "                \"type\":\"keyword\"\n" +
            "            },\n" +
            "            \"description\":{\n" +
            "                \"type\":\"text\",\n" +
            "                \"analyzer\":\"ik_max_word\",\n" +
            "                \"copy_to\":\"all\"\n" +
            "            },\n" +
            "            \"all\":{\n" +
            "                \"type\":\"text\",\n" +
            "                \"analyzer\":\"ik_max_word\"\n" +
            "            }\n" +
            "        }\n" +
            "    }\n" +
            "}";
    //设置请求中的参数
    request.source(json, XContentType.JSON);
    client.indices().create(request, RequestOptions.DEFAULT);
}
```

IK分词器是通过请求参数的形式进行设置的，设置请求参数使用request对象中的source方法进行设置。

当请求中需要参数时，均可使用当前形式进行参数设置。

**添加文档**：

```java
@Test
//添加文档
void testCreateDoc() throws IOException {
    Book book = bookDao.selectById(1);
    IndexRequest request = new IndexRequest("books").id(book.getId().toString());
    String json = JSON.toJSONString(book);
    request.source(json,XContentType.JSON);
    client.index(request,RequestOptions.DEFAULT);
}
```

添加文档使用的请求对象是IndexRequest，与创建索引使用的请求对象不同。	

**批量添加文档**：

先创建一个BulkRequest的对象，可以将该对象理解为是一个保存request对象的容器，将所有的请求都初始化好后，添加到BulkRequest对象中，再使用BulkRequest对象的bulk方法，一次性执行完毕。

```java
@Test
//批量添加文档
void testCreateDocAll() throws IOException {
    List<Book> bookList = bookDao.selectList(null);
    BulkRequest bulk = new BulkRequest();
    for (Book book : bookList) {
        IndexRequest request = new IndexRequest("books").id(book.getId().toString());
        String json = JSON.toJSONString(book);
        request.source(json,XContentType.JSON);
        bulk.add(request);
    }
    client.bulk(bulk,RequestOptions.DEFAULT);
}
```

**按id查询文档**：

根据id查询文档使用的请求对象是GetRequest。

```java
@Test
//按id查询
void testGet() throws IOException {
    GetRequest request = new GetRequest("books","1");
    GetResponse response = client.get(request, RequestOptions.DEFAULT);
    String json = response.getSourceAsString();
    System.out.println(json);
}
```

**按条件查询文档**：

按条件查询文档使用的请求对象是SearchRequest，查询时调用SearchRequest对象的termQuery方法，需要给出查询属性名，此处支持使用合并字段，也就是前面定义索引属性时添加的all属性。

```java
@Test
// 按条件查询
void testSearch() throws IOException {
    SearchRequest request = new SearchRequest("books");

    SearchSourceBuilder builder = new SearchSourceBuilder();
    builder.query(QueryBuilders.termQuery("all","spring"));
    request.source(builder);

    SearchResponse response = client.search(request, RequestOptions.DEFAULT);
    SearchHits hits = response.getHits();
    for (SearchHit hit : hits) {
        String source = hit.getSourceAsString();
        // System.out.println(source);
        Book book = JSON.parseObject(source, Book.class);
        System.out.println(book);
    }
}
```

**总结**

1. springboot整合ES步骤
   1. 导入springboot整合ES的High Level Client坐标
   2. 手工管理客户端对象，包括初始化和关闭操作
   3. 使用High Level Client根据操作的种类不同，选择不同的Request对象完成对应操作



### 5、整合第三方技术

#### 5-1.缓存

缓存是什么？

缓存是一种介于数据永久存储介质与应用程序之间的数据临时存储介质，使用缓存可以有效的减少低速数据读取过程的次数（例如磁盘IO），提高系统性能。此外缓存不仅可以用于提高永久性存储介质的数据读取效率，还可以提供临时的数据存储空间。



##### SpringBoot内置缓存解决方案

springboot技术提供有内置的缓存解决方案，可以帮助开发者快速开启缓存技术，并使用缓存技术进行数据的快速操作，例如读取缓存数据和写入数据到缓存。

在业务方法上面使用注解@Cacheable声明当前方法的返回值放入缓存中，其中要指定缓存的存储位置，以及缓存中保存当前方法返回值对应的名称。其中value属性描述缓存的存储位置，可视为一个存储空间名，key属性描述了缓存中保存数据的名称，使用#id读取形参中的id值作为缓存名称。

使用@Cacheable注解后，执行当前操作，如果发现对应名称在缓存中没有数据，就正常读取数据，然后放入缓存；如果对应名称在缓存中有数据，就终止当前业务方法执行，直接返回缓存中的数据。

**步骤①**：导入springboot提供的缓存技术对应的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**步骤②**：启用缓存，在引导类上方标注注解@EnableCaching配置springboot程序中可以使用缓存

```java
@SpringBootApplication
//开启缓存功能
@EnableCaching
public class Springboot19CacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot19CacheApplication.class, args);
    }
}
```

**步骤③**：设置操作的数据是否使用缓存

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;

    @Cacheable(value="cacheSpace",key="#id")
    public Book getById(Integer id) {
        return bookDao.selectById(id);
    }
}
```



###### 手机验证码案例

创建一个手机验证码的案例环境，模拟使用缓存保存手机验证码的过程。

手机验证码案例需求如下：

- 输入手机号获取验证码，组织文档以短信形式发送给用户（页面模拟）
- 输入手机号和验证码验证结果

为了描述上述操作，需制作两个表现层接口：

- 一个用来模拟发送短信的过程，其实就是根据用户提供的手机号生成一个验证码，然后放入缓存
- 另一个用来模拟验证码校验的过程，其实就是使用传入的手机号和验证码进行匹配，并返回最终匹配结果。

**步骤①**：导入springboot提供的缓存技术对应的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

**步骤②**：启用缓存，在引导类上方标注注解@EnableCaching配置springboot程序中可以使用缓存

```java
@SpringBootApplication
//开启缓存功能
@EnableCaching
public class Springboot19CacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot19CacheApplication.class, args);
    }
}
```

**步骤③**：定义验证码对应的实体类，封装手机号与验证码两个属性

```java
@Data
public class SMSCode {
    private String tele;
    private String code;
}
```

**步骤④**：定义验证码功能的业务层接口与实现类

```java
public interface SMSCodeService {
    public String sendCodeToSMS(String tele);
    public boolean checkCode(SMSCode smsCode);
}

@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;

    @CachePut(value = "smsCode", key = "#tele")
    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        //取出内存中的验证码与传递过来的验证码比对，如果相同，返回true
        String code = smsCode.getCode();
        String cacheCode = codeUtils.get(smsCode.getTele());
        return code.equals(cacheCode);
    }
}
```

获取验证码后，当验证码失效时必须重新获取验证码，因此在获取验证码的功能上不能使用@Cacheable注解，@Cacheable注解是缓存中没有值则放入值，缓存中有值则取值。此处的功能仅仅是生成验证码并放入缓存，并不具有从缓存中取值的功能，因此不能使用@Cacheable注解，应该使用仅具有向缓存中保存数据的功能，使用@CachePut注解即可。



**步骤⑤**：定义验证码的生成策略与根据手机号读取验证码的功能

```java
@Component
public class CodeUtils {
    private String [] patch = {"000000","00000","0000","000","00","0",""};

    public String generator(String tele){
        int hash = tele.hashCode();
        int encryption = 20206666;
        long result = hash ^ encryption;
        long nowTime = System.currentTimeMillis();
        result = result ^ nowTime;
        long code = result % 1000000;
        code = code < 0 ? -code : code;
        String codeStr = code + "";
        int len = codeStr.length();
        return patch[len] + codeStr;
    }

    @Cacheable(value = "smsCode",key="#tele")
    public String get(String tele){
        return null;
    }
}
```

**步骤⑥**：定义验证码功能的web层接口，一个方法用于提供手机号获取验证码，一个方法用于提供手机号和验证码进行校验

```java
@RestController
@RequestMapping("/sms")
public class SMSCodeController {
    @Autowired
    private SMSCodeService smsCodeService;
    
    @GetMapping
    public String getCode(String tele){
        String code = smsCodeService.sendCodeToSMS(tele);
        return code;
    }
    
    @PostMapping
    public boolean checkCode(SMSCode smsCode){
        return smsCodeService.checkCode(smsCode);
    }
}
```



##### SpringBoot整合Ehcache缓存

**步骤①**：导入Ehcache的坐标

```xml
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

**步骤②**：配置缓存技术实现使用Ehcache

```yaml
spring:
  cache:
    type: ehcache
    ehcache:
      config: ehcache.xml
```

由于ehcache的配置有独立的配置文件格式，因此还需要指定ehcache的配置文件，以便于读取相应配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <diskStore path="D:\ehcache" />

    <!--默认缓存策略 -->
    <!-- external：是否永久存在，设置为true则不会被清除，此时与timeout冲突，通常设置为false-->
    <!-- diskPersistent：是否启用磁盘持久化-->
    <!-- maxElementsInMemory：最大缓存数量-->
    <!-- overflowToDisk：超过最大缓存数量是否持久化到磁盘-->
    <!-- timeToIdleSeconds：最大不活动间隔，设置过长缓存容易溢出，设置过短无效果，可用于记录时效性数据，例如验证码-->
    <!-- timeToLiveSeconds：最大存活时间-->
    <!-- memoryStoreEvictionPolicy：缓存清除策略-->
    <defaultCache
        eternal="false"
        diskPersistent="false"
        maxElementsInMemory="1000"
        overflowToDisk="false"
        timeToIdleSeconds="60"
        timeToLiveSeconds="60"
        memoryStoreEvictionPolicy="LRU" />

    <cache
        name="smsCode"
        eternal="false"
        diskPersistent="false"
        maxElementsInMemory="1000"
        overflowToDisk="false"
        timeToIdleSeconds="10"
        timeToLiveSeconds="10"
        memoryStoreEvictionPolicy="LRU" />
</ehcache>
```

注意前面的案例中，设置了数据保存的位置是smsCode

```java
@CachePut(value = "smsCode", key = "#tele")
public String sendCodeToSMS(String tele) {
    String code = codeUtils.generator(tele);
    return code;
}	
```

**总结**

1. springboot使用Ehcache作为缓存实现需要导入Ehcache的坐标
2. 修改设置，配置缓存供应商为ehcache，并提供对应的缓存配置文件

​		

##### SpringBoot整合Redis缓存

**步骤①**：导入redis的坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

**步骤②**：配置缓存技术实现使用redis

```yaml
spring:
  redis:
    host: localhost
    port: 6379
  cache:
    type: redis
```

如果需要对redis作为缓存进行配置，注意不是对原始的redis进行配置，而是配置redis作为缓存使用相关的配置，隶属于spring.cache.redis节点下

```yaml
spring:
  redis:
    host: localhost
    port: 6379
  cache:
    type: redis
    redis:
      use-key-prefix: false
      key-prefix: sms_
      cache-null-values: false
      time-to-live: 10s
```

**总结**

1. springboot使用redis作为缓存实现需要导入redis的坐标
2. 修改设置，配置缓存供应商为redis，并提供对应的缓存配置



##### SpringBoot整合Memcached缓存

安装：windows版安装包下载地址：https://www.runoob.com/memcached/window-install-memcached.html

执行安装服务的命令即可，如下：

```CMD
memcached.exe -d install
```

服务安装完毕后可以使用命令启动和停止服务，如下：

```cmd
memcached.exe -d start		# 启动服务
memcached.exe -d stop		# 停止服务
```



在任务管理器中进行服务状态的切换

<img src="images/image-20220226175441675.png" alt="image-20220226175441675" style="zoom:67%;" />

**变更缓存为Memcached**

memcached目前提供有三种客户端技术，分别是Memcached Client for Java、SpyMemcached和Xmemcached，其中性能指标各方面最好的客户端是Xmemcached。

**步骤①**：导入xmemcached的坐标

```xml
<dependency>
    <groupId>com.googlecode.xmemcached</groupId>
    <artifactId>xmemcached</artifactId>
    <version>2.4.7</version>
</dependency>
```

**步骤②**：配置memcached，制作memcached的配置类

```java
@Configuration
public class XMemcachedConfig {
    @Bean
    public MemcachedClient getMemcachedClient() throws IOException {
        MemcachedClientBuilder memcachedClientBuilder = new XMemcachedClientBuilder("localhost:11211");
        MemcachedClient memcachedClient = memcachedClientBuilder.build();
        return memcachedClient;
    }
}
```

**步骤③**：使用xmemcached客户端操作缓存，注入MemcachedClient对象

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;
    @Autowired
    private MemcachedClient memcachedClient;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        try {
            memcachedClient.set(tele,10,code);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = null;
        try {
            code = memcachedClient.get(smsCode.getTele()).toString();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return smsCode.getCode().equals(code);
    }
}
```

**定义配置属性**

定义配置类，加载必要的配置属性，读取配置文件中memcached节点信息

```java
@Component
@ConfigurationProperties(prefix = "memcached")
@Data
public class XMemcachedProperties {
    private String servers;
    private int poolSize;
    private long opTimeout;
}
```

定义memcached节点信息

```yaml
memcached:
  servers: localhost:11211
  poolSize: 10
  opTimeout: 3000
```

在memcached配置类中加载信息

```java
@Configuration
public class XMemcachedConfig {
    @Autowired
    private XMemcachedProperties props;
    @Bean
    public MemcachedClient getMemcachedClient() throws IOException {
        MemcachedClientBuilder memcachedClientBuilder = new XMemcachedClientBuilder(props.getServers());
        memcachedClientBuilder.setConnectionPoolSize(props.getPoolSize());
        memcachedClientBuilder.setOpTimeout(props.getOpTimeout());
        MemcachedClient memcachedClient = memcachedClientBuilder.build();
        return memcachedClient;
    }
}
```

**总结**

1. memcached安装后需要启动对应服务才可以对外提供缓存功能，安装memcached服务需要基于windows系统管理员权限
2. 由于springboot没有提供对memcached的缓存整合方案，需要采用手工编码的形式创建xmemcached客户端操作缓存
3. 导入xmemcached坐标后，创建memcached配置类，注册MemcachedClient对应的bean，用于操作缓存
4. 初始化MemcachedClient对象所需要使用的属性可以通过自定义配置属性类的形式加载





##### SpringBoot整合jetcache缓存

jetcache严格意义上来说，并不是一个缓存解决方案，而是一个缓存框架，然后把别的缓存放到jetcache中管理，如此就可以支持AB缓存一起用。并且jetcache参考了springboot整合缓存的思想，整体技术使用方式和springboot的缓存解决方案思想非常类似。

目前jetcache支持的缓存方案本地缓存支持两种，远程缓存支持两种，分别如下：

- 本地缓存（Local）
  - LinkedHashMap
  - Caffeine
- 远程缓存（Remote）
  - Redis
  - Tair

###### 1、纯远程方案

**步骤①**：导入springboot整合jetcache对应的坐标starter，当前坐标默认使用的远程方案是redis

```xml
<dependency>
    <groupId>com.alicp.jetcache</groupId>
    <artifactId>jetcache-starter-redis</artifactId>
    <version>2.6.2</version>
</dependency>
```

**步骤②**：远程方案基本配置

```yaml
jetcache:
  remote:
    default:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
```

其中poolConfig是必配项，否则会报错

**步骤③**：启用缓存，在引导类上方标注注解@EnableCreateCacheAnnotation配置springboot程序中可以使用注解的形式创建缓存

```java
@SpringBootApplication
//jetcache启用缓存的主开关
@EnableCreateCacheAnnotation
public class Springboot20JetCacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot20JetCacheApplication.class, args);
    }
}
```

**步骤④**：创建缓存对象Cache，并使用注解@CreateCache标记当前缓存的信息，然后使用Cache对象的API操作缓存，put写缓存，get读缓存。

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;
    
    @CreateCache(name="jetCache_",expire = 10,timeUnit = TimeUnit.SECONDS)
    private Cache<String ,String> jetCache;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        jetCache.put(tele,code);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = jetCache.get(smsCode.getTele());
        return smsCode.getCode().equals(code);
    }
}
```

通过上述jetcache使用远程方案连接redis可以看出，jetcache操作缓存时的接口操作更符合开发者习惯，使用缓存就先获取缓存对象Cache，放数据进去就是put，取数据出来就是get，更加简单易懂。并且jetcache操作缓存时，可以为某个缓存对象设置过期时间，将同类型的数据放入缓存中，方便有效周期的管理。

上述方案中使用的是配置中定义的default（该key值可随意更改）缓存。例如再添加一种缓存解决方案，参照如下配置进行：

```yaml
jetcache:
  remote:
    default:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
    sms:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
```

如果想使用名称是sms的缓存，需要再创建缓存时指定参数area，声明使用对应缓存即可

```JAVA
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;
    
    @CreateCache(area="sms",name="jetCache_",expire = 10,timeUnit = TimeUnit.SECONDS)
    private Cache<String ,String> jetCache;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        jetCache.put(tele,code);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = jetCache.get(smsCode.getTele());
        return smsCode.getCode().equals(code);
    }
}
```



###### 2、纯本地方案

远程方案中，配置中使用remote表示远程，换成local就是本地，只不过类型不一样而已。

**步骤①**：导入springboot整合jetcache对应的坐标starter

```xml
<dependency>
    <groupId>com.alicp.jetcache</groupId>
    <artifactId>jetcache-starter-redis</artifactId>
    <version>2.6.2</version>
</dependency>
```

**步骤②**：本地缓存基本配置

```yaml
jetcache:
  local:
    default:
      type: linkedhashmap
      keyConvertor: fastjson
```

为了加速数据获取时key的匹配速度，jetcache要求指定key的类型转换器。简单说就是，如果你给了一个Object作为key的话，我先用key的类型转换器给转换成字符串，然后再保存。等到获取数据时，仍然是先使用给定的Object转换成字符串，然后根据字符串匹配。由于jetcache是阿里的技术，因此推荐key的类型转换器使用阿里的fastjson。

**步骤③**：启用缓存

```java
@SpringBootApplication
//jetcache启用缓存的主开关
@EnableCreateCacheAnnotation
public class Springboot20JetCacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot20JetCacheApplication.class, args);
    }
}
```

**步骤④**：创建缓存对象Cache时，标注当前使用本地缓存

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @CreateCache(name="jetCache_",expire = 1000,timeUnit = TimeUnit.SECONDS,cacheType = CacheType.LOCAL)
    private Cache<String ,String> jetCache;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        jetCache.put(tele,code);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = jetCache.get(smsCode.getTele());
        return smsCode.getCode().equals(code);
    }
}
```

cacheType控制当前缓存使用本地缓存还是远程缓存，配置cacheType=CacheType.LOCAL即使用本地缓存。



###### 		3、本地+远程方案

```YAML
jetcache:
  local:
    default:
      type: linkedhashmap
      keyConvertor: fastjson
  remote:
    default:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
    sms:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
```

在创建缓存的时候，配置cacheType为BOTH即则本地缓存与远程缓存同时使用。

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @CreateCache(name="jetCache_",expire = 1000,timeUnit = TimeUnit.SECONDS,cacheType = CacheType.BOTH)
    private Cache<String ,String> jetCache;
}
```

cacheType如果不进行配置，默认值是REMOTE，即仅使用远程缓存方案。关于jetcache的配置，参考以下信息

| 属性                                                      | 默认值 | 说明                                                         |
| --------------------------------------------------------- | ------ | ------------------------------------------------------------ |
| jetcache.statIntervalMinutes                              | 0      | 统计间隔，0表示不统计                                        |
| jetcache.hiddenPackages                                   | 无     | 自动生成name时，隐藏指定的包名前缀                           |
| jetcache.[local\|remote].${area}.type                     | 无     | 缓存类型，本地支持linkedhashmap、caffeine，远程支持redis、tair |
| jetcache.[local\|remote].${area}.keyConvertor             | 无     | key转换器，当前仅支持fastjson                                |
| jetcache.[local\|remote].${area}.valueEncoder             | java   | 仅remote类型的缓存需要指定，可选java和kryo                   |
| jetcache.[local\|remote].${area}.valueDecoder             | java   | 仅remote类型的缓存需要指定，可选java和kryo                   |
| jetcache.[local\|remote].${area}.limit                    | 100    | 仅local类型的缓存需要指定，缓存实例最大元素数                |
| jetcache.[local\|remote].${area}.expireAfterWriteInMillis | 无穷大 | 默认过期时间，毫秒单位                                       |
| jetcache.local.${area}.expireAfterAccessInMillis          | 0      | 仅local类型的缓存有效，毫秒单位，最大不活动间隔              |

以上方案仅支持手工控制缓存，但是springcache方案中的方法缓存特别好用，给一个方法添加一个注解，方法就会自动使用缓存。jetcache也提供了对应的功能，即方法缓存。

**方法缓存**

jetcache提供了方法缓存方案，只不过名称变更了而已。在对应的操作接口上方使用注解@Cached即可

**步骤①**：导入springboot整合jetcache对应的坐标starter

```xml
<dependency>
    <groupId>com.alicp.jetcache</groupId>
    <artifactId>jetcache-starter-redis</artifactId>
    <version>2.6.2</version>
</dependency>
```

**步骤②**：配置缓存

```yaml
jetcache:
  local:
    default:
      type: linkedhashmap
      keyConvertor: fastjson
  remote:
    default:
      type: redis
      host: localhost
      port: 6379
      keyConvertor: fastjson
      valueEncode: java
      valueDecode: java
      poolConfig:
        maxTotal: 50
    sms:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
```

由于redis缓存中不支持保存对象，因此需要对redis设置当Object类型数据进入到redis中时如何进行类型转换。需要配置keyConvertor表示key的类型转换方式，同时标注value的转换类型方式，值进入redis时是java类型，标注valueEncode为java，值从redis中读取时转换成java，标注valueDecode为java。

注意：为了实现Object类型的值进出redis，需要保障进出redis的Object类型的数据必须实现序列化接口。

```JAVA
@Data
public class Book implements Serializable {
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```

**步骤③**：启用缓存时开启方法缓存功能，并配置basePackages，说明在哪些包中开启方法缓存

```java
@SpringBootApplication
//jetcache启用缓存的主开关
@EnableCreateCacheAnnotation
//开启方法注解缓存
@EnableMethodCache(basePackages = "com.itheima")
public class Springboot20JetCacheApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot20JetCacheApplication.class, args);
    }
}
```

**步骤④**：使用注解@Cached标注当前方法使用缓存

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;
    
    @Override
    @Cached(name="book_",key="#id",expire = 3600,cacheType = CacheType.REMOTE)
    public Book getById(Integer id) {
        return bookDao.selectById(id);
    }
}
```



###### 4、远程方案的数据同步

由于远程方案中redis保存的数据可以被多个客户端共享，这就存在数据同步问题。jetcache提供3个注解解决此问题，分别在更新、删除操作时同步缓存数据，和读取缓存时定时刷新数据

更新缓存

```JAVA
@CacheUpdate(name="book_",key="#book.id",value="#book")
public boolean update(Book book) {
    return bookDao.updateById(book) > 0;
}
```

删除缓存

```JAVA
@CacheInvalidate(name="book_",key = "#id")
public boolean delete(Integer id) {
    return bookDao.deleteById(id) > 0;
}
```

定时刷新缓存

```JAVA
@Cached(name="book_",key="#id",expire = 3600,cacheType = CacheType.REMOTE)
@CacheRefresh(refresh = 5)
public Book getById(Integer id) {
    return bookDao.selectById(id);
}
```



###### 5、数据报表

jetcache还提供有简单的数据报表功能，帮助开发者快速查看缓存命中信息，只需要添加一个配置即可

```yaml
jetcache:
  statIntervalMinutes: 1
```

设置后，每1分钟在控制台输出缓存数据命中信息

```CMD
[DefaultExecutor] c.alicp.jetcache.support.StatInfoLogger  : jetcache stat from 2022-02-28 09:32:15,892 to 2022-02-28 09:33:00,003
cache    |    qps|   rate|   get|    hit|   fail|   expire|   avgLoadTime|   maxLoadTime
---------+-------+-------+------+-------+-------+---------+--------------+--------------
book_    |   0.66| 75.86%|    29|     22|      0|        0|          28.0|           188
---------+-------+-------+------+-------+-------+---------+--------------+--------------
```

**总结**

1. jetcache是一个类似于springcache的缓存解决方案，自身不具有缓存功能，它提供有本地缓存与远程缓存多级共同使用的缓存解决方案
2. jetcache提供的缓存解决方案受限于目前支持的方案，本地缓存支持两种，远程缓存支持两种
3. 注意数据进入远程缓存时的类型转换问题
4. jetcache提供方法缓存，并提供了对应的缓存更新与刷新功能
5. jetcache提供有简单的缓存信息命中报表方便开发者即时监控缓存数据命中情况





##### SpringBoot整合j2cache缓存

**步骤①**：导入j2cache、redis、ehcache坐标

```xml
<dependency>
    <groupId>net.oschina.j2cache</groupId>
    <artifactId>j2cache-core</artifactId>
    <version>2.8.4-release</version>
</dependency>
<dependency>
    <groupId>net.oschina.j2cache</groupId>
    <artifactId>j2cache-spring-boot2-starter</artifactId>
    <version>2.8.0-release</version>
</dependency>
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
</dependency>
```

j2cache的starter中默认包含redis坐标，官方推荐使用redis作为二级缓存，因此此处无需导入redis坐标

**步骤②**：配置一级与二级缓存，并配置一二级缓存间数据传递方式，配置书写在名称为j2cache.properties的文件中。

如果使用ehcache还需要单独添加ehcache的配置文件

```yaml
# 1级缓存
j2cache.L1.provider_class = ehcache
ehcache.configXml = ehcache.xml

# 2级缓存
j2cache.L2.provider_class = net.oschina.j2cache.cache.support.redis.SpringRedisProvider
j2cache.L2.config_section = redis
redis.hosts = localhost:6379

# 1级缓存中的数据如何到达二级缓存
j2cache.broadcast = net.oschina.j2cache.cache.support.redis.SpringRedisPubSubPolicy
```

此处配置不能乱配置，需要参照官方给出的配置说明进行。例如1级供应商选择ehcache，供应商名称仅仅是一个ehcache，但是2级供应商选择redis时要写专用的Spring整合Redis的供应商类名SpringRedisProvider，而且这个名称并不是所有的redis包中能提供的，也不是spring包中提供的。因此配置j2cache必须参照官方文档配置，而且还要去找专用的整合包，导入对应坐标才可以使用。

一级与二级缓存最重要的一个配置就是两者之间的数据沟通方式，此类配置也不是随意配置的，并且不同的缓存解决方案提供的数据沟通方式差异化很大，需要查询官方文档进行设置。

**步骤③**：使用缓存

```java
@Service
public class SMSCodeServiceImpl implements SMSCodeService {
    @Autowired
    private CodeUtils codeUtils;

    @Autowired
    private CacheChannel cacheChannel;

    public String sendCodeToSMS(String tele) {
        String code = codeUtils.generator(tele);
        cacheChannel.set("sms",tele,code);
        return code;
    }

    public boolean checkCode(SMSCode smsCode) {
        String code = cacheChannel.get("sms",smsCode.getTele()).asString();
        return smsCode.getCode().equals(code);
    }
}
```

j2cache的使用和jetcache比较类似，但是无需开启使用的开关，直接定义缓存对象即可使用，缓存对象名CacheChannel。

j2cache的使用不复杂，配置是j2cache的核心，毕竟是一个整合型的缓存框架。缓存相关的配置过多，可以查阅j2cache-core核心包中的j2cache.properties文件中的说明。如下：

```properties
#J2Cache configuration
#########################################
# Cache Broadcast Method
# values:
# jgroups -> use jgroups's multicast
# redis -> use redis publish/subscribe mechanism (using jedis)
# lettuce -> use redis publish/subscribe mechanism (using lettuce, Recommend)
# rabbitmq -> use RabbitMQ publisher/consumer mechanism
# rocketmq -> use RocketMQ publisher/consumer mechanism
# none -> don't notify the other nodes in cluster
# xx.xxxx.xxxx.Xxxxx your own cache broadcast policy classname that implement net.oschina.j2cache.cluster.ClusterPolicy
#########################################
j2cache.broadcast = redis

# jgroups properties
jgroups.channel.name = j2cache
jgroups.configXml = /network.xml

# RabbitMQ properties
rabbitmq.exchange = j2cache
rabbitmq.host = localhost
rabbitmq.port = 5672
rabbitmq.username = guest
rabbitmq.password = guest

# RocketMQ properties
rocketmq.name = j2cache
rocketmq.topic = j2cache
# use ; to split multi hosts
rocketmq.hosts = 127.0.0.1:9876

#########################################
# Level 1&2 provider
# values:
# none -> disable this level cache
# ehcache -> use ehcache2 as level 1 cache
# ehcache3 -> use ehcache3 as level 1 cache
# caffeine -> use caffeine as level 1 cache(only in memory)
# redis -> use redis as level 2 cache (using jedis)
# lettuce -> use redis as level 2 cache (using lettuce)
# readonly-redis -> use redis as level 2 cache ,but never write data to it. if use this provider, you must uncomment `j2cache.L2.config_section` to make the redis configurations available.
# memcached -> use memcached as level 2 cache (xmemcached),
# [classname] -> use custom provider
#########################################

j2cache.L1.provider_class = caffeine
j2cache.L2.provider_class = redis

# When L2 provider isn't `redis`, using `L2.config_section = redis` to read redis configurations
# j2cache.L2.config_section = redis

# Enable/Disable ttl in redis cache data (if disabled, the object in redis will never expire, default:true)
# NOTICE: redis hash mode (redis.storage = hash) do not support this feature)
j2cache.sync_ttl_to_redis = true

# Whether to cache null objects by default (default false)
j2cache.default_cache_null_object = true

#########################################
# Cache Serialization Provider
# values:
# fst -> using fast-serialization (recommend)
# kryo -> using kryo serialization
# json -> using fst's json serialization (testing)
# fastjson -> using fastjson serialization (embed non-static class not support)
# java -> java standard
# fse -> using fse serialization
# [classname implements Serializer]
#########################################

j2cache.serialization = json
#json.map.person = net.oschina.j2cache.demo.Person

#########################################
# Ehcache configuration
#########################################

# ehcache.configXml = /ehcache.xml

# ehcache3.configXml = /ehcache3.xml
# ehcache3.defaultHeapSize = 1000

#########################################
# Caffeine configuration
# caffeine.region.[name] = size, xxxx[s|m|h|d]
#
#########################################
caffeine.properties = /caffeine.properties

#########################################
# Redis connection configuration
#########################################

#########################################
# Redis Cluster Mode
#
# single -> single redis server
# sentinel -> master-slaves servers
# cluster -> cluster servers (数据库配置无效，使用 database = 0）
# sharded -> sharded servers  (密码、数据库必须在 hosts 中指定，且连接池配置无效 ; redis://user:password@127.0.0.1:6379/0）
#
#########################################

redis.mode = single

#redis storage mode (generic|hash)
redis.storage = generic

## redis pub/sub channel name
redis.channel = j2cache
## redis pub/sub server (using redis.hosts when empty)
redis.channel.host =

#cluster name just for sharded
redis.cluster_name = j2cache

## redis cache namespace optional, default[empty]
redis.namespace =

## redis command scan parameter count, default[1000]
#redis.scanCount = 1000

## connection
# Separate multiple redis nodes with commas, such as 192.168.0.10:6379,192.168.0.11:6379,192.168.0.12:6379

redis.hosts = 127.0.0.1:6379
redis.timeout = 2000
redis.password =
redis.database = 0
redis.ssl = false

## redis pool properties
redis.maxTotal = 100
redis.maxIdle = 10
redis.maxWaitMillis = 5000
redis.minEvictableIdleTimeMillis = 60000
redis.minIdle = 1
redis.numTestsPerEvictionRun = 10
redis.lifo = false
redis.softMinEvictableIdleTimeMillis = 10
redis.testOnBorrow = true
redis.testOnReturn = false
redis.testWhileIdle = true
redis.timeBetweenEvictionRunsMillis = 300000
redis.blockWhenExhausted = false
redis.jmxEnabled = false

#########################################
# Lettuce scheme
#
# redis -> single redis server
# rediss -> single redis server with ssl
# redis-sentinel -> redis sentinel
# redis-cluster -> cluster servers
#
#########################################

#########################################
# Lettuce Mode
#
# single -> single redis server
# sentinel -> master-slaves servers
# cluster -> cluster servers (数据库配置无效，使用 database = 0）
# sharded -> sharded servers  (密码、数据库必须在 hosts 中指定，且连接池配置无效 ; redis://user:password@127.0.0.1:6379/0）
#
#########################################

## redis command scan parameter count, default[1000]
#lettuce.scanCount = 1000
lettuce.mode = single
lettuce.namespace =
lettuce.storage = hash
lettuce.channel = j2cache
lettuce.scheme = redis
lettuce.hosts = 127.0.0.1:6379
lettuce.password =
lettuce.database = 0
lettuce.sentinelMasterId =
lettuce.maxTotal = 100
lettuce.maxIdle = 10
lettuce.minIdle = 10
# timeout in milliseconds
lettuce.timeout = 10000
# redis cluster topology refresh interval in milliseconds
lettuce.clusterTopologyRefresh = 3000

#########################################
# memcached server configurations
# refer to https://gitee.com/mirrors/XMemcached
#########################################

memcached.servers = 127.0.0.1:11211
memcached.username =
memcached.password =
memcached.connectionPoolSize = 10
memcached.connectTimeout = 1000
memcached.failureMode = false
memcached.healSessionInterval = 1000
memcached.maxQueuedNoReplyOperations = 100
memcached.opTimeout = 100
memcached.sanitizeKeys = false
```

**总结**

1. j2cache是一个缓存框架，自身不具有缓存功能，它提供多种缓存整合在一起使用的方案
2. j2cache需要通过复杂的配置设置各级缓存，以及缓存之间数据交换的方式
3. j2cache操作接口通过CacheChannel实现



#### 5-2.任务

##### Quartz

Quartz技术是一个比较成熟的定时任务框架，但配置略微复杂。springboot对其进行整合后，简化一系列的配置，将很多配置采用默认设置，如此开发阶段就简化了很多。

- 工作（Job）：用于定义具体执行的工作
- 工作明细（JobDetail）：用于描述定时工作相关的信息
- 触发器（Trigger）：描述了工作明细与调度器的对应关系
- 调度器（Scheduler）：用于描述触发工作的执行规则，通常使用cron表达式定义规则

下面开始springboot整合Quartz。

**步骤①**：导入springboot整合Quartz的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-quartz</artifactId>
</dependency>
```

**步骤②**：定义任务Bean，按照Quartz的开发规范制作，继承QuartzJobBean

```java
public class MyQuartz extends QuartzJobBean {
    @Override
    protected void executeInternal(JobExecutionContext context) throws JobExecutionException {
        System.out.println("quartz task run...");
    }
}
```

**步骤③**：创建Quartz配置类，定义工作明细（JobDetail）与触发器的（Trigger）bean

```java
@Configuration
public class QuartzConfig {
    @Bean
    public JobDetail printJobDetail(){
        //绑定具体的工作
        return JobBuilder.newJob(MyQuartz.class).storeDurably().build();
    }
    @Bean
    public Trigger printJobTrigger(){
        ScheduleBuilder schedBuilder = CronScheduleBuilder.cronSchedule("0/5 * * * * ?");
        //绑定对应的工作明细
        return TriggerBuilder.newTrigger().forJob(printJobDetail()).withSchedule(schedBuilder).build();
    }
}
```

触发器需要绑定任务，使用forJob()操作传入绑定的工作明细对象。此处可以为工作明细设置名称然后使用名称绑定，也可以直接调用对应方法绑定。触发器中最核心的规则是执行时间，此处使用调度器定义执行时间，执行时间描述方式使用的是cron表达式。

1. springboot整合Quartz就是将Quartz对应的核心对象交给spring容器管理，包含两个对象，JobDetail和Trigger对象
2. JobDetail对象描述的是工作的执行信息，需要绑定一个QuartzJobBean类型的对象
3. Trigger对象定义了一个触发器，需要为其指定绑定的JobDetail是哪个，同时要设置执行周期调度器



##### Task

spring根据定时任务的特征，将定时任务的开发简化到了极致。

**步骤①**：开启定时任务功能，在引导类上开启定时任务功能的开关，使用注解@EnableScheduling

```java
@SpringBootApplication
//开启定时任务功能
@EnableScheduling
public class Springboot22TaskApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot22TaskApplication.class, args);
    }
}
```

**步骤②**：定义Bean，在对应要定时执行的操作上方，使用注解@Scheduled定义执行的时间，执行时间的描述方式还是cron表达式

```java
@Component
public class MyBean {
    @Scheduled(cron = "0/1 * * * * ?")
    public void print(){
        System.out.println(Thread.currentThread().getName()+" :spring task run...");
    }
}
```

如何想对定时任务进行相关配置，可以通过配置文件进行

```yaml
spring:
  task:
   	scheduling:
      pool:
       	size: 1							# 任务调度线程池大小 默认 1
      thread-name-prefix: ssm_      	# 调度线程名称前缀 默认 scheduling-      
        shutdown:
          await-termination: false		# 线程池关闭时等待所有任务完成
          await-termination-period: 10s	# 调度线程关闭前最大等待时间，确保最后一定关闭
```

**总结**

1. spring task需要使用注解@EnableScheduling开启定时任务功能

2. 为定时执行的的任务设置执行周期，描述方式cron表达式

   

#### 5-3.邮件

邮件的3个概念：SMPT是发邮件的标准，POP3是收邮件的标准，IMAP是对POP3的升级。

- SMTP（Simple Mail Transfer Protocol）：简单邮件传输协议，用于**发送**电子邮件的传输协议
- POP3（Post Office Protocol - Version 3）：用于**接收**电子邮件的标准协议
- IMAP（Internet Mail Access Protocol）：互联网消息协议，是POP3的替代协议



springboot如何整合javamail发送邮件。

##### 发送简单邮件

**步骤①**：导入springboot整合javamail的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

**步骤②**：配置邮箱的登录信息

```yaml
spring:
  mail:
    host: smtp.126.com
    username: test@126.com
    password: test
```

java程序仅用于发送邮件，邮件的功能还是邮件供应商提供的，因此是用别人的邮件服务，要配置对应信息。

- host配置的是提供邮件服务的主机协议，当前程序仅用于发送邮件，因此配置的是smtp的协议。

- password并不是邮箱账号的登录密码，是邮件供应商提供的一个加密后的密码，也是为了保障系统安全性。

下例仅供参考：

![image-20220228111251036](images/image-20220228111251036.png)

**步骤③**：使用JavaMailSender接口发送邮件

```java
@Service
public class SendMailServiceImpl implements SendMailService {
    @Autowired
    private JavaMailSender javaMailSender;

    //发送人
    private String from = "test@qq.com";
    //接收人
    private String to = "test@126.com";
    //标题
    private String subject = "测试邮件";
    //正文
    private String context = "测试邮件正文内容";

    @Override
    public void sendMail() {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setFrom(from+"(小甜甜)");
        message.setTo(to);
        message.setSubject(subject);
        message.setText(context);
        javaMailSender.send(message);
    }
}
```

​		将发送邮件的必要信息（发件人、收件人、标题、正文）封装到SimpleMailMessage对象中，可以根据规则设置发送人昵称等。



##### 发送多组件邮件（附件、复杂正文）

**发送网页正文邮件**

```JAVA
@Service
public class SendMailServiceImpl2 implements SendMailService {
    @Autowired
    private JavaMailSender javaMailSender;

    //发送人
    private String from = "test@qq.com";
    //接收人
    private String to = "test@126.com";
    //标题
    private String subject = "测试邮件";
    //正文
    private String context = "<img src='ABC.JPG'/><a href='https://www.itcast.cn'>点开有惊喜</a>";

    public void sendMail() {
        try {
            MimeMessage message = javaMailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message);
            helper.setFrom(to+"(小甜甜)");
            helper.setTo(from);
            helper.setSubject(subject);
            helper.setText(context,true);		//此处设置正文支持html解析

            javaMailSender.send(message);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**发送带有附件的邮件**

```JAVA
@Service
public class SendMailServiceImpl2 implements SendMailService {
    @Autowired
    private JavaMailSender javaMailSender;

    //发送人
    private String from = "test@qq.com";
    //接收人
    private String to = "test@126.com";
    //标题
    private String subject = "测试邮件";
    //正文
    private String context = "测试邮件正文";

    public void sendMail() {
        try {
            MimeMessage message = javaMailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message,true);		//此处设置支持附件
            helper.setFrom(to+"(小甜甜)");
            helper.setTo(from);
            helper.setSubject(subject);
            helper.setText(context);

            //添加附件
            File f1 = new File("springboot_23_mail-0.0.1-SNAPSHOT.jar");
            File f2 = new File("resources\\logo.png");

            helper.addAttachment(f1.getName(),f1);
            helper.addAttachment("最靠谱的培训结构.png",f2);

            javaMailSender.send(message);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**总结**

1. springboot整合javamail其实就是简化了发送邮件的客户端对象JavaMailSender的初始化过程，通过配置的形式加载信息简化开发过程

   

#### 5-4.消息

##### 消息的概念

从广义角度来说，消息其实就是信息，但是和信息又有所不同。信息通常被定义为一组数据，而消息除了具有数据的特征之外，还有消息的来源与接收的概念。通常发送消息的一方称为消息的生产者，接收消息的一方称为消息的消费者。

生产者发送消息，可以理解为生产者发送了一个信息，也可以理解为生产者发送了一个命令；消费者接收消息，可以理解为消费者得到了一个信息，也可以理解为消费者得到了一个命令。

可以将消息划分成两种模式，同步消费与异步消息：

- 所谓同步消息就是生产者发送完消息，等待消费者处理，消费者处理完将结果告知生产者，然后生产者继续向下执行业务。

- 所谓异步消息就是生产者发送完消息，无需等待消费者处理完毕，生产者继续向下执行其他动作。


##### Java处理消息的标准规范

目前企业级开发中广泛使用的消息处理技术共三大类，具体如下：

- JMS
- AMQP
- MQTT

​		为什么是三大类，而不是三个技术呢？因为这些都是规范，就想JDBC技术，是个规范，开发针对规范开发，运行还要靠实现类，例如MySQL提供了JDBC的实现，最终运行靠的还是实现。并且这三类规范都是针对异步消息进行处理的，也符合消息的设计本质，处理异步的业务。对以上三种消息规范做一下普及



###### JMS

JMS（Java Message Service）,这是一个规范，作用等同于JDBC规范，提供了与消息服务相关的API接口。

JMS消息模型

JMS规范中规范了消息有两种模型。分别是**点对点模型**和**发布订阅模型**。

- **点对点模型**：peer-2-peer，生产者会将消息发送到一个保存消息的容器中，通常使用队列模型，使用队列保存消息。一个队列的消息只能被一个消费者消费，或未被及时消费导致超时。这种模型下，生产者和消费者是一对一绑定的。
- **发布订阅模型**：publish-subscribe，生产者将消息发送到一个保存消息的容器中，也是使用队列模型来保存。但是消息可以被多个消费者消费，生产者和消费者完全独立，相互不需要感知对方的存在。



**JMS消息种类**

​		根据消息中包含的数据种类划分，可以将消息划分成6种消息。

- TextMessage
- MapMessage
- BytesMessage
- StreamMessage
- ObjectMessage
- Message （只有消息头和属性）



JMS主张不同种类的消息，消费方式不同，可以根据使用需要选择不同种类的消息。但是整体上来说，JMS就是典型的保守派，什么都按照J2EE的规范来，做一套规范，定义若干个标准，每个标准下又提供一大批API。目前对JMS规范实现的消息中间件技术很多，例如ActiveMQ、Redis、HornetMQ。但是也有一些不太规范的实现，参考JMS的标准设计，但是又不完全满足其规范，例如：RabbitMQ、RocketMQ。



###### AMQP

AMQP（advanced message queuing protocol）：一种协议（高级消息队列协议，也是消息代理规范），规范了网络交换的数据格式，兼容JMS操作。具有跨平台性，服务器供应商，生产者，消费者可以使用不同的语言来实现

**JMS消息种类**

AMQP消息种类：byte[]

**AMQP消息模型**

- direct exchange
- fanout exchange
- topic exchange
- headers exchange
- system exchange

目前实现了AMQP协议的消息中间件技术也很多，而且都是较为流行的技术，例如：RabbitMQ、StormMQ、RocketMQ



###### MQTT

MQTT（Message Queueing Telemetry Transport）消息队列遥测传输，专为小设备设计，是物联网（IOT）生态系统中主要成分之一。



###### KafKa

Kafka，一种高吞吐量的分布式发布订阅消息系统，提供实时消息功能。Kafka技术并不是作为消息中间件为主要功能的产品，但是其拥有发布订阅的工作模式，也可以充当消息中间件来使用，而且目前企业级开发中其身影也不少见。



购物订单发送手机短信案例

为了便于下面演示各种各样的消息中间件技术，创建一个购物过程生成订单时为用户发送短信的案例环境，模拟使用消息中间件实现发送手机短信的过程。

手机验证码案例需求如下：

- 执行下单业务时（模拟此过程），调用消息服务，将要发送短信的订单id传递给消息中间件

- 消息处理服务接收到要发送的订单id后输出订单id（模拟发短信）


由于不涉及数据读写，仅开发业务层与表现层，其中短信处理的业务代码独立开发，代码如下：

**订单业务**

**业务层接口**

```JAVA
public interface OrderService {
    void order(String id);
}
```

模拟传入订单id，执行下订单业务，参数为虚拟设定，实际应为订单对应的实体类

**业务层实现**

```JAVA
@Service
public class OrderServiceImpl implements OrderService {
    @Autowired
    private MessageService messageService;
    
    @Override
    public void order(String id) {
        //一系列操作，包含各种服务调用，处理各种业务
        System.out.println("订单处理开始");
        //短信消息处理
        messageService.sendMessage(id);
        System.out.println("订单处理结束");
        System.out.println();
    }
}
```

业务层转调短信处理的服务MessageService

**表现层服务**

```JAVA
@RestController
@RequestMapping("/orders")
public class OrderController {

    @Autowired
    private OrderService orderService;

    @PostMapping("{id}")
    public void order(@PathVariable String id){
        orderService.order(id);
    }
}
```

表现层对外开发接口，传入订单id即可（模拟）

**短信处理业务**

**业务层接口**

```JAVA
public interface MessageService {
    void sendMessage(String id);
    String doMessage();
}
```

短信处理业务层接口提供两个操作，发送要处理的订单id到消息中间件，另一个操作目前暂且设计成处理消息，实际消息的处理过程不应该是手动执行，应该是自动执行，到具体实现时再进行设计

**业务层实现**

```JAVA
@Service
public class MessageServiceImpl implements MessageService {
    private ArrayList<String> msgList = new ArrayList<String>();

    @Override
    public void sendMessage(String id) {
        System.out.println("待发送短信的订单已纳入处理队列，id："+id);
        msgList.add(id);
    }

    @Override
    public String doMessage() {
        String id = msgList.remove(0);
        System.out.println("已完成短信发送业务，id："+id);
        return id;
    }
}
```

短信处理业务层实现中使用集合先模拟消息队列，观察效果

**表现层服务**

```JAVA
@RestController
@RequestMapping("/msgs")
public class MessageController {

    @Autowired
    private MessageService messageService;

    @GetMapping
    public String doMessage(){
        String id = messageService.doMessage();
        return id;
    }
}
```

短信处理表现层接口暂且开发出一个处理消息的入口，但是此业务是对应业务层中设计的模拟接口，实际业务不需要设计此接口。



##### SpringBoot整合ActiveMQ

安装：windows版安装包下载地址：[https://activemq.apache.org/components/classic/download](https://activemq.apache.org/components/classic/download/)[/](https://activemq.apache.org/components/classic/download/)

**启动服务器**

```CMD
activemq.bat
```

运行bin目录下的win32或win64目录下的activemq.bat命令即可，默认对外服务端口61616。

**访问web管理服务**

ActiveMQ启动后会启动一个Web控制台服务，可以通过该服务管理ActiveMQ。

```CMD
http://127.0.0.1:8161/
```

首先输入访问用户名和密码，初始化用户名和密码相同，均为：admin



**启动失败**

在ActiveMQ启动时要占用多个端口，Windows系统中终止端口运行的操作参看[【命令行启动常见问题及解决方案】](#命令行启动常见问题及解决方案)



SpringBoot整合ActiveMQ

**步骤①**：导入springboot整合ActiveMQ的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
```

**步骤②**：配置ActiveMQ的服务器地址

```yaml
spring:
  activemq:
    broker-url: tcp://localhost:61616
```

**步骤③**：使用JmsMessagingTemplate操作ActiveMQ

```java
@Service
public class MessageServiceActivemqImpl implements MessageService {
    @Autowired
    private JmsMessagingTemplate messagingTemplate;

    @Override
    public void sendMessage(String id) {
        System.out.println("待发送短信的订单已纳入处理队列，id："+id);
        messagingTemplate.convertAndSend("order.queue.id",id);
    }

    @Override
    public String doMessage() {
        String id = messagingTemplate.receiveAndConvert("order.queue.id",String.class);
        System.out.println("已完成短信发送业务，id："+id);
        return id;
    }
}
```

发送消息需要先将消息的类型转换成字符串，然后再发送，所以是convertAndSend，定义消息发送的位置，和具体的消息内容，此处使用id作为消息内容。

接收消息需要先将消息接收到，然后再转换成指定的数据类型，所以是receiveAndConvert，接收消息除了提供读取的位置，还要给出转换后的数据的具体类型。

**步骤④**：使用消息监听器在服务器启动后，监听指定位置，当消息出现后，立即消费消息

```JAVA
@Component
public class MessageListener {
    @JmsListener(destination = "order.queue.id")
    @SendTo("order.other.queue.id")
    public String receive(String id){
        System.out.println("已完成短信发送业务，id："+id);
        return "new:"+id;
    }
}
```

使用注解@JmsListener定义当前方法监听ActiveMQ中指定名称的消息队列。

如果当前消息队列处理完还需要继续向下传递当前消息到另一个队列中使用注解@SendTo即可，这样即可构造连续执行的顺序消息队列。

**步骤⑤**：切换消息模型由点对点模型到发布订阅模型，修改jms配置即可

```yaml
spring:
  activemq:
    broker-url: tcp://localhost:61616
  jms:
    pub-sub-domain: true
```

pub-sub-domain默认值为false，即点对点模型，修改为true后就是发布订阅模型。

**总结**

1. springboot整合ActiveMQ提供了JmsMessagingTemplate对象作为客户端操作消息队列
2. 操作ActiveMQ需要配置ActiveMQ服务器地址，默认端口61616
3. 企业开发时通常使用监听器来处理消息队列中的消息，设置监听器使用注解@JmsListener
4. 配置jms的pub-sub-domain属性可以在点对点模型和发布订阅模型间切换消息模型



##### SpringBoot整合RabbitMQ

RabbitMQ是MQ产品中的目前较为流行的产品之一，它遵从AMQP协议。RabbitMQ的底层实现语言使用的是Erlang，所以安装RabbitMQ需要先安装Erlang。

**Erlang安装**：windows版安装包下载地址：[https](https://www.erlang.org/downloads)[://www.erlang.org/downloads](https://www.erlang.org/downloads)



Erlang安装后需要配置环境变量，否则RabbitMQ将无法找到安装的Erlang。

需要配置项如下，作用等同JDK配置环境变量的作用。

- ERLANG_HOME
- PATH

安装RabbitMQ：windows版安装包下载地址：[https://](https://rabbitmq.com/install-windows.html)[rabbitmq.com/install-windows.html](https://rabbitmq.com/install-windows.html)

**启动服务器**

```CMD
rabbitmq-service.bat start		# 启动服务
rabbitmq-service.bat stop		# 停止服务
rabbitmqctl status				# 查看服务状态
```

运行sbin目录下的rabbitmq-service.bat命令即可，start参数表示启动，stop参数表示退出，默认对外服务端口5672。

注意：启动rabbitmq的过程实际上是开启rabbitmq对应的系统服务，需要管理员权限方可执行。



**访问web管理服务**

初始化用户名和密码相同，均为：guest

RabbitMQ也提供有web控制台服务，但是此功能是一个插件，需要先启用才可以使用。

```CMD
rabbitmq-plugins.bat list							# 查看当前所有插件的运行状态
rabbitmq-plugins.bat enable rabbitmq_management		# 启动rabbitmq_management插件
```

启动插件后可以在插件运行状态中查看是否运行，运行后通过浏览器即可打开服务后台管理界面

```CMD
http://localhost:15672
```



**整合(direct模型)**

RabbitMQ满足AMQP协议，因此不同的消息模型对应的制作不同，先使用最简单的direct模型开发。

**步骤①**：导入springboot整合amqp的starter，amqp协议默认实现为rabbitmq方案

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

**步骤②**：配置RabbitMQ的服务器地址

```yaml
spring:
  rabbitmq:
    host: localhost
    port: 5672
```

**步骤③**：初始化直连模式系统设置

由于RabbitMQ不同模型要使用不同的交换机，因此需要先初始化RabbitMQ相关的对象，例如队列，交换机等

```JAVA
@Configuration
public class RabbitConfigDirect {
    @Bean
    public Queue directQueue(){
        return new Queue("direct_queue");
    }
    @Bean
    public Queue directQueue2(){
        return new Queue("direct_queue2");
    }
    @Bean
    public DirectExchange directExchange(){
        return new DirectExchange("directExchange");
    }
    @Bean
    public Binding bindingDirect(){
        return BindingBuilder.bind(directQueue()).to(directExchange()).with("direct");
    }
    @Bean
    public Binding bindingDirect2(){
        return BindingBuilder.bind(directQueue2()).to(directExchange()).with("direct2");
    }
}
```

队列Queue与直连交换机DirectExchange创建后，还需绑定他们之间的关系Binding，这样就可以通过交换机操作对应队列。

**步骤④**：使用AmqpTemplate操作RabbitMQ

```java
@Service
public class MessageServiceRabbitmqDirectImpl implements MessageService {
    @Autowired
    private AmqpTemplate amqpTemplate;

    @Override
    public void sendMessage(String id) {
        System.out.println("待发送短信的订单已纳入处理队列（rabbitmq direct），id："+id);
        amqpTemplate.convertAndSend("directExchange","direct",id);
    }
}
```

amqp协议中的操作API接口名称看上去和jms规范的操作API接口很相似，但是传递参数差异很大。

**步骤⑤**：使用消息监听器在服务器启动后，监听指定位置，当消息出现后，立即消费消息

```JAVA
@Component
public class MessageListener {
    @RabbitListener(queues = "direct_queue")
    public void receive(String id){
        System.out.println("已完成短信发送业务(rabbitmq direct)，id："+id);
    }
}
```

使用注解@RabbitListener定义当前方法监听RabbitMQ中指定名称的消息队列。



**整合(topic模型)**

**步骤①**：同上

**步骤②**：同上

**步骤③**：初始化主题模式系统设置

```JAVA
@Configuration
public class RabbitConfigTopic {
    @Bean
    public Queue topicQueue(){
        return new Queue("topic_queue");
    }
    @Bean
    public Queue topicQueue2(){
        return new Queue("topic_queue2");
    }
    @Bean
    public TopicExchange topicExchange(){
        return new TopicExchange("topicExchange");
    }
    @Bean
    public Binding bindingTopic(){
        return BindingBuilder.bind(topicQueue()).to(topicExchange()).with("topic.*.id");
    }
    @Bean
    public Binding bindingTopic2(){
        return BindingBuilder.bind(topicQueue2()).to(topicExchange()).with("topic.orders.*");
    }
}
```

主题模式支持routingKey匹配模式，*表示匹配一个单词，#表示匹配任意内容，可以通过主题交换机将消息分发到不同的队列中，详细内容请参看RabbitMQ系列课程。	

| **匹配键**        | **topic.\*.\*** | **topic.#** |
| ----------------- | --------------- | ----------- |
| topic.order.id    | true            | true        |
| order.topic.id    | false           | false       |
| topic.sm.order.id | false           | true        |
| topic.sm.id       | false           | true        |
| topic.id.order    | true            | true        |
| topic.id          | false           | true        |
| topic.order       | false           | true        |

**步骤④**：使用AmqpTemplate操作RabbitMQ

```java
@Service
public class MessageServiceRabbitmqTopicImpl implements MessageService {
    @Autowired
    private AmqpTemplate amqpTemplate;

    @Override
    public void sendMessage(String id) {
        System.out.println("待发送短信的订单已纳入处理队列（rabbitmq topic），id："+id);
        amqpTemplate.convertAndSend("topicExchange","topic.orders.id",id);
    }
}
```

发送消息后，根据当前提供的routingKey与绑定交换机时设定的routingKey进行匹配，规则匹配成功消息才会进入到对应的队列中。

**步骤⑤**：使用消息监听器在服务器启动后，监听指定队列

```JAVA
@Component
public class MessageListener {
    @RabbitListener(queues = "topic_queue")
    public void receive(String id){
        System.out.println("已完成短信发送业务(rabbitmq topic 1)，id："+id);
    }
    @RabbitListener(queues = "topic_queue2")
    public void receive2(String id){
        System.out.println("已完成短信发送业务(rabbitmq topic 22222222)，id："+id);
    }
}
```

使用注解@RabbitListener定义当前方法监听RabbitMQ中指定名称的消息队列。

**总结**

1. springboot整合RabbitMQ提供了AmqpTemplate对象作为客户端操作消息队列
2. 操作ActiveMQ需要配置ActiveMQ服务器地址，默认端口5672
3. 企业开发时通常使用监听器来处理消息队列中的消息，设置监听器使用注解@RabbitListener
4. RabbitMQ有5种消息模型，使用的队列相同，但是交换机不同。交换机不同，对应的消息进入的策略也不同



##### SpringBoot整合RocketMQ

安装：windows版安装包下载地址：[https://rocketmq.apache.org](https://rocketmq.apache.org/)[/](https://rocketmq.apache.org/)

RocketMQ安装后需要配置环境变量，具体如下：

- ROCKETMQ_HOME
- PATH
- NAMESRV_ADDR （建议）： 127.0.0.1:9876

**RocketMQ工作模式**

在RocketMQ中，处理业务的服务器称为broker，生产者与消费者不是直接与broker联系的，而是通过命名服务器进行通信。broker启动后会通知命名服务器自己已经上线，这样命名服务器中就保存有所有的broker信息。当生产者与消费者需要连接broker时，通过命名服务器找到对应的处理业务的broker，因此命名服务器在整套结构中起到一个信息中心的作用。并且broker启动前必须保障命名服务器先启动。

<img src="images/image-20220228175123790.png" alt="image-20220228175123790" style="zoom:80%;" />

**启动服务器**

```CMD
mqnamesrv		# 启动命名服务器
mqbroker		# 启动broker
```

运行bin目录下的mqnamesrv命令即可启动命名服务器，默认对外服务端口9876。

运行bin目录下的mqbroker命令即可启动broker服务器，如果环境变量中没有设置NAMESRV_ADDR则需要在运行mqbroker指令前通过set指令设置NAMESRV_ADDR的值，并且每次开启均需要设置此项。

**测试服务器启动状态**

RocketMQ提供有一套测试服务器功能的测试程序，运行bin目录下的tools命令即可使用。

```CMD
tools org.apache.rocketmq.example.quickstart.Producer		# 生产消息
tools org.apache.rocketmq.example.quickstart.Consumer		# 消费消息
```

##### 整合（异步消息）

**步骤①**：导入springboot整合RocketMQ的starter，此坐标不由springboot维护版本

```xml
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-spring-boot-starter</artifactId>
    <version>2.2.1</version>
</dependency>
```

**步骤②**：配置RocketMQ的服务器地址

```yaml
rocketmq:
  name-server: localhost:9876
  producer:
    group: group_rocketmq
```

​		设置默认的生产者消费者所属组group。

**步骤③**：使用RocketMQTemplate操作RocketMQ

```java
@Service
public class MessageServiceRocketmqImpl implements MessageService {
    @Autowired
    private RocketMQTemplate rocketMQTemplate;

    @Override
    public void sendMessage(String id) {
        System.out.println("待发送短信的订单已纳入处理队列（rocketmq），id："+id);
        SendCallback callback = new SendCallback() {
            @Override
            public void onSuccess(SendResult sendResult) {
                System.out.println("消息发送成功");
            }
            @Override
            public void onException(Throwable e) {
                System.out.println("消息发送失败！！！！！");
            }
        };
        rocketMQTemplate.asyncSend("order_id",id,callback);
    }
}
```

使用asyncSend方法发送异步消息。

**步骤④**：使用消息监听器在服务器启动后，监听指定位置，当消息出现后，立即消费消息

```JAVA
@Component
@RocketMQMessageListener(topic = "order_id",consumerGroup = "group_rocketmq")
public class MessageListener implements RocketMQListener<String> {
    @Override
    public void onMessage(String id) {
        System.out.println("已完成短信发送业务(rocketmq)，id："+id);
    }
}
```

RocketMQ的监听器必须按照标准格式开发，实现RocketMQListener接口，泛型为消息类型。

使用注解@RocketMQMessageListener定义当前类监听RabbitMQ中指定组、指定名称的消息队列。

**总结**

1. springboot整合RocketMQ使用RocketMQTemplate对象作为客户端操作消息队列
2. 操作RocketMQ需要配置RocketMQ服务器地址，默认端口9876
3. 企业开发时通常使用监听器来处理消息队列中的消息，设置监听器使用注解@RocketMQMessageListener



##### SpringBoot整合Kafka

安装：windows版安装包下载地址：[https://](https://kafka.apache.org/downloads)[kafka.apache.org/downloads](https://kafka.apache.org/downloads)

> 建议使用windows版2.8.1版本。

**启动服务器**

kafka服务器的功能相当于RocketMQ中的broker，kafka运行还需要一个类似于命名服务器的服务。在kafka安装目录中自带一个类似于命名服务器的工具，叫做zookeeper，作用是注册中心。

```CMD
zookeeper-server-start.bat ..\..\config\zookeeper.properties		# 启动zookeeper
kafka-server-start.bat ..\..\config\server.properties				# 启动kafka
```

运行bin目录下的windows目录下的zookeeper-server-start命令即可启动注册中心，默认对外服务端口2181。

运行bin目录下的windows目录下的kafka-server-start命令即可启动kafka服务器，默认对外服务端口9092。

**创建主题**

和之前操作其他MQ产品相似，kakfa也是基于主题操作，操作之前需要先初始化topic。

```CMD
# 创建topic
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic itheima
# 查询topic
kafka-topics.bat --zookeeper 127.0.0.1:2181 --list					
# 删除topic
kafka-topics.bat --delete --zookeeper localhost:2181 --topic itheima
```

**测试服务器启动状态**

Kafka提供有一套测试服务器功能的测试程序，运行bin目录下的windows目录下的命令即可使用。

```CMD
kafka-console-producer.bat --broker-list localhost:9092 --topic itheima							# 测试生产消息
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic itheima --from-beginning	# 测试消息消费
```



**SpringBoot整合Kafka**

**步骤①**：导入springboot整合Kafka的starter，此坐标由springboot维护版本

```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

**步骤②**：配置Kafka的服务器地址

```yaml
spring:
  kafka:
    bootstrap-servers: localhost:9092
    consumer:
      group-id: order
```

设置默认的生产者消费者所属组id。

**步骤③**：使用KafkaTemplate操作Kafka

```java
@Service
public class MessageServiceKafkaImpl implements MessageService {
    @Autowired
    private KafkaTemplate<String,String> kafkaTemplate;

    @Override
    public void sendMessage(String id) {
        System.out.println("待发送短信的订单已纳入处理队列（kafka），id："+id);
        kafkaTemplate.send("itheima2022",id);
    }
}
```

使用send方法发送消息，需要传入topic名称。

**步骤④**：使用消息监听器在服务器启动后，监听指定位置，当消息出现后，立即消费消息

```JAVA
@Component
public class MessageListener {
    @KafkaListener(topics = "itheima2022")
    public void onMessage(ConsumerRecord<String,String> record){
        System.out.println("已完成短信发送业务(kafka)，id："+record.value());
    }
}
```

使用注解@KafkaListener定义当前方法监听Kafka中指定topic的消息，接收到的消息封装在对象ConsumerRecord中，获取数据从ConsumerRecord对象中获取即可。

**总结**

1. springboot整合Kafka使用KafkaTemplate对象作为客户端操作消息队列

2. 操作Kafka需要配置Kafka服务器地址，默认端口9092

3. 企业开发时通常使用监听器来处理消息队列中的消息，设置监听器使用注解@KafkaListener。接收消息保存在形参ConsumerRecord对象中

   

### 6、监控

什么是监控？就是通过软件的方式展示另一个软件的运行情况，运行的情况则通过各种各样的指标数据反馈给监控人员。例如网络是否顺畅、服务器是否在运行、程序的功能是否能够整百分百运行成功，内存是否够用等等。

#### 6-1.监控的意义

由于现在的互联网程序大部分都是基于微服务的程序，一个程序的运行需要若干个服务来保障，因此第一个要监控的指标就是服务是否正常运行，也就是**监控服务状态是否处理宕机状态**。一旦发现某个服务宕机了，必须马上给出对应的解决方案，避免整体应用功能受影响。

 其次，由于互联网程序服务的客户量是巨大的，当客户的请求在短时间内集中达到服务器后，就会出现各种程序运行指标的波动。比如内存占用严重，请求无法及时响应处理等，这就是第二个要监控的重要指标，**监控服务运行指标**。

虽然软件是对外提供用户的访问需求，完成对应功能的，但是后台的运行是否平稳，是否出现了不影响客户使用的功能隐患，这些也是要密切监控的，此时就需要在不停机的情况下，监控系统运行情况，日志是一个不错的手段。如果在众多日志中找到开发者或运维人员所关注的日志信息，简单快速有效的过滤出要看的日志也是监控系统需要考虑的问题，这就是第三个要监控的指标，**监控程序运行日志**。

虽然我们期望程序一直平稳运行，但是由于突发情况的出现，例如服务器被攻击、服务器内存溢出等情况造成了服务器宕机，此时当前服务不能满足使用需要，就要将其重启甚至关闭，如果快速控制服务器的启停也是程序运行过程中不可回避的问题，这就是第四个监控项，**管理服务状态**。



通过上述描述，可以看出监控很重要。那具体的监控要如何开展呢？

- 首先监控程序必须有一个单独的平台，将多个被监控的服务对应的监控指标信息汇总在一起，如此更利于监控工作的开展。

- 监控程序必须具有主动发起请求获取被监控服务信息的能力。

- 在被监控程序启动时上报监控程序，告诉监控程序可以监控我了。因此需要在被监控程序端做主动上报的操作，这就要求被监控程序中配置对应的监控程序是谁。
- 被监控程序可以提供各种各样的指标数据给监控程序看，但是每一个指标都代表着公司的机密信息，并不是所有的指标都可以给任何人看的，乃至运维人员，因此对被监控指标的是否开放出来给监控系统看，也需要做详细的设定。

以上描述的整个过程就是一个监控系统的基本流程。

**总结**

1. 监控是一个非常重要的工作，是保障程序正常运行的基础手段
2. 监控的过程通过一个监控程序进行，它汇总所有被监控的程序的信息集中统一展示
3. 被监控程序需要主动上报自己被监控，同时要设置哪些指标被监控



#### 6-2.可视化监控平台

springboot抽取了大部分监控系统的常用指标，提出了监控的总思想。然后就有好心的同志根据监控的总思想，制作了一个通用性很强的监控系统，因为是基于springboot监控的核心思想制作的，因此程序被命名为**Spring Boot Admin**。

Spring Boot Admin，这是一个开源社区项目，用于管理和监控SpringBoot应用程序。该项目中包含有客户端和服务端两部分，而监控平台指的就是服务端。某程序如果需要被监控，可将该程序制作成客户端，然后配置服务端地址后，服务端就可以通过HTTP请求的方式从客户端获取对应的信息，并通过UI界面展示对应信息。



**服务端开发**

**步骤①**：导入springboot admin对应的starter，版本与当前使用的springboot版本保持一致，并将其配置成web工程

```xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-server</artifactId>
    <version>2.5.4</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dpendency>
```

**步骤②**：在引导类上添加注解@EnableAdminServer，声明当前应用启动后作为SpringBootAdmin的服务器使用

```java
@SpringBootApplication
@EnableAdminServer
public class SpringbootAdminServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot25AdminServerApplication.class, args);
    }
}
```

访问地址：http://localhost:8080/applications



**客户端开发**

**步骤①**：导入springboot admin对应的starter，版本与当前使用的springboot版本保持一致，并将其配置成web工程

```xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-client</artifactId>
    <version>2.5.4</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**步骤②**：设置当前客户端将信息上传到哪个服务器上，通过yml文件配置

```yaml
spring:
  boot:
    admin:
      client:
        url: http://localhost:8080
```

客户端启动后，再次访问服务端程序即可。





由于当前没有设置开放哪些信息给监控服务器，所以目前看不到什么有效的信息。

下面需要做两组配置就可以看到信息：

1. 开放指定信息给服务器看

2. 允许服务器以HTTP请求的方式获取对应的信息


配置如下：

```yaml
server:
  port: 80
spring:
  boot:
    admin:
      client:
        url: http://localhost:8080
management:
  endpoint:
    health:
      show-details: always
  endpoints:
    web:
      exposure:
        include: "*"
```

通过配置可以开放所有的健康信息明细查看：

```yaml
management:
  endpoint:
    health:
      show-details: always
```

由于其他12种信息是默认不提供给服务器通过HTTP请求查阅的，需要开启查阅的内容项，使用*表示查阅全部。

```yaml
endpoints:
  web:
    exposure:
      include: "*" # 记得带引号
```

**配置多个客户端**

可以通过配置客户端的方式在其他的springboot程序中添加客户端坐标，如此当前服务器就可以监控多个客户端程序。每个客户端展示不同的监控信息。

进入监控面板，如果你加载的应用具有功能，在监控面板中可以看到3组信息展示的与之前加载的空工程不一样。

- 类加载面板中可以查阅到开发者自定义的类，如左图

​                        <img src="images/image-20220301161246835.png" alt="image-20220301161246835" style="zoom:33%;" /><img src="E:/StudyNode/myData/SpringBoot/images/image-20220301161949431.png" alt="image-20220301161949431" style="zoom:33%;" />

- 映射中可以查阅到当前应用配置的所有请求

​                        <img src="E:/StudyNode/myData/SpringBoot/images/image-20220301161418791.png" alt="image-20220301161418791" style="zoom: 33%;" /><img src="images/image-20220301162008737.png" alt="image-20220301162008737" style="zoom:33%;" />

- 性能指标中可以查阅当前应用独有的请求路径统计数据

​                        <img src="E:/StudyNode/myData/SpringBoot/images/image-20220301161906949.png" alt="image-20220301161906949" style="zoom: 33%;" /><img src="images/image-20220301162040670.png" alt="image-20220301162040670" style="zoom: 33%;" />

**总结**

1. 开发监控服务端需要导入坐标，然后在引导类上添加注解@EnableAdminServer，并将其配置成web程序即可
2. 开发被监控的客户端需要导入坐标，然后配置服务端服务器地址，并做开放指标的设定即可
3. 在监控平台中可以查阅到各种各样被监控的指标，前提是客户端开放了被监控的指标



#### 6-3.监控原理

通过查阅监控中的映射指标，可以看到当前系统中可以运行的所有请求路径，其中大部分路径以/actuator开头

<img src="E:\StudyNode\myData\SpringBoot\image\image-20220301170214076-1714535232054124.png" alt="image-20220301170214076" style="zoom: 50%;" />

这个路径代表什么含义呢？既然这个路径可以访问，就可以通过浏览器发送该请求看看究竟可以得到什么信息。

![image-20220301170723057](images/image-20220301170723057.png)

通过发送请求，可以得到一组json信息，如下

```json
{
    "_links": {
        "self": {
            "href": "http://localhost:81/actuator",
            "templated": false
        },
        "beans": {
            "href": "http://localhost:81/actuator/beans",
            "templated": false
        },
        "caches-cache": {
            "href": "http://localhost:81/actuator/caches/{cache}",
            "templated": true
        },
        "caches": {
            "href": "http://localhost:81/actuator/caches",
            "templated": false
        },
        "health": {
            "href": "http://localhost:81/actuator/health",
            "templated": false
        },
        "health-path": {
            "href": "http://localhost:81/actuator/health/{*path}",
            "templated": true
        },
        "info": {
            "href": "http://localhost:81/actuator/info",
            "templated": false
        },
        "conditions": {
            "href": "http://localhost:81/actuator/conditions",
            "templated": false
        },
        "shutdown": {
            "href": "http://localhost:81/actuator/shutdown",
            "templated": false
        },
        "configprops": {
            "href": "http://localhost:81/actuator/configprops",
            "templated": false
        },
        "configprops-prefix": {
            "href": "http://localhost:81/actuator/configprops/{prefix}",
            "templated": true
        },
        "env": {
            "href": "http://localhost:81/actuator/env",
            "templated": false
        },
        "env-toMatch": {
            "href": "http://localhost:81/actuator/env/{toMatch}",
            "templated": true
        },
        "loggers": {
            "href": "http://localhost:81/actuator/loggers",
            "templated": false
        },
        "loggers-name": {
            "href": "http://localhost:81/actuator/loggers/{name}",
            "templated": true
        },
        "heapdump": {
            "href": "http://localhost:81/actuator/heapdump",
            "templated": false
        },
        "threaddump": {
            "href": "http://localhost:81/actuator/threaddump",
            "templated": false
        },
        "metrics-requiredMetricName": {
            "href": "http://localhost:81/actuator/metrics/{requiredMetricName}",
            "templated": true
        },
        "metrics": {
            "href": "http://localhost:81/actuator/metrics",
            "templated": false
        },
        "scheduledtasks": {
            "href": "http://localhost:81/actuator/scheduledtasks",
            "templated": false
        },
        "mappings": {
            "href": "http://localhost:81/actuator/mappings",
            "templated": false
        }
    }
}
```

其中每一组数据都有一个请求路径，而在这里请求路径中有之前看到过的health，发送此请求又得到了一组信息

```JSON
{
    "status": "UP",
    "components": {
        "diskSpace": {
            "status": "UP",
            "details": {
                "total": 297042808832,
                "free": 72284409856,
                "threshold": 10485760,
                "exists": true
            }
        },
        "ping": {
            "status": "UP"
        }
    }
}
```

当前信息与监控面板中的数据存在着对应关系

<img src="images/image-20220301171025615.png" alt="image-20220301171025615" style="zoom:50%;" />

原来监控中显示的信息实际上是通过发送请求后得到json数据，然后展示出来。按照上述操作，可以发送更多的以/actuator开头的链接地址，获取更多的数据，这些数据汇总到一起组成了监控平台显示的所有数据。

整理上述信息可得到核心信息：监控平台中显示的信息实际上是通过对被监控的应用发送请求得到的。打开被监控应用的pom文件，其中导入了springboot admin的对应的client，在这个资源中导入了一个名称叫做actuator的包。被监控的应用之所以可以对外提供上述请求路径，就是因为添加了这个包。

![image-20220301171437817](images/image-20220301171437817.png)

Actuator，可以称为端点，描述了一组监控信息，SpringBootAdmin提供了多个内置端点，通过访问端点就可以获取对应的监控信息，也可以根据需要自定义端点信息。通过发送请求路劲**/actuator**可以访问应用所有端点信息，如果端点中还有明细信息可以发送请求**/actuator/端点名称**来获取详细信息。以下列出了所有端点信息说明：

| ID               | 描述                                                         | 默认启用 |
| ---------------- | ------------------------------------------------------------ | -------- |
| auditevents      | 暴露当前应用程序的审计事件信息。                             | 是       |
| beans            | 显示应用程序中所有 Spring bean 的完整列表。                  | 是       |
| caches           | 暴露可用的缓存。                                             | 是       |
| conditions       | 显示在配置和自动配置类上评估的条件以及它们匹配或不匹配的原因。 | 是       |
| configprops      | 显示所有 @ConfigurationProperties 的校对清单。               | 是       |
| env              | 暴露 Spring ConfigurableEnvironment 中的属性。               | 是       |
| flyway           | 显示已应用的 Flyway 数据库迁移。                             | 是       |
| health           | 显示应用程序健康信息                                         | 是       |
| httptrace        | 显示 HTTP 追踪信息（默认情况下，最后 100 个  HTTP 请求/响应交换）。 | 是       |
| info             | 显示应用程序信息。                                           | 是       |
| integrationgraph | 显示 Spring Integration 图。                                 | 是       |
| loggers          | 显示和修改应用程序中日志记录器的配置。                       | 是       |
| liquibase        | 显示已应用的 Liquibase 数据库迁移。                          | 是       |
| metrics          | 显示当前应用程序的指标度量信息。                             | 是       |
| mappings         | 显示所有 @RequestMapping 路径的整理清单。                    | 是       |
| scheduledtasks   | 显示应用程序中的调度任务。                                   | 是       |
| sessions         | 允许从 Spring Session 支持的会话存储中检索和删除用户会话。当使用 Spring Session 的响应式 Web 应用程序支持时不可用。 | 是       |
| shutdown         | 正常关闭应用程序。                                           | 否       |
| threaddump       | 执行线程 dump。                                              | 是       |
| heapdump         | 返回一个 hprof 堆 dump 文件。                                | 是       |
| jolokia          | 通过 HTTP 暴露 JMX bean（当  Jolokia 在 classpath 上时，不适用于 WebFlux）。 | 是       |
| logfile          | 返回日志文件的内容（如果已设置 logging.file 或 logging.path 属性）。支持使用 HTTP Range 头来检索部分日志文件的内容。 | 是       |
| prometheus       | 以可以由 Prometheus 服务器抓取的格式暴露指标。               | 是       |



上述端点每一项代表被监控的指标，如果对外开放则监控平台可以查询到对应的端点信息，如果未开放则无法查询对应的端点信息。通过配置可以设置端点是否对外开放功能。使用enable属性控制端点是否对外开放。其中health端点为默认端点，不能关闭。

```yaml
management:
  endpoint:
    health:						# 端点名称
      show-details: always
    info:						# 端点名称
      enabled: true				# 是否开放
```

为了方便开发者快速配置端点，springboot admin设置了13个较为常用的端点作为默认开放的端点，如果需要控制默认开放的端点的开放状态，可以通过配置设置，如下：

```YAML
management:
  endpoints:
    enabled-by-default: true	# 是否开启默认端点，默认值true
```

上述端点开启后，就可以通过端点对应的路径查看对应的信息。但是此时还不能通过HTTP请求查询此信息，还需要开启通过HTTP请求查询的端点名称，使用“*”可以简化配置成开放所有端点的WEB端HTTP请求权限。

```YAML
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

对端点的配置有两组信息：

- 一组是endpoints开头的，对所有端点进行配置
- 一组是endpoint开头的，对具体端点进行配置

```YAML
management:
  endpoint:		# 具体端点的配置
    health:
      show-details: always
    info:
      enabled: true
  endpoints:	# 全部端点的配置
    web:
      exposure:
        include: "*"
    enabled-by-default: true
```

**总结**

1. 被监控客户端通过添加actuator的坐标可以对外提供被访问的端点功能

2. 端点功能的开放与关闭可以通过配置进行控制

3. web端默认无法获取所有端点信息，通过配置开放端点功能

   

#### 6-4.自定义监控指标

端点描述了被监控的信息，除了系统默认的指标，还可以自行添加显示的指标，下面就通过3种不同的端点的指标自定义方式来学习端点信息的二次开发。

**INFO端点**

info端点描述了当前应用的基本信息，可以通过两种形式快速配置info端点的信息

- 配置形式

  在yml文件中通过设置info节点的信息就可以快速配置端点信息

  ```yaml
  info:
    appName: @project.artifactId@
    version: @project.version@
    company: myHikari
    author: myHikari
  ```

  配置完毕后，对应信息显示在监控平台上，也可以通过请求端点信息路径获取对应json信息。

- 编程形式

  通过配置的形式只能添加固定的数据，如果需要动态数据还可以通过配置bean的方式为info端点添加信息，此信息与配置信息共存

  ```JAVA
  @Component
  public class InfoConfig implements InfoContributor {
      @Override
      public void contribute(Info.Builder builder) {
          builder.withDetail("runTime",System.currentTimeMillis());		//添加单个信息
          Map infoMap = new HashMap();		
          infoMap.put("buildTime","2006");
          builder.withDetails(infoMap);									//添加一组信息
      }
  }
  ```

**Health端点**

health端点描述当前应用的运行健康指标，即应用的运行是否成功。通过编程的形式可以扩展指标信息。

```JAVA
@Component
public class HealthConfig extends AbstractHealthIndicator {
    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {
        boolean condition = true;
        if(condition) {
            builder.status(Status.UP);					//设置运行状态为启动状态
            builder.withDetail("runTime", System.currentTimeMillis());
            Map infoMap = new HashMap();
            infoMap.put("buildTime", "2006");
            builder.withDetails(infoMap);
        }else{
            builder.status(Status.OUT_OF_SERVICE);		//设置运行状态为不在服务状态
            builder.withDetail("上线了吗？","你做梦");
        }
    }
}
```

当任意一个组件状态不为UP时，整体应用对外服务状态为非UP状态。

**Metrics端点**

metrics端点描述了性能指标，除了系统自带的监控性能指标，还可以自定义性能指标。

```JAVA
@Service
public class BookServiceImpl extends ServiceImpl<BookDao, Book> implements IBookService {
    @Autowired
    private BookDao bookDao;

    private Counter counter;

    public BookServiceImpl(MeterRegistry meterRegistry){
        counter = meterRegistry.counter("用户付费操作次数：");
    }

    @Override
    public boolean delete(Integer id) {
        //每次执行删除业务等同于执行了付费业务
        counter.increment();
        return bookDao.deleteById(id) > 0;
    }
}
```

在性能指标中就出现了自定义的性能指标监控项

<img src="images/image-20220301175101812.png" alt="image-20220301175101812" style="zoom:50%;" />

**自定义端点**

可以根据业务需要自定义端点，方便业务监控

```JAVA
@Component
@Endpoint(id="pay",enableByDefault = true)
public class PayEndpoint {
    @ReadOperation
    public Object getPay(){
        Map payMap = new HashMap();
        payMap.put("level 1","300");
        payMap.put("level 2","291");
        payMap.put("level 3","666");
        return payMap;
    }
}
```

由于此端点数据spirng boot admin无法预知该如何展示，所以通过界面无法看到此数据，通过HTTP请求路径可以获取到当前端点的信息，但是需要先开启当前端点对外功能，或者设置当前端点为默认开发的端点。

<img src="images/image-20220301175355482.png" alt="image-20220301175355482" style="zoom:50%;" />

**总结**

1. 端点的指标可以自定义，但是每种不同的指标根据其功能不同，自定义方式不同
2. info端点通过配置和编程的方式都可以添加端点指标
3. health端点通过编程的方式添加端点指标，需要注意要为对应指标添加启动状态的逻辑设定
4. metrics指标通过在业务中添加监控操作设置指标
5. 可以自定义端点添加更多的指标



## SpringBoot原理篇

### bean的加载控制

企业级开发中不可能在spring容器中进行bean的饱和式加载的。什么是饱和式加载，就是不管用不用，全部加载。比如jdk中有两万个类，那就加载两万个bean，显然是不合理的，因为你压根就不会使用其中大部分的bean。

那合理的加载方式是什么？肯定是必要性加载，就是用什么加载什么。就是你要用什么技术，即加载对应技术的类，然后就加载对应的bean。因此在spring容器中，通过判定是否加载了某个类来控制某些bean的加载是一种常见操作。

下例给出对应的代码实现：先判断一个类的全路径名是否能够成功加载，加载成功说明有该类，那就干某项工作，否则就干别的工作。

```JAVA
public class MyImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        try {
            Class<?> clazz = Class.forName("com.goes.bean.Mouse");
            if(clazz != null) {
                return new String[]{"com.goes.bean.Cat"};
            }
        } catch (ClassNotFoundException e) {
		   // e.printStackTrace();
            return new String[0];
        }
        return null;
    }
}
```

通过上述的分析，可以看到此类操作将成为企业级开发中的常见操作，于是springboot将把这些常用操作给我们做了一次封装。

下例使用@ConditionalOnClass注解实现了当虚拟机中加载了com.itheima.bean.Wolf类时加载对应的bean。

```java
@Bean
@ConditionalOnClass(name = "com.goes.bean.Wolf")
public Cat tom(){
    return new Cat();
}
```

@ConditionalOnMissingClass注解控制虚拟机中没有加载指定的类才加载对应的bean。

```java
@Bean
@ConditionalOnMissingClass("com.goes.bean.Dog")
public Cat tom(){
    return new Cat();
}
```

多个条件的逻辑关系：多个条件都成立，才起作用

```java
@Bean
@ConditionalOnClass(name = "com.goes.bean.Wolf")
@ConditionalOnMissingClass("com.goes.bean.Mouse")
public Cat tom(){
    return new Cat();
}
```

除了判定是否加载类，还可以对当前容器类型做判定，下例是判定当前容器环境是否是web环境。

```java
@Bean
@ConditionalOnWebApplication
public Cat tom(){
    return new Cat();
}
```

下面是判定容器环境是否是非web环境。

```java
@Bean
@ConditionalOnNotWebApplication
public Cat tom(){
    return new Cat();
}
```

判定是否加载了指定名称的bean，这种有什么用呢？

比如当前容器中已经提供了jdbcTemplate对应的bean，你还需要再加载一个全新的jdbcTemplate的bean吗？

```java
@Bean
@ConditionalOnBean(name="jerry")
public Cat tom(){
    return new Cat();
}
```

判定当前是否加载了mysql的驱动类，如果加载了，则创建Druid的数据源对象。

```JAVA
public class SpringConfig {
    @Bean
    @ConditionalOnClass(name="com.mysql.jdbc.Driver")
    public DruidDataSource dataSource(){
        return new DruidDataSource();
    }
}
```

**总结**

1. springboot定义了若干种控制bean加载的条件设置注解，由spring固定加载bean变成了可以根据情况选择性的加载bean



### bean的依赖属性配置管理

bean在运行时，实现对应的业务逻辑时有可能需要开发者提供一些设置值，即属性。如果使用构造方法将参数固定，灵活性不足，此时就可以使用bean的属性配置相关的知识进行灵活的配置。先通过yml配置文件，设置bean运行需要使用的配置信息。

```yaml
cartoon:
  cat:
    name: "图多盖洛"
    age: 5
  mouse:
    name: "泰菲"
    age: 1
```

然后定义一个封装属性的专用类，加载配置属性，读取对应前缀相关的属性值。

```JAVA
@ConfigurationProperties(prefix = "cartoon")
@Data
public class CartoonProperties {
    private Cat cat;
    private Mouse mouse;
}
```

最后在使用的位置注入对应的配置即可。

```JAVA
@EnableConfigurationProperties(CartoonProperties.class)
public class CartoonCatAndMouse{
    @Autowired
    private CartoonProperties cartoonProperties;
}
```

在业务类上使用@EnableConfigurationProperties声明bean，如此在不使用该类时，就不会加载专用的属性配置类CartoonProperties，减少spring管控的资源数量。（建议）

**总结**

1. bean的运行如果需要外部设置值，建议将设置值封装成专用的属性类* * * * Properties
2. 设置属性类加载指定前缀的配置信息
3. 在需要使用属性类的位置通过注解@EnableConfigurationProperties加载bean，而不要直接在属性配置类上定义bean，减少资源加载的数量，因需加载而不要饱和式加载。



### 自动配置原理（工作流程）

整体过程分为2个阶段：

**阶段一：准备阶段**

1. springboot的开发人员先大量收集Spring开发者的编程习惯，整理开发过程每一个程序经常使用的技术列表，形成一个**技术集A**
2. 收集常用技术(**技术集A**)的使用参数，不管你用什么常用设置，我用什么常用设置，统统收集起来整理一下，得到开发过程中每一个技术的常用设置，形成每一个技术对应的**设置集B**

**阶段二：加载阶段**

1. springboot初始化Spring容器基础环境，读取用户的配置信息，加载用户自定义的bean和导入的其他坐标，形成**初始化环境**
2. springboot将**技术集A**包含的所有技术在SpringBoot启动时默认全部加载，这时肯定加载的东西有一些是无效的，没有用的
3. springboot会对**技术集A**中每一个技术约定出启动这个技术对应的条件，并设置成按条件加载，由于开发者导入了一些bean和其他坐标，也就是与**初始化环境**，这个时候就可以根据这个**初始化环境**与springboot的**技术集A**进行比对了，哪个匹配上加载哪个
4. 因为有些技术不做配置就无法工作，所以springboot开始对**设置集B**下手了。它统计出各个国家各个行业的开发者使用某个技术时最常用的设置是什么，然后把这些设置作为默认值直接设置好，并告诉开发者当前设置我已经给你搞了一套，你要用可以直接用，这样可以减少开发者配置参数的工作量
5. 但是默认配置不一定能解决问题，于是springboot开放修改**设置集B**的接口，可以由开发者根据需要决定是否覆盖默认配置



首先指定一个技术X，打算让技术X具备自动配置的功能，这个技术X可以是任意功能，这个技术隶属于上面描述的**技术集A**

```JAVA
public class CartoonCatAndMouse{
}
```

然后找出技术X使用过程中的常用配置Y，这个配置隶属于上面表述的**设置集B**

```yaml
cartoon:
  cat:
    name: "图多盖洛"
    age: 5
  mouse:
    name: "泰菲"
    age: 1
```

将常用配置Y设计出对应的yml配置书写格式，然后定义一个属性类封装对应的配置属性，该过程其实就是bean的依赖属性管理

```JAVA
@ConfigurationProperties(prefix = "cartoon")
@Data
public class CartoonProperties {
    private Cat cat;
    private Mouse mouse;
}
```

最后做一个配置类，当这个类加载的时候就可以初始化对应的功能bean，并且可以加载到对应的配置

```JAVA
@EnableConfigurationProperties(CartoonProperties.class)
public class CartoonCatAndMouse implements ApplicationContextAware {
    private CartoonProperties cartoonProperties;
}
```

当然也可以为当前自动配置类设置上激活条件，例如使用@CondtionOn* * * * 为其设置加载条件

```JAVA
@ConditionalOnClass(name="org.springframework.data.redis.core.RedisOperations")
@EnableConfigurationProperties(CartoonProperties.class)
public class CartoonCatAndMouse implements ApplicationContextAware {
    private CartoonProperties cartoonProperties;
}
```

如何让springboot启动的时候去加载这个类呢？springboot开放了一个配置入口，在配置目录中创建META-INF目录，并创建spring.factories文件，在其中添加设置，说明哪些类要启动自动配置就可以。

```CMD
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.itheima.bean.CartoonCatAndMouse
```



自动配置其实是一个小的生态，可以按照如下思想理解：

1. 自动配置从根本上来说就是一个bean的加载
2. 通过bean加载条件的控制给开发者一种感觉：自动配置是自适应的，可以根据情况自己判定，但实际上就是最普通的分支语句的应用
3. 使用bean的时候，如果不设置属性，就有默认值，如果不想用默认值，就可以自定义设置，也就是可以修改部分或者全部参数，也是一种自适应的形式，其实还是需要使用分支语句来做判断的
4. springboot技术提前将大量开发者有可能使用的技术提前做好，条件也写好，用的时候只需导入一个坐标，对应技术就可以使用，其实就是提前把spring.factories文件写好



**总结**

1. springboot启动时先加载spring.factories文件中的org.springframework.boot.autoconfigure.EnableAutoConfiguration配置项，将其中配置的所有的类都加载成bean
2. 在加载bean的时候，bean对应的类定义上都设置有加载条件，因此有可能加载成功，也可能条件检测失败不加载bean
3. 对于可以正常加载成bean的类，通常会通过@EnableConfigurationProperties注解初始化对应的配置属性类并加载对应的配置
4. 配置属性类上通常会通过@ConfigurationProperties加载指定前缀的配置（通常都有默认值）。如果没有默认值，就强制开发者必须配置后使用



### 变更自动配置

**方式一：通过yaml配置设置排除指定的自动配置类**

```YAML
spring:
  autoconfigure:
    exclude:
      - org.springframework.boot.autoconfigure.task.TaskExecutionAutoConfiguration
```

**方式二：通过注解参数排除自动配置类**

```JAVA
@EnableAutoConfiguration(excludeName = "",exclude = {})
```

**方式三：排除坐标（应用面较窄）**

如果当前自动配置中包含有更多的自动配置功能，此时可以通过检测条件的控制来管理自动配置是否启动。

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <!--web起步依赖环境中，排除Tomcat起步依赖，匹配自动配置条件-->
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <!--添加Jetty起步依赖，匹配自动配置条件-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

**总结**

springboot的自动配置并不是必然运行的，可以通过配置的形式干预是否启用对应的自动配置功能

# SpringBoot面试



## SpringBoot程序启动流程解析

SpringBoot初始化的参数根据参数的提供方，划分成如下3个大类，每个大类的参数又被封装各种各样的对象，具体如下：

- 环境属性（Environment）
- 系统配置（spring.factories）
- 参数（Arguments、application.properties）

SpringBoot程序启动流程的具体代码：

```java
Springboot30StartupApplication【10】->SpringApplication.run(Springboot30StartupApplication.class, args);
    SpringApplication【1332】->return run(new Class<?>[] { primarySource }, args);
        SpringApplication【1343】->return new SpringApplication(primarySources).run(args);
            SpringApplication【1343】->SpringApplication(primarySources)
            # 加载各种配置信息，初始化各种配置对象
                SpringApplication【266】->this(null, primarySources);
                    SpringApplication【280】->public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources)
                        SpringApplication【281】->this.resourceLoader = resourceLoader;
                        # 初始化资源加载器
                        SpringApplication【283】->this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
                        # 初始化配置类的类名信息（格式转换）
                        SpringApplication【284】->this.webApplicationType = WebApplicationType.deduceFromClasspath();
                        # 确认当前容器加载的类型
                        SpringApplication【285】->this.bootstrapRegistryInitializers = getBootstrapRegistryInitializersFromSpringFactories();
                        # 获取系统配置引导信息
                        SpringApplication【286】->setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
                        # 获取ApplicationContextInitializer.class对应的实例
                        SpringApplication【287】->setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
                        # 初始化监听器，对初始化过程及运行过程进行干预
                        SpringApplication【288】->this.mainApplicationClass = deduceMainApplicationClass();
                        # 初始化了引导类类名信息，备用
            SpringApplication【1343】->new SpringApplication(primarySources).run(args)
            # 初始化容器，得到ApplicationContext对象
                SpringApplication【323】->StopWatch stopWatch = new StopWatch();
                # 设置计时器
                SpringApplication【324】->stopWatch.start();
                # 计时开始
                SpringApplication【325】->DefaultBootstrapContext bootstrapContext = createBootstrapContext();
                # 系统引导信息对应的上下文对象
                SpringApplication【327】->configureHeadlessProperty();
                # 模拟输入输出信号，避免出现因缺少外设导致的信号传输失败，进而引发错误（模拟显示器，键盘，鼠标...）
                    java.awt.headless=true
                SpringApplication【328】->SpringApplicationRunListeners listeners = getRunListeners(args);
                # 获取当前注册的所有监听器
                SpringApplication【329】->listeners.starting(bootstrapContext, this.mainApplicationClass);
                # 监听器执行了对应的操作步骤
                SpringApplication【331】->ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
                # 获取参数
                SpringApplication【333】->ConfigurableEnvironment environment = prepareEnvironment(listeners, bootstrapContext, applicationArguments);
                # 将前期读取的数据加载成了一个环境对象，用来描述信息
                SpringApplication【333】->configureIgnoreBeanInfo(environment);
                # 做了一个配置，备用
                SpringApplication【334】->Banner printedBanner = printBanner(environment);
                # 初始化logo
                SpringApplication【335】->context = createApplicationContext();
                # 创建容器对象，根据前期配置的容器类型进行判定并创建
                SpringApplication【363】->context.setApplicationStartup(this.applicationStartup);
                # 设置启动模式
                SpringApplication【337】->prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
                # 对容器进行设置，参数来源于前期的设定
                SpringApplication【338】->refreshContext(context);
                # 刷新容器环境
                SpringApplication【339】->afterRefresh(context, applicationArguments);
                # 刷新完毕后做后处理
                SpringApplication【340】->stopWatch.stop();
                # 计时结束
                SpringApplication【341】->if (this.logStartupInfo) {
                # 判定是否记录启动时间的日志
                SpringApplication【342】->    new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), stopWatch);
                # 创建日志对应的对象，输出日志信息，包含启动时间
                SpringApplication【344】->listeners.started(context);
                # 监听器执行了对应的操作步骤
                SpringApplication【345】->callRunners(context, applicationArguments);
                # 调用运行器
                SpringApplication【353】->listeners.running(context);
                # 监听器执行了对应的操作步骤
```

如果想干预SpringBoot的启动过程，比如自定义数据库环境检测的程序，该如何将这个过程加入springboot的启动流程呢？遇到这样的问题，大部分技术是这样设计的，设计若干个标准接口，对应程序中的所有标准过程。当你想干预某个过程时，实现接口即可。例如Spring技术中bean的生命周期管理就是采用标准接口进行的。

```java
public class Abc implements InitializingBean, DisposableBean {
    public void destroy() throws Exception {
        //销毁操作
    }
    public void afterPropertiesSet() throws Exception {
        //初始化操作
    }
}
```

springboot启动过程由于存在着大量的过程阶段，如果设计接口就要设计十余个标准接口，这样对开发者不友好，同时整体过程管理分散，十余个过程各自为政，管理难度大，过程过于松散。那springboot如何解决这个问题呢？它采用一种最原始的设计模式来解决这个问题，这就是监听器模式，使用监听器来解决这个问题。

springboot将自身的启动过程比喻成一个大的事件，该事件是由若干个小的事件组成的。例如：

- org.springframework.boot.context.event.ApplicationStartingEvent
  - 应用启动事件，在应用运行但未进行任何处理时，将发送 ApplicationStartingEvent
- org.springframework.boot.context.event.ApplicationEnvironmentPreparedEvent
  - 环境准备事件，当Environment被使用，且上下文创建之前，将发送 ApplicationEnvironmentPreparedEvent
- org.springframework.boot.context.event.ApplicationContextInitializedEvent
  - 上下文初始化事件
- org.springframework.boot.context.event.ApplicationPreparedEvent
  - 应用准备事件，在开始刷新之前，bean定义被加载之后发送 ApplicationPreparedEvent
- org.springframework.context.event.ContextRefreshedEvent
  - 上下文刷新事件
- org.springframework.boot.context.event.ApplicationStartedEvent
  - 应用启动完成事件，在上下文刷新之后且所有的应用和命令行运行器被调用之前发送 ApplicationStartedEvent
- org.springframework.boot.context.event.ApplicationReadyEvent
  - 应用准备就绪事件，在应用程序和命令行运行器被调用之后，将发出 ApplicationReadyEvent，用于通知应用已经准备处理请求
- org.springframework.context.event.ContextClosedEvent（上下文关闭事件，对应容器关闭）

上述列出的仅仅是部分事件，当应用启动后走到某一个过程点时，监听器监听到某个事件触发，就会执行对应的事件。除了系统内置的事件处理，用户还可以根据需要自定义开发当前事件触发时要做的其他动作。

```JAVA
//设定监听器，在应用启动开始事件时进行功能追加
public class MyListener implements ApplicationListener<ApplicationStartingEvent> {
    public void onApplicationEvent(ApplicationStartingEvent event) {
		//自定义事件处理逻辑
    }
}
```

按照上述方案处理，用户就可以干预springboot启动过程的所有工作节点，设置自己的业务系统中独有的功能点。

**总结**

1. springboot启动流程是先初始化容器需要的各种配置，并加载成各种对象，初始化容器时读取这些对象，创建容器
2. 整体流程采用事件监听的机制进行过程控制，开发者可以根据需要自行扩展，添加对应的监听器绑定具体事件，就可以在事件触发位置执行开发者的业务代码







# 其他

## Actuator

SpringBoot-Actuator提供监控应用程序的机制，可帮助查看应用程序的运行和健康状况.

* 添加actuator依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

* 访问端点

actuator访问路径:http://localhost:8080/actuator

默认情况下,SpringBoot处于安全的考虑,自己开放部分的端点.

如果需要全部开放,配置如下信息

```properties
management.endpoints.web.exposure.include=*
```

* 常用的端点

  1. 查看应用程序加载的Bean:[/actuator/beans](http://localhost:8080/actuator/beans)
  2. 自动配置详解:[/actuator/conditions](http://localhost:8080/actuator/conditions)
  3. 查看配置属性:[/actuator/configprops](http://localhost:8080/actuator/configprops)
  4. 查看环境配置信息:[actuator/env](http://localhost:8080/actuator/env)
  5. 映射信息:[/actuator/mappings](http://localhost:8080/actuator/mappings)
  6. 系统性能度量指标:[/actuator/metrics](http://localhost:8080/actuator/metrics)
     - 检查某个具体指标的信息:[http://localhost:8080/actuator/metrics/XXX](http://localhost:8080/actuator/metrics/XXX)

  7. 追踪Web请求:[/actuator/httptrace](http://localhost:8080/actuator/httptrace)
  8. 线程快照:[/actuator/threaddump](http://localhost:8080/actuator/threaddump)
  9. 健康状况:[/actuator/health](http://localhost:8080/actuator/health)

  ```properties
  # 处于安全考虑,SpringBoot没有开放系统安全详细信息,配置如下信息
  management.endpoint.health.show-details=always
  ```

  10. 系统信息:[/actuator/info](http://localhost:8080/actuator/info)

  ```properties
  # 配置系统定义信息
  info.contactEmail=support@myreadinglist.com
  info.telephone=XXXXXXXX
  ```





## 自定义 - Starter

> 参考地址：[自定义starter](https://pdai.tech/md/spring/springboot/springboot-y-starter.html)

创建步骤：

1. 创建Maven项目 custom-demo-model，模拟一个需要被封装的CustomModel模块，其中核心方法为exeCustomMethod
2. 通过starter封装可以直接初始化CustomModule的实例到Spring容器
3. 在Maven中引入starter，且在yml中配置相应到参数即可直接初始化CustomModule的实例
4. 在应用中注入CustomModule即可使用其exeCustomMethod方法

### 1、新建自定义测试模块

> 创建Maven项目 custom-demo-model
>
> 项目目录结构如下：
>
> ![custom-demo-model](images/custom-demo-model.png)

#### pom.xml 文件如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.7.RELEASE</version>
        <relativePath/>
    </parent>

    <groupId>com.goes</groupId>
    <artifactId>custom-demo-model</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>custom-demo-model</name>
    <description>custom-demo-model</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <!-- 空依赖 -->
    </dependencies>

    <build>
    </build>
</project>
```

#### 创建自定义模块

```java
package com.goes.custom;

public class CustomModel {
    private String version;

    private String name;

    public String exeCustomMethod() {
        return "custom-module: {name = " + name + ", version = " + version + "}";
    }

    public String getVersion() {
        return version;
    }

    public void setVersion(String version) {
        this.version = version;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

#### Install

![maven-install](images/custom-demo-model-install.png)



### 2、封装自定义 - Starter

> 创建Springboot项目custom-springboot-starter
>
> 项目结构如下：
>
> ![项目结构](images/custom-springboot-starter.png)

#### pom.xml 文件如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.7.RELEASE</version>
        <relativePath/>
    </parent>

    <groupId>com.goes</groupId>
    <artifactId>custom-springboot-starter</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>custom-springboot-starter</name>
    <description>custom-springboot-starter</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>com.goes</groupId>
            <artifactId>custom-demo-model</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

#### Properties

```java
package com.goes.custom;

import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties(prefix = "com.goes")
public class CustomProperties {
    private String version;
    private String name;

    public String getVersion() {
        return version;
    }

    public void setVersion(String version) {
        this.version = version;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

#### AutoConfiguration

```java
package com.goes.custom;

import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableConfigurationProperties(CustomProperties.class)
public class CustomAutoConfiguration {

    @Bean
    public com.goes.custom.CustomModel demoModule(CustomProperties properties){
        com.goes.custom.CustomModel demoModule = new com.goes.custom.CustomModel();
        demoModule.setName(properties.getName());
        demoModule.setVersion(properties.getVersion());
        return demoModule;

    }
}
```

#### spring.factory

> 在META-INF下创建spring.factory文件

```bash
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.pdai.demospringbootstarter.DemoAutoConfiguration
```

#### Install

![install](images/custom-springboot-starter-install.png)



### 3、使用自定义 - Starter

> 创建Springboot项目custom-usage
>
> 项目结构如下：
>
> ![项目结构](images/custom-usage.png)

#### pom.xml文件如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.7.RELEASE</version>
        <relativePath/>
    </parent>
    <groupId>com.goes</groupId>
    <artifactId>custom-usage</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>custom-usage</name>
    <description>custom-usage</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>com.goes</groupId>
            <artifactId>custom-springboot-starter</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

#### applacation.yaml

```yml
spring:
  application:
    name: custom-usage

com:
  goes:
    name: Hikari
    version: 0.0.1-SNAPSHOT
```

#### Application

```java
package com.goes.custom;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@SpringBootApplication
public class CustomUsageApplication {

    public static void main(String[] args) {
        SpringApplication.run(CustomUsageApplication.class, args);
    }

    @Autowired
    private CustomModel customModel;


    @GetMapping("demo")
    public String demo(){
        return customModel.exeCustomMethod();
    }
}
```

#### 访问地址

> http://localhost:8080/demo