---
title: Spring Boot Profile
---
在Spring boot启动时，可以设置profile
```java
		new SpringApplicationBuilder(SiteMain.class)
		.web(true)
		.bannerMode(Banner.Mode.OFF)
		.profiles("test1")
		.run(args);
```
这里设置成test1，也可以通过启动参数`spring.profiles.active`来启动设置启动参数中写成`-Dspring.profiles.active=test1`

##配置文件读取
在spring中，使用
```
@Configuration
@ConfigurationProperties(prefix="http")
@PropertySource({"${encrypt.file:classpath:config.properties}"})
```
会读取配置文件中以`http`开头的配置，并映射`http`二级参数到配置的`bean`中，如：`http.url`会映射到bean下面的`url`属性中。由于有`@PropertySource`，配置文件会都读取到spring的环境变量中来`Environment`，在以上的`@PropertySource`中，使用`${}`取`application.yml`中的配置`encrypt.file`，如果取不到，则取`:`后的默认配置文件`classpath:config.properties`，而我们的`encrypt.file`配置在springprofile下，会自动按`profiles`分配，如：
```
spring:
  profiles: test1
encrypt:
  file: file:d:/config.properties
---
spring:
  profiles: test2
encrypt:
  file: file:d:/config2.properties
```
其中用`---`分隔两个profile
