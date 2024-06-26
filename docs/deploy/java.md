# Java

## Spring Boot

### 解决跨域问题

::: tip 跨域问题
浏览器为了用户的安全，仅允许向 **同域名、同端口** 的服务器发送请求。
:::

#### 网关支持 (Nginx)

让Nginx代理服务器告诉浏览器：允许跨域（返回 cross-origin-allow 响应头）

> [配置允许跨域 - nginx](nginx.md#配置允许跨域)

#### 修改后端服务

添加允许跨域调用的过滤器

```java
@Configuration
public class GlobalCorsConfig {
    
    @Bean
    public CorsFilter corsFilter() {
        CorsConfiguration config = new CorsConfiguration();
        //允许所有域名进行跨域调用
        config.addAllowedOriginPattern("*");
        //允许跨越发送cookie
        config.setAllowCredentials(true);
        //放行全部原始头信息
        config.addAllowedHeader("*");
        //允许所有请求方法跨域调用
        config.addAllowedMethod("*");
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", config);
        return new CorsFilter(source);
    }
}
```

### 多环境配置

::: tip 多环境
指同一套项目代码在不同的阶段需要根据实际情况来调整配置并且部署到不同的机器上。
:::

在 SpringBoot 项目中，通过多个 `application-xxx.yaml` 的方式配置多环境。针对不同环境做不同的配置。

通过 application-dev.yml 与 application-prod.yml 区分环境：

1. 数据库、缓存地址 
2. 端口号 
3. content path

### maven 打包项目

```shell
maven package
```

> 跳过测试打包
>
> ```shell
> mvn package -DskipTests
> ```

### 区分环境运行

在启动项目时传入环境变量，使用`prod`环境配置。

```shell
java -jar ./backennd.jar --spring.profiles.active=prod
```

后台运行

```shell
nohup java -jar ./backend.jar --spring.profiles.active=prod &
```

### idea中将Java项目打包成jar包

点击idea中Maven按钮

<img src="https://ywdoc-1306153177.cos.ap-shanghai.myqcloud.com/2024/04/23/66276ed9045d7.png" alt="image-20240423161917162" style="zoom:150%;" />

依次点击项目名 -> Lifecycle -> package

<img src="https://ywdoc-1306153177.cos.ap-shanghai.myqcloud.com/2024/04/23/66277031c7d3e.png" alt="ywdoc" style="zoom: 200%;" />

查看控制台，查看打包是否成功

jar包存放的地址一般就是项目中target文件夹

![ywdoc](https://ywdoc-1306153177.cos.ap-shanghai.myqcloud.com/2024/04/23/6627725297fff.jpg)

接下来部署你的项目吧
