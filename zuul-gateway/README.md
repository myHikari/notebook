# Zuul

## 1、简单介绍

Zuul提供了动态路由、监控、弹性负载和安全功能。Zuul底层利用各种Filter实现如下功能：

- 认证和安全：识别每个需要认证的资源，拒绝不符合要求的请求。
- 性能监测：在服务边界追踪并统计数据，提供精确的生产视图。
- 动态路由：根据需要将请求动态路由到后端集群。
- 压力测试：逐渐增加对集群的流量以了解其性能。
- 负载卸载：预先为每种类型的请求分配容量，当请求超过容量时自动丢弃。
- 静态资源处理：直接在边界返回某些响应。



## 2、快速入门

#### 1、创建zuul-gateway的工程并引入依赖

```xml
<!--zuul的依赖-->
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-zuul</artifactId>
</dependency>

<!--eureka-client-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```



#### 2、创建应用主类

使用 @EnableZuulProxy 注解开启Zuul的API网关服务功能

```java
@SpringBootApplication
@EnableZuulProxy
public class ZuulGatewayApplication {

	public static void main(String[] args) {
		SpringApplication.run(ZuulGatewayApplication.class, args);
	}
}
```

在 application.yaml 配置文件中配置Zuul应用的基础信息

```yaml
server:
  port: 9010

spring:
  application:
    name: zuul-gateway

# 指定Eureka server的注册中心的位置，出来将Zuul的注册成服务之外，也让Zuul能够获取注册中心的实例清单
eureka:
  client:
    service-url:
      defaultZone: http://eureka-server:9001/eureka/
      
#Zuul实现的传统的路由配置
zuul:
  routes:
    hello-server-url:
      path: /hello-server/**
      url: http://localhost:9000

# Zuul面向服务的配置服务
zuul:
  routes:
    api-hello-server:
      path: /hello-server/**
      service-id: hello-server
    api-customer-server:
      path: /customer-server/**
      service-id: customer-server
```





# SpringCloudGateway

> Spring Cloud Gateway是Spring Cloud生态系统中的一个组件，它是一个基于Spring Framework 5，Spring Boot 2和Project Reactor的构建的非阻塞API网关，用于处理所有进入应用程序的HTTP请求。它基于过滤器链(Filter Chain)的概念，允许开发者以非常灵活的方式来处理HTTP请求，比如路由、过滤、限流、重试等。

## 1、简单介绍

网关的 **核心功能特性** ：

- 请求路由
- 权限控制
- 限流

架构图：

> **权限控制**：网关作为微服务入口，需要校验用户是是否有请求资格，如果没有则进行拦截。
>
> **路由和负载均衡**：一切请求都必须先经过gateway，但网关不处理业务，而是根据某种规则，把请求转发到某个微服务，这个过程叫做路由。当然路由的目标服务有多个时，还需要做负载均衡。
>
> **限流**：当请求流量过高时，在网关中按照下流的微服务能够接受的速度来放行请求，避免服务压力过大。

![image-20210714210131152](images/gateway-framework.png)

在SpringCloud中网关的实现包括两种：

- gateway
- zuul

Zuul是基于Servlet的实现，属于阻塞式编程。而SpringCloudGateway则是基于Spring5中提供的WebFlux，属于响应式编程的实现，具备更好的性能。



## 2、快速入门

> 网关搭建步骤：
>
> 1. 创建项目，引入nacos服务发现和gateway依赖
>
> 2. 配置application.yml，包括服务基本信息、nacos地址、路由
>
> 路由配置包括：
>
> 1. 路由id：路由的唯一标示
>
> 2. 路由目标（uri）：路由的目标地址，http代表固定地址，lb代表根据服务名负载均衡
>
> 3. 路由断言（predicates）：判断路由的规则，
>
> 4. 路由过滤器（filters）：对请求或响应做处理

### 引入依赖

```xml
<!--网关-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
<!--nacos服务发现依赖-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

### 编写启动类

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class GatewayApplication {

	public static void main(String[] args) {
		SpringApplication.run(GatewayApplication.class, args);
	}
}
```

### 编写基础配置和路由规则

创建application.yml文件，内容如下：

```yaml
server:
  port: 10010 # 网关端口
spring:
  application:
    name: gateway # 服务名称
  cloud:
    nacos:
      server-addr: localhost:8848 # nacos地址
    gateway:
      routes: # 网关路由配置
        - id: user-service # 路由id，自定义，只要唯一即可
          # uri: http://127.0.0.1:8081 	# 路由的目标地址 http就是固定地址
          uri: lb://userservice # 路由的目标地址 lb就是负载均衡，后面跟服务名称
          predicates: # 路由断言，也就是判断请求是否符合路由规则的条件
            - Path=/user/** # 这个是按照路径匹配，只要以/user/开头就符合要求
```

### 启动测试

> ### 网关路由的流程图
>
> 整个访问的流程如下：
>
> ![image-20210714211742956](images/gateway-liuCheng.png)
>
> 



## 3、断言工厂

在配置文件中写的断言规则（字符串）会被Predicate Factory读取并处理，转变为路由判断的条件

例如Path=/user/**是按照路径匹配

所有断言规则是由`org.springframework.cloud.gateway.handler.predicate.PathRoutePredicateFactory`类来处理

> ## SpringCloudGateway的断言工厂列表
>
> | **名称**   | **说明**                       | **示例**                                                     |
> | ---------- | ------------------------------ | ------------------------------------------------------------ |
> | After      | 是某个时间点后的请求           | -  After=2037-01-20T17:42:47.789-07:00[America/Denver]       |
> | Before     | 是某个时间点之前的请求         | -  Before=2031-04-13T15:14:47.433+08:00[Asia/Shanghai]       |
> | Between    | 是某两个时间点之前的请求       | -  Between=2037-01-20T17:42:47.789-07:00[America/Denver],  2037-01-21T17:42:47.789-07:00[America/Denver] |
> | Cookie     | 请求必须包含某些cookie         | - Cookie=chocolate, ch.p                                     |
> | Header     | 请求必须包含某些header         | - Header=X-Request-Id, \d+                                   |
> | Host       | 请求必须是访问某个host（域名） | -  Host=**.somehost.org,**.anotherhost.org                   |
> | Method     | 请求方式必须是指定方式         | - Method=GET,POST                                            |
> | Path       | 请求路径必须符合指定规则       | - Path=/red/{segment},/blue/**                               |
> | Query      | 请求参数必须包含指定参数       | - Query=name, Jack或者-  Query=name                          |
> | RemoteAddr | 请求者的ip必须是指定范围       | - RemoteAddr=192.168.1.1/24                                  |
> | Weight     | 权重处理                       |                                                              |



## 4、过滤器工厂

GatewayFilter是网关中提供的一种过滤器，可以对进入网关的请求和微服务返回的响应做处理：

![image-20210714212312871](images/spring-gateway-filterFacotry.png)



### 4.1、路由过滤器的种类

Spring提供了31种不同的路由过滤器工厂。例如：

| **名称**             | **说明**                     |
| -------------------- | ---------------------------- |
| AddRequestHeader     | 给当前请求添加一个请求头     |
| RemoveRequestHeader  | 移除请求中的一个请求头       |
| AddResponseHeader    | 给响应结果中添加一个响应头   |
| RemoveResponseHeader | 从响应结果中移除有一个响应头 |
| RequestRateLimiter   | 限制请求的流量               |



### 4.2、请求头过滤器

下面我们以AddRequestHeader 为例来讲解。

> **需求**：给所有进入userservice的请求添加一个请求头：Truth=itcast is freaking awesome!



只需要修改gateway服务的application.yml文件，添加路由过滤即可：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: user-service 
        uri: lb://userservice 
        predicates: 
        - Path=/user/** 
        filters: # 过滤器
        - AddRequestHeader=Truth, Itcast is freaking awesome! # 添加请求头
```

当前过滤器写在userservice路由下，因此仅仅对访问userservice的请求有效。





### 4.3、默认过滤器

如果要对所有的路由都生效，则可以将过滤器工厂写到default下。格式如下：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: user-service 
        uri: lb://userservice 
        predicates: 
        - Path=/user/**
      default-filters: # 默认过滤项
      - AddRequestHeader=Truth, Itcast is freaking awesome! 
```



### 4.4、总结

过滤器的作用是什么？

① 对路由的请求或响应做加工处理，比如添加请求头

② 配置在路由下的过滤器只对当前路由的请求生效

defaultFilters的作用是什么？

① 对所有路由都生效的过滤器



## 5、全局过滤器

### 5.1、全局过滤器作用

全局过滤器的作用也是处理一切进入网关的请求和微服务响应，与GatewayFilter的作用一样。区别在于GatewayFilter通过配置定义，处理逻辑是固定的；而GlobalFilter的逻辑需要自己写代码实现。

定义方式是实现GlobalFilter接口。

```java
public interface GlobalFilter {
    /**
     *  处理当前请求，有必要的话通过{@link GatewayFilterChain}将请求交给下一个过滤器处理
     *
     * @param exchange 请求上下文，里面可以获取Request、Response等信息
     * @param chain 用来把请求委托给下一个过滤器 
     * @return {@code Mono<Void>} 返回标示当前过滤器业务结束
     */
    Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain);
}
```



在filter中编写自定义逻辑，可以实现下列功能：

- 登录状态判断
- 权限校验
- 请求限流等





### 5.2、自定义全局过滤器

需求：定义全局过滤器，拦截请求，判断请求的参数是否满足下面条件：

- 参数中是否有authorization，

- authorization参数值是否为admin

如果同时满足则放行，否则拦截



实现

在gateway中定义一个过滤器：

```java
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.annotation.Order;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

@Order(-1)
@Component
public class AuthorizeFilter implements GlobalFilter {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // 1.获取请求参数
        MultiValueMap<String, String> params = exchange.getRequest().getQueryParams();
        // 2.获取authorization参数
        String auth = params.getFirst("authorization");
        // 3.校验
        if ("admin".equals(auth)) {
            // 放行
            return chain.filter(exchange);
        }
        // 4.拦截
        // 4.1.禁止访问，设置状态码
        exchange.getResponse().setStatusCode(HttpStatus.FORBIDDEN);
        // 4.2.结束处理
        return exchange.getResponse().setComplete();
    }
}
```





### 5.3、过滤器执行顺序

请求进入网关会碰到三类过滤器：当前路由的过滤器、DefaultFilter、GlobalFilter

请求路由后，会将当前路由过滤器和DefaultFilter、GlobalFilter，合并到一个过滤器链（集合）中，排序后依次执行每个过滤器：

![image-20210714214228409](E:\StudyNode\myData\images\spring-gateway-filterProcess)



排序的规则是什么呢？

- 每一个过滤器都必须指定一个int类型的order值，**order值越小，优先级越高，执行顺序越靠前**。
- GlobalFilter通过实现Ordered接口，或者添加@Order注解来指定order值，由我们自己指定
- 路由过滤器和defaultFilter的order由Spring指定，默认是按照声明顺序从1递增。
- 当过滤器的order值一样时，会按照 defaultFilter > 路由过滤器 > GlobalFilter的顺序执行。



详细内容，可以查看源码：

`org.springframework.cloud.gateway.route.RouteDefinitionRouteLocator#getFilters()`方法是先加载defaultFilters，然后再加载某个route的filters，然后合并。

`org.springframework.cloud.gateway.handler.FilteringWebHandler#handle()`方法会加载全局过滤器，与前面的过滤器合并后根据order排序，组织过滤器链



## 6、跨域问题

### 6.1、什么是跨域问题

![cors](images/gateway-cors.png)

跨域：域名不一致就是跨域，主要包括：

- 域名不同： www.taobao.com 和 www.taobao.org 和 www.jd.com 和 miaosha.jd.com

- 域名相同，端口不同：localhost:8080和localhost8081

跨域问题：浏览器禁止请求的发起者与服务端发生跨域ajax请求，请求被浏览器拦截的问题

解决方案：CORS => https://www.ruanyifeng.com/blog/2016/04/cors.html



### 6.2、解决跨域问题

在gateway服务的application.yml文件中，添加下面的配置：

```yaml
spring:
  cloud:
    gateway:
      # 。。。
      globalcors: # 全局的跨域处理
        add-to-simple-url-handler-mapping: true # 解决options请求被拦截问题
        corsConfigurations:
          '[/**]':
            allowedOrigins: # 允许哪些网站的跨域请求 
              - "http://localhost:8090"
            allowedMethods: # 允许的跨域ajax的请求方式
              - "GET"
              - "POST"
              - "DELETE"
              - "PUT"
              - "OPTIONS"
            allowedHeaders: "*" # 允许在请求中携带的头信息
            allowCredentials: true # 是否允许携带cookie
            maxAge: 360000 # 这次跨域检测的有效期
```


