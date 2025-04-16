# 业务

## 校验处理方式

```java
/**
 * JSR303
 *   1）、给Bean添加校验注解:javax.validation.constraints，并定义自己的message提示
 *   2)、开启校验功能@Valid
 *      效果：校验错误以后会有默认的响应；
 *   3）、给校验的bean后紧跟一个BindingResult，就可以获取到校验的结果
 *   4）、分组校验（多场景的复杂校验）
 *         1)、	@NotBlank(message = "品牌名必须提交",groups = {AddGroup.class,UpdateGroup.class})
 *          给校验注解标注什么情况需要进行校验
 *         2）、@Validated({AddGroup.class})
 *         3)、默认没有指定分组的校验注解@NotBlank，在分组校验情况@Validated({AddGroup.class})下不生效，只会在@Validated生效；
 *
 *   5）、自定义校验
 *      1）、编写一个自定义的校验注解
 *      2）、编写一个自定义的校验器 ConstraintValidator
 *      3）、关联自定义的校验器和自定义的校验注解
 *      @Documented
 * @Constraint(validatedBy = { ListValueConstraintValidator.class【可以指定多个不同的校验器，适配不同类型的校验】 })
 * @Target({ METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER, TYPE_USE })
 * @Retention(RUNTIME)
 * public @interface ListValue {
 *
 * 4、统一的异常处理
 * @ControllerAdvice
 *  1）、编写异常处理类，使用@ControllerAdvice。
 *  2）、使用@ExceptionHandler标注方法可以处理的异常。
 */
```

### 1、处理校验异常

#### 步骤1、实体类处理

> 通过对需要检验的字段添加注解
>
> @NotBlank
>
> @NotEmpty
>
> @NotNull
>
> @Null
>
> @Pattern
>
> ```java
> package com.atgoes.shopmall.product.entity;
> 
> /**
>  * 品牌
>  * 
>  * @author Go4014
>  * @email goes4014@gmail.com
>  * @date 2023-06-20 11:54:41
>  */
> @Data
> @TableName("pms_brand")
> public class BrandEntity implements Serializable {
> 	@NotBlank(message = "品牌名必须提交")
> 	private String name;
> 	/**
> 	 * 品牌logo地址
> 	 */
> 	@NotEmpty
> 	@URL(message = "Logo必须是合法的url地址")
> 	private String logo;
> 	/**
> 	 * 检索首字母
> 	 */
> 	@NotEmpty
> 	@Pattern(regexp = "/^[a-zA-Z]$/", message = "检索首字母必须是一个字母")
> 	private String firstLetter;
> 	/**
> 	 * 排序
> 	 */
> 	@NotNull
> 	@Min(value = 0, message = "排序字段必须是大于等于零的整数")
> 	private Integer sort;
> }
> ```
>
> 



#### 步骤2、编写校验异常处理类

枚举异常状态码

```java
package com.atgoes.common.exception;

public enum BizCodeEnume {
    UNKNOW_EXCEPTION(10000,"系统位置异常"),
    VALID_EXCEPTION(10001,"参数格式校验失败");

    private int code;
    private String msg;
    BizCodeEnume(int code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    public int getCode() {
        return this.code;
    }

    public String getMsg() {
        return this.msg;
    }
}
```

异常处理类（@RestControllerAdvice(basePackages = "com/atgoes/shopmall/product/controller")）

```java
package com.atgoes.shopmall.product.exception;

/**
 * 集中处理所有异常
 * */
@Slf4j
//@ResponseBody
//@ControllerAdvice(basePackages = "com/atgoes/shopmall/product/controller")
@RestControllerAdvice(basePackages = "com/atgoes/shopmall/product/controller")
public class ShopMallExceptionControllerAdvice {

    @ExceptionHandler(value = MethodArgumentNotValidException.class)
    public R handleValidException(MethodArgumentNotValidException e) {
        log.error("数据校验出现问题：{}, 异常类型：{}",e.getMessage(), e.getClass());

        Map<String, String> errorMap = new HashMap<>();
        BindingResult bindingResult = e.getBindingResult();
        bindingResult.getFieldErrors().forEach((fieldError -> {
            errorMap.put(fieldError.getField(), fieldError.getDefaultMessage());
        }));

        return R.error(BizCodeEnume.VALID_EXCEPTION.getCode(), BizCodeEnume.VALID_EXCEPTION.getMsg()).put("data", errorMap);
    }

    @ExceptionHandler(value = Throwable.class)
    public R handleException(Throwable throwable) {

        return R.error(BizCodeEnume.UNKNOW_EXCEPTION.getCode(), BizCodeEnume.UNKNOW_EXCEPTION.getMsg());
    }
}
```



#### 步骤3、网络接口添加异常处理注解参数（@Valid）

```java
@RestController
@RequestMapping("product/brand")
public class BrandController {
	/**
     * 保存
     */
    @RequestMapping("/save")
    //@RequiresPermissions("product:brand:save")
    public R save(@Valid @RequestBody BrandEntity brand /*, BindingResult result*/) {
        /*if (result.hasErrors()) {
            Map<String, String> map = new HashMap<>();
            // 1、获取校验的错误结果
            result.getFieldErrors().forEach((item) -> {
                // FieldError 获取到错误提示
                String message = item.getDefaultMessage();
                // 获取错误的属性名
                String field = item.getField();

            });
            return R.error(400, "提交的数据不合法").put("data", map);
        } else {
            brandService.save(brand);
        }*/

        brandService.save(brand);
        return R.ok();
    }
}

```



### 2、分组校验异常

#### 步骤1、实体类处理

> 编写分组接口
>
> 1、新增分组接口
>
> ```java
> package com.atgoes.common.valid;
> 
> public interface AddGroup {
> }
> ```
>
> 2、修改分组接口
>
> ```java
> package com.atgoes.common.valid;
> 
> public interface UpdateGroup {
> }
> ```
>
> 通过对需要检验的字段添加注解
>
> @NotBlank
>
> @NotEmpty
>
> @NotNull
>
> @Null
>
> @Pattern
>
> ```java
> package com.atgoes.shopmall.product.entity;
> 
> /**
>  * 品牌
>  * 
>  * @author Go4014
>  * @email goes4014@gmail.com
>  * @date 2023-06-20 11:54:41
>  */
> @Data
> @TableName("pms_brand")
> public class BrandEntity implements Serializable {
> 	private static final long serialVersionUID = 1L;
> 
> 	/**
> 	 * 品牌id
> 	 */
> 	@NotNull(message = "修改必须指定品牌id", groups = {UpdateGroup.class})
> 	@Null(message = "新增不能指定id", groups = {AddGroup.class})
> 	@TableId
> 	private Long brandId;
> 	/**
> 	 * 品牌名
> 	 */
> 	@NotBlank(message = "品牌名必须提交", groups = {AddGroup.class, UpdateGroup.class})
> 	private String name;
> 	/**
> 	 * 品牌logo地址
> 	 */
> 	@NotEmpty(groups = {AddGroup.class})
> 	@URL(message = "Logo必须是合法的url地址", groups = {AddGroup.class, UpdateGroup.class})
> 	private String logo;
> 	/**
> 	 * 检索首字母
> 	 */
> 	@NotEmpty(groups = {AddGroup.class})
> 	@Pattern(regexp = "/^[a-zA-Z]$/", message = "检索首字母必须是一个字母", groups = {AddGroup.class, UpdateGroup.class})
> 	private String firstLetter;
> 	/**
> 	 * 排序
> 	 */
> 	@NotNull(groups = {AddGroup.class})
> 	@Min(value = 0, message = "排序字段必须是大于等于零的整数", groups = {AddGroup.class, UpdateGroup.class})
> 	private Integer sort;
> }
> ```
>
> 



#### 步骤2、编写校验异常处理类

枚举异常状态码

```java
package com.atgoes.common.exception;

public enum BizCodeEnume {
    UNKNOW_EXCEPTION(10000,"系统位置异常"),
    VALID_EXCEPTION(10001,"参数格式校验失败");

    private int code;
    private String msg;
    BizCodeEnume(int code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    public int getCode() {
        return this.code;
    }

    public String getMsg() {
        return this.msg;
    }
}
```

异常处理类（@RestControllerAdvice(basePackages = "com/atgoes/shopmall/product/controller")）

```java
package com.atgoes.shopmall.product.exception;

/**
 * 集中处理所有异常
 * */
@Slf4j
//@ResponseBody
//@ControllerAdvice(basePackages = "com/atgoes/shopmall/product/controller")
@RestControllerAdvice(basePackages = "com/atgoes/shopmall/product/controller")
public class ShopMallExceptionControllerAdvice {

    @ExceptionHandler(value = MethodArgumentNotValidException.class)
    public R handleValidException(MethodArgumentNotValidException e) {
        log.error("数据校验出现问题：{}, 异常类型：{}",e.getMessage(), e.getClass());

        Map<String, String> errorMap = new HashMap<>();
        BindingResult bindingResult = e.getBindingResult();
        bindingResult.getFieldErrors().forEach((fieldError -> {
            errorMap.put(fieldError.getField(), fieldError.getDefaultMessage());
        }));

        return R.error(BizCodeEnume.VALID_EXCEPTION.getCode(), BizCodeEnume.VALID_EXCEPTION.getMsg()).put("data", errorMap);
    }

    @ExceptionHandler(value = Throwable.class)
    public R handleException(Throwable throwable) {

        return R.error(BizCodeEnume.UNKNOW_EXCEPTION.getCode(), BizCodeEnume.UNKNOW_EXCEPTION.getMsg());
    }
}
```



#### 步骤3、网络接口添加异常处理注解参数（@Validated）

```java
@RestController
@RequestMapping("product/brand")
public class BrandController {
	/**
     * 保存
     */
    @RequestMapping("/save")
    //@RequiresPermissions("product:brand:save")
    public R save(@Validated(AddGroup.class) @RequestBody BrandEntity brand) {
        /*if (result.hasErrors()) {
            Map<String, String> map = new HashMap<>();
            // 1、获取校验的错误结果
            result.getFieldErrors().forEach((item) -> {
                // FieldError 获取到错误提示
                String message = item.getDefaultMessage();
                // 获取错误的属性名
                String field = item.getField();

            });
            return R.error(400, "提交的数据不合法").put("data", map);
        } else {
            brandService.save(brand);
        }*/

        brandService.save(brand);
        return R.ok();
    }

    /**
     * 修改
     */
    @RequestMapping("/update")
    //@RequiresPermissions("product:brand:update")
    public R update(@Validated(UpdateGroup.class)@RequestBody BrandEntity brand) {
        brandService.updateById(brand);

        return R.ok();
    }
}

```



### 3、自定义校验异常

添加依赖

```xml
<dependency>
	<groupId>javax.validation</groupId>
	<artifactId>validation-api</artifactId>
	<version>2.0.1.Final</version>
</dependency>
```

#### 步骤1、编写自定义校验注解

```java
package com.atgoes.common.valid;

import javax.validation.Constraint;
import javax.validation.Payload;
import java.lang.annotation.*;

@Documented
@Constraint(
        validatedBy = { ListValueConstraintValidator.class }
)
@Target({ElementType.METHOD, ElementType.FIELD, ElementType.ANNOTATION_TYPE, ElementType.CONSTRUCTOR, ElementType.PARAMETER, ElementType.TYPE_USE})
@Retention(RetentionPolicy.RUNTIME)
public @interface ListValue {

    String message() default "{com.atgoes.common.valid.ListValue.message}";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};

    int[] vals() default {};
}
```



#### 步骤2、编写校验返回信息(src/main/resources/ValidationMessages.properties)

```properties
com.atgoes.common.valid.ListValue.message = 必须提交指定的值
```



#### 步骤3、编写校验规则

```java
package com.atgoes.common.valid;

import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;
import java.util.HashSet;
import java.util.Set;

public class ListValueConstraintValidator implements ConstraintValidator<ListValue, Integer> {
    private Set<Integer> set = new HashSet<>();

    //初始化方法
    @Override
    public void initialize(ListValue constraintAnnotation) {
        ConstraintValidator.super.initialize(constraintAnnotation);

        for (int val : constraintAnnotation.vals()) {
            set.add(val);
        }

    }

    /**
     * 判断是否校验成功
     * @param integer 需要校验的值
     * @param constraintValidatorContext
     * @return
     */
    //判断是否校验成功
    @Override
    public boolean isValid(Integer integer, ConstraintValidatorContext constraintValidatorContext) {
        return set.contains(integer);
    }
}
```



#### 步骤4、在实体类需校验字段添加注解

```java
package com.atgoes.shopmall.product.entity;
/**
 * 品牌
 * 
 * @author Go4014
 * @email goes4014@gmail.com
 * @date 2023-06-20 11:54:41
 */
@Data
@TableName("pms_brand")
public class BrandEntity implements Serializable {
	/**
	 * 显示状态[0-不显示；1-显示]
	 */
	@ListValue(vals = {0, 1}, groups = { AddGroup.class })
	private Integer showStatus;
}
```



## Nginx反向代理

1、编写域名解析文件：`C:\Windows\System32\drivers\etc\hosts` 添加项目域名地址 [域名解析文件](C:\Windows\System32\drivers\etc\hosts)

```sh
# shopMall (虚拟机地址 项目域名)
192.168.100.134 shopmall.com
```

2、在docker容器 `nginx` 配置文件夹：`/nginx/conf/conf.d` 下创建 `shopmall.conf`

```sh
server {
    listen       80;
    # server_name  localhost;
    server_name  shopmall.com;

    #charset koi8-r;
    #access_log  /var/log/nginx/log/host.access.log  main;

    location / {
    	# nginx代理给网关的时候会丢失请求的host信息
        proxy_set_header Host $host;
        # 要代理服务的地址
        proxy_pass http://shopmall;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

`注： /nginx/conf/conf.d 文件夹下的文件会追加到 nginx.conf 文件中`

3、在docker容器 `nginx` 配置文件夹：`/nginx/conf ` 下修改 `nginx.conf`

```sh
http{
	#...
	#...
	
	#gzip  on;
	upstream shopmall {
		# 192.168.0.103是主机的IP地址（类似主机的localhost）
    	server 192.168.0.103:88;
    }
}
```

4、修改网关配置文件

```yaml
spring:
  cloud:
    gateway:
      routes:
      	- id: shopmall_host_route
          uri: lb://shop-product 	# 负载均衡路由的模块
          predicates:
            - Host=**.shopmall.com, shopmall.com
```

`注：该配置要在最后添加，否则会使得往后的网关配置失效`

5、重新启动 `容器nginx` 以及 `网关服务`

```sh
docker restart nginx
```



### 前后端分离（静态页面）

1、在 `nginx` 容器下的 `/nginx/html/` 文件夹创建 `static` 目录存放静态页面 `/index/...` 

2、修改配置文件 `/nginx/conf/conf.d/xxx.conf`

```sh
server {
    # 添加静态数据的代理
    location /static/ {
        root /usr/share/nginx/html;
    }
}
```





## 认证服务

### 1、发送短信验证码

`详细查看产品文档即可`

#### 1.1、附属工具类

```java
public class HttpUtils {

    /**
     * get
     *
     * @param host
     * @param path
     * @param method
     * @param headers
     * @param querys
     * @return
     * @throws Exception
     */
    public static HttpResponse doGet(String host, String path, String method,
                                     Map<String, String> headers,
                                     Map<String, String> querys)
            throws Exception {
        HttpClient httpClient = wrapClient(host);

        HttpGet request = new HttpGet(buildUrl(host, path, querys));
        for (Map.Entry<String, String> e : headers.entrySet()) {
            request.addHeader(e.getKey(), e.getValue());
        }

        return httpClient.execute(request);
    }

    /**
     * post form
     *
     * @param host
     * @param path
     * @param method
     * @param headers
     * @param querys
     * @param bodys
     * @return
     * @throws Exception
     */
    public static HttpResponse doPost(String host, String path, String method,
                                      Map<String, String> headers,
                                      Map<String, String> querys,
                                      Map<String, String> bodys)
            throws Exception {
        HttpClient httpClient = wrapClient(host);

        HttpPost request = new HttpPost(buildUrl(host, path, querys));
        for (Map.Entry<String, String> e : headers.entrySet()) {
            request.addHeader(e.getKey(), e.getValue());
        }

        if (bodys != null) {
            List<NameValuePair> nameValuePairList = new ArrayList<NameValuePair>();

            for (String key : bodys.keySet()) {
                nameValuePairList.add(new BasicNameValuePair(key, bodys.get(key)));
            }
            UrlEncodedFormEntity formEntity = new UrlEncodedFormEntity(nameValuePairList, "utf-8");
            formEntity.setContentType("application/x-www-form-urlencoded; charset=UTF-8");
            request.setEntity(formEntity);
        }

        return httpClient.execute(request);
    }

    /**
     * Post String
     *
     * @param host
     * @param path
     * @param method
     * @param headers
     * @param querys
     * @param body
     * @return
     * @throws Exception
     */
    public static HttpResponse doPost(String host, String path, String method,
                                      Map<String, String> headers,
                                      Map<String, String> querys,
                                      String body)
            throws Exception {
        HttpClient httpClient = wrapClient(host);

        HttpPost request = new HttpPost(buildUrl(host, path, querys));
        for (Map.Entry<String, String> e : headers.entrySet()) {
            request.addHeader(e.getKey(), e.getValue());
        }

        if (StringUtils.isNotBlank(body)) {
            request.setEntity(new StringEntity(body, "utf-8"));
        }

        return httpClient.execute(request);
    }

    /**
     * Post stream
     *
     * @param host
     * @param path
     * @param method
     * @param headers
     * @param querys
     * @param body
     * @return
     * @throws Exception
     */
    public static HttpResponse doPost(String host, String path, String method,
                                      Map<String, String> headers,
                                      Map<String, String> querys,
                                      byte[] body)
            throws Exception {
        HttpClient httpClient = wrapClient(host);

        HttpPost request = new HttpPost(buildUrl(host, path, querys));
        for (Map.Entry<String, String> e : headers.entrySet()) {
            request.addHeader(e.getKey(), e.getValue());
        }

        if (body != null) {
            request.setEntity(new ByteArrayEntity(body));
        }

        return httpClient.execute(request);
    }

    /**
     * Put String
     * @param host
     * @param path
     * @param method
     * @param headers
     * @param querys
     * @param body
     * @return
     * @throws Exception
     */
    public static HttpResponse doPut(String host, String path, String method,
                                     Map<String, String> headers,
                                     Map<String, String> querys,
                                     String body)
            throws Exception {
        HttpClient httpClient = wrapClient(host);

        HttpPut request = new HttpPut(buildUrl(host, path, querys));
        for (Map.Entry<String, String> e : headers.entrySet()) {
            request.addHeader(e.getKey(), e.getValue());
        }

        if (StringUtils.isNotBlank(body)) {
            request.setEntity(new StringEntity(body, "utf-8"));
        }

        return httpClient.execute(request);
    }

    /**
     * Put stream
     * @param host
     * @param path
     * @param method
     * @param headers
     * @param querys
     * @param body
     * @return
     * @throws Exception
     */
    public static HttpResponse doPut(String host, String path, String method,
                                     Map<String, String> headers,
                                     Map<String, String> querys,
                                     byte[] body)
            throws Exception {
        HttpClient httpClient = wrapClient(host);

        HttpPut request = new HttpPut(buildUrl(host, path, querys));
        for (Map.Entry<String, String> e : headers.entrySet()) {
            request.addHeader(e.getKey(), e.getValue());
        }

        if (body != null) {
            request.setEntity(new ByteArrayEntity(body));
        }

        return httpClient.execute(request);
    }

    /**
     * Delete
     *
     * @param host
     * @param path
     * @param method
     * @param headers
     * @param querys
     * @return
     * @throws Exception
     */
    public static HttpResponse doDelete(String host, String path, String method,
                                        Map<String, String> headers,
                                        Map<String, String> querys)
            throws Exception {
        HttpClient httpClient = wrapClient(host);

        HttpDelete request = new HttpDelete(buildUrl(host, path, querys));
        for (Map.Entry<String, String> e : headers.entrySet()) {
            request.addHeader(e.getKey(), e.getValue());
        }

        return httpClient.execute(request);
    }

    private static String buildUrl(String host, String path, Map<String, String> querys) throws UnsupportedEncodingException {
        StringBuilder sbUrl = new StringBuilder();
        sbUrl.append(host);
        if (!StringUtils.isBlank(path)) {
            sbUrl.append(path);
        }
        if (null != querys) {
            StringBuilder sbQuery = new StringBuilder();
            for (Map.Entry<String, String> query : querys.entrySet()) {
                if (0 < sbQuery.length()) {
                    sbQuery.append("&");
                }
                if (StringUtils.isBlank(query.getKey()) && !StringUtils.isBlank(query.getValue())) {
                    sbQuery.append(query.getValue());
                }
                if (!StringUtils.isBlank(query.getKey())) {
                    sbQuery.append(query.getKey());
                    if (!StringUtils.isBlank(query.getValue())) {
                        sbQuery.append("=");
                        sbQuery.append(URLEncoder.encode(query.getValue(), "utf-8"));
                    }
                }
            }
            if (0 < sbQuery.length()) {
                sbUrl.append("?").append(sbQuery);
            }
        }

        return sbUrl.toString();
    }

    private static HttpClient wrapClient(String host) {
        HttpClient httpClient = new DefaultHttpClient();
        if (host.startsWith("https://")) {
            sslClient(httpClient);
        }

        return httpClient;
    }

    private static void sslClient(HttpClient httpClient) {
        try {
            SSLContext ctx = SSLContext.getInstance("TLS");
            X509TrustManager tm = new X509TrustManager() {
                public X509Certificate[] getAcceptedIssuers() {
                    return null;
                }
                public void checkClientTrusted(X509Certificate[] xcs, String str) {

                }
                public void checkServerTrusted(X509Certificate[] xcs, String str) {

                }
            };
            ctx.init(null, new TrustManager[] { tm }, null);
            SSLSocketFactory ssf = new SSLSocketFactory(ctx);
            ssf.setHostnameVerifier(SSLSocketFactory.ALLOW_ALL_HOSTNAME_VERIFIER);
            ClientConnectionManager ccm = httpClient.getConnectionManager();
            SchemeRegistry registry = ccm.getSchemeRegistry();
            registry.register(new Scheme("https", 443, ssf));
        } catch (KeyManagementException ex) {
            throw new RuntimeException(ex);
        } catch (NoSuchAlgorithmException ex) {
            throw new RuntimeException(ex);
        }
    }
}
```

#### 1.2、发送短信接口

```java
@Data
@Component
@ConfigurationProperties(prefix = "spring.cloud.alicloud.sms")
public class SmsComponent {

    private String host;
    private String path;
    private String method;
    private String appcode;

    public void sendSmsCode(String phone, String code) {
        String method = "GET";
        Map<String, String> headers = new HashMap<String, String>();
        //最后在header中的格式(中间是英文空格)为Authorization:APPCODE 83359fd73fe94948385f570e3c139105
        headers.put("Authorization", "APPCODE " + appcode);
        Map<String, String> querys = new HashMap<String, String>();
        querys.put("mobile", phone);
        querys.put("content", "【智能云】您的验证码是 " + code + "。如非本人操作，请忽略本短信");


        try {
            HttpResponse response = HttpUtils.doGet(host, path, method, headers, querys);
            System.out.println(response.toString());
            //
            System.out.println("responseBody" + EntityUtils.toString(response.getEntity()));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### 1.3、测试API

```java
/**
 * 测试短信发送
 */
@Test
public void testSms() {
    smsComponent.sendSmsCode("13030210745", "56789");
}
```



### 2、邮件发送验证码

#### 2.1、引入依赖

```xml
<!--发送邮件-->
<dependency>
    <groupId>javax.mail</groupId>
    <artifactId>javax.mail-api</artifactId>
    <version>1.6.2</version>
</dependency>
<dependency>
    <groupId>com.sun.mail</groupId>
    <artifactId>javax.mail</artifactId>
    <version>1.6.2</version>
</dependency>
```

#### 2.2、编写发送邮件API

```java
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.util.Properties;

@ConfigurationProperties(prefix = "javax.mail.mymail")
@Data
@Component
public class EmailSender {

    /**
     * 发送邮件
     * @param toEmail 收件人邮箱地址
     * @param subject 邮件标题
     * @param content 邮件内容
     * @param fromEmail 发件人邮箱地址
     * @param password 发件人邮箱密码
     */
    public void sendEmail(String toEmail, String subject, String content, String fromEmail, String password) {
        // 配置SMTP服务器信息
        Properties props = new Properties();
        props.put("mail.smtp.auth", "true");
        props.put("mail.smtp.starttls.enable", "true");
        // SMTP服务器地址
        props.put("mail.smtp.host", "smtp.outlook.com");
        // SMTP服务器端口号
        props.put("mail.smtp.port", "587");

        // 创建会话对象
        Session session = Session.getInstance(props, new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication(fromEmail, password);
            }
        });

        try {
            // 创建邮件消息对象
            Message message = new MimeMessage(session);
            message.setFrom(new InternetAddress(fromEmail));
            message.setRecipients(Message.RecipientType.TO, InternetAddress.parse(toEmail));
            message.setSubject(subject);
            message.setText(content);

            // 发送邮件
            Transport.send(message);
            System.out.println("邮件发送成功！");
        } catch (MessagingException e) {
            e.printStackTrace();
            System.err.println("邮件发送失败：" + e.getMessage());
        }
    }
}
```

#### 2.3、测试API

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class ShopThirdPartyApplicationTests {
    @Autowired
    EmailSender emailSender;

    @Test
    public void testSendEmail() {
        String toEmail = "example@gmail.com";
        String subject = "Send Email";
        String content = "Hello World！";

        String fromEmail = "example@outlook.com";
        String password = "password";

        emailSender.sendEmail(toEmail, subject, content, fromEmail, password);
    }
}
```



### 3、MD5盐值加密

#### BCryptPasswordEncoder

```java
// 加密
BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
String encode = passwordEncoder.encode(password);
// 解析匹配
boolean matches = passwordEncoder.matches(password, encode);
```



### 4、社交登录





### 5、单点登录（统一认证）





## Feign远程调用

### 请求头数据丢失问题

```java
@Configuration
public class ShopFeignConfig {

    @Bean("requestInterceptor")
    public RequestInterceptor requestInterceptor() {
        return new RequestInterceptor() {
            @Override
            public void apply(RequestTemplate template) {
                //1、RequestContextHolder拿到刚进来的这个请求
                ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
                if (attributes != null) {
                    System.out.println("RequestInterceptor线程...." + Thread.currentThread().getId());
                    HttpServletRequest request = attributes.getRequest(); //老请求
                    if (request != null) {
                        //同步请求头数据，Cookie
                        String cookie = request.getHeader("Cookie");
                        //给新请求同步老请求的cookie
                        template.header("Cookie", cookie);
                    }
                }
            }
        };
    }
}
```



## 幂等性

### 何为幂等性?

> 接口幂等性就是用户对于同一操作发起的一次请求或者多次请求的结果是一致的，不会因为多次点击而产生副作用

比如：支付场景，用户购买了商品支付扣款成功，但是返回结果的时候网络异常，此时钱已经扣了，用户再次点击按钮，此时会进行第二次扣款，返回结果成功，用户查询余额返发现多扣钱了，流水记录也变成了两条，这就没有保证接口的幂等性。



### 解决方案！

#### token 机制 

1、服务端提供了发送 token 的接口。我们在分析业务的时候，哪些业务是存在幂等问题的，就必须在执行业务前，先去获取 token，服务器会把 token 保存到 redis 中。 

2、然后调用业务接口请求时，把 token 携带过去，一般放在请求头部。

3、服务器判断 token 是否存在 redis 中，存在表示第一次请求，然后删除token,继续执行业务。

4、如果判断 token 不存在 redis 中，就表示是重复操作，直接返回重复标记给client，这样就保证了业务代码，不被重复执行。 



#### 危险性： 

##### 1、先删除 token 还是后删除 token； 

(1) 先删除可能导致，业务确实没有执行，重试还带上之前 token，由于防重设计导致，请求还是不能执行

(2) 后删除可能导致，业务处理成功，但是服务闪断，出现超时，没有删除token，别人继续重试，导致业务被执行两边

(3) 我们最好设计为先删除 token，如果业务调用失败，就重新获取token 再次请求。



##### 2、Token 获取、比较和删除必须是原子性 

(1) redis.get(token) 、token.equals、redis.del(token)如果这两个操作不是原子，可能导致，高并发下，都 get 到同样的数据，判断都成功，继续业务并发执行

(2) 可以在 redis 使用 lua 脚本完成这个操作 

if redis.call('get', KEYS[1]) == ARGV[1] 

then return redis.call('del', KEYS[1]) 

else return 0 end 



#### 各种锁机制 

##### 1、数据库悲观锁 

select * from xxxx where id = 1 for update; 悲观锁使用时一般伴随事务一起使用，数据锁定时间可能会很长，需要根据实际情况选用。另外要注意的是，id 字段一定是主键或者唯一索引，不然可能造成锁表的结果，处理起来会非常麻烦。 

##### 2、数据库乐观锁 

这种方法适合在更新的场景中， update t_goods set count = count -1 , version = version + 1 where good_id=2 and version = 1 根据 version 版本，也就是在操作库存前先获取当前商品的 version 版本号，然后操作的时候带上此 version 号。我们梳理下，我们第一次操作库存时，得到 version 为1，调用库存服务version 变成了 2；但返回给订单服务出现了问题，订单服务又一次发起调用库存服务，当订单服务传如的 version 还是 1，再执行上面的 sql 语句时，就不会执行；因为version 已经变为 2 了，where 条件就不成立。这样就保证了不管调用几次，只会真正的处理一次。乐观锁主要使用于处理读多写少的问题 

##### 3、业务层分布式锁 

如果多个机器可能在同一时间同时处理相同的数据，比如多台机器定时任务都拿到了相同数据处理，我们就可以加分布式锁，锁定此数据，处理完成后释放锁。获取到锁的必须先判断这个数据是否被处理过。 



#### 各种唯一约束

##### 1、数据库唯一约束 

插入数据，应该按照唯一索引进行插入，比如订单号，相同的订单就不可能有两条记录插入。我们在数据库层面防止重复。 这个机制是利用了数据库的主键唯一约束的特性，解决了在 insert 场景时幂等问题。但主键的要求不是自增的主键，这样就需要业务生成全局唯一的主键。 如果是分库分表场景下，路由规则要保证相同请求下，落地在同一个数据库和同一表中，要不然数据库主键约束就不起效果了，因为是不同的数据库和表主键不相关。

##### 2、redis set 防重

很多数据需要处理，只能被处理一次，比如我们可以计算数据的 MD5 将其放入redis 的set，每次处理数据，先看这个 MD5 是否已经存在，存在就不处理。 

##### 4、防重表

使用订单号 orderNo 做为去重表的唯一索引，把唯一索引插入去重表，再进行业务操作，且他们在同一个事务中。这个保证了重复请求时，因为去重表有唯一约束，导致请求失败，避免了幂等问题。这里要注意的是，去重表和业务表应该在同一库中，这样就保证了在同一个事务，即使业务操作失败了，也会把去重表的数据回滚。这个很好的保证了数据一致性。之前说的 redis 防重也算 

##### 5、全局请求唯一 id

调用接口时，生成一个唯一 id，redis 将数据保存到集合中（去重），存在即处理过。可以使用 nginx 设置每一个请求的唯一 id； proxy_set_header X-Request-Id $request_id;



## Object 划分 

### 1、PO 持久对象

> persistant object

持久对象，通常与数据库中的表相对应，用于表示持久化数据的对象。



### 2、DO 领域对象

> Domain Object

领域对象，表示业务领域中的对象，通常是对业务实体的抽象和封装。



### 3、TO 数据传输对象

> Transfer Object

数据传输对象，用于在不同层之间传输数据，通常用于网络传输或者方法调用参数传递。



### 4、DTO 数据传输对象

> Data Transfer Object

数据传输对象，与TO类似，用于在不同层之间传输数据，是一种设计模式的实现。



### 5、VO 值对象 

> value object

值对象，通常用于表示简单的值，比如日期、金额等，用于传递不可变的数据。

View object：视图对象； 

接受页面传递来的数据，封装对象 

将业务处理完成的对象，封装成页面要用的数据 

### 6、BO 业务对象

> business object

业务对象，用于表示业务逻辑处理的对象，通常包含业务规则和操作。



### 7、POJO 简单无规则java 对象

> plain ordinary java object

简单无规则Java对象，是一种Java编程模型，指的是普通的Java对象，不继承特定的类或实现特定的接口。 

POJO 是 DO/DTO/BO/VO 的统称。

### 8、DAO 数据访问对象

> data access object

数据访问对象，用于封装对数据源的访问和操作，提供了对数据的CRUD（创建、读取、更新、删除）操作。



## 其他

### 1、Mybatis-Plus分页配置

```java
@Configuration
@EnableTransactionManagement
@MapperScan("com/atgoes/shopmall/product/dao")
public class MybatisConfig {

    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();

        paginationInterceptor.setOverflow(true);
        paginationInterceptor.setLimit(1000);

        return paginationInterceptor;
    }
}
```



### 2、Web视图映射

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    /**
     * 视图映射
     * @param registry
     */
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("login.html").setViewName("login");
        registry.addViewController("reg.html").setViewName("reg");
    }
}
```



### 3、解决跨域问题

方法1：

```java
@Configuration
public class MyCorsConfiguration {

    @Bean
    public CorsWebFilter corsWebFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration corsConfiguration = new CorsConfiguration();

        // 1、配置跨域
        corsConfiguration.addAllowedHeader("*");
        corsConfiguration.addAllowedMethod("*");
        corsConfiguration.addAllowedOrigin("*");
        corsConfiguration.setAllowCredentials(true);

        source.registerCorsConfiguration("/**", corsConfiguration);
        return new CorsWebFilter(source);
    }
}
```





# 技术