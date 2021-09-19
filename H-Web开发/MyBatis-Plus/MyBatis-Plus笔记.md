# 概述

#### 需要的基础

MyBatis、Spring、SpringMVC 

#### 为什么要学习它呢？

MyBatis-Plus可以节省我们大量工作时间，所有的CRUD代码它都可以自动化完成！ 偷懒的！

官网：https://mp.baomidou.com/

# 快速开始

1. 导入依赖

```xml
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId> 
</dependency>
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifactId>lombok</artifactId> 
</dependency>
<!--尽量不要同时导入 mybatis 和 mybatis-plus！-->
<dependency>
	<groupId>com.baomidou</groupId>
	<artifactId>mybatis-plus-boot-starter</artifactId>
	<version>3.0.5</version> 
</dependency>
```

2. 连接配置

```properties
# mysql 5 驱动不同 com.mysql.jdbc.Driver
# mysql 8 驱动不同 com.mysql.cj.jdbc.Driver(向下兼容)、需要增加时区的配置 serverTimezone=GMT%2B8(东八区)
spring.datasource.username=root 
spring.datasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus? useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

3. pojo-dao-service-controller

```java
@Mapper/@Repository
public interface UserService extends BaseMapper<User>{}
```

4. 启用Mapper扫描

```java
@MapperScan("com.example.mapper")
```



### 配置日志

所有的sql现在是不可见的，我们希望知道它是怎么执行的，所以我们必须要看日志！

```properties
# 配置日志 
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

配置完毕日志之后，后面的学习就需要注意这个自动生成的SQL，你们就会喜欢上 MyBatis-Plus！

