# 服务

1. 加依赖

   ```xml
   <!-- Dubbo Spring Boot Starter -->
   <dependency>
       <groupId>org.apache.dubbo</groupId>
       <artifactId>dubbo-spring-boot-starter</artifactId>
       <version>2.7.7</version>
   </dependency>
   <!--zk客户端-->
   <dependency>
       <groupId>org.apache.curator</groupId>
       <artifactId>curator-framework</artifactId>
       <version>5.0.0</version>
   </dependency>
   <dependency>
       <groupId>org.apache.curator</groupId>
       <artifactId>curator-recipes</artifactId>
       <version>5.0.0</version>
   </dependency>
   <dependency>
       <groupId>org.apache.curator</groupId>
       <artifactId>curator-client</artifactId>
       <version>5.0.0</version>
   </dependency>
   ```

2. 写配置

   ```properties
   # Base packages to scan Dubbo Component: @org.apache.dubbo.config.annotation.Service
   dubbo.scan.base-packages=com.example.order.service
   
   # Dubbo Application
   ## The default value of dubbo.application.name is ${spring.application.name}
   ## dubbo.application.name=${spring.application.name}
   
   # Dubbo Protocol
   dubbo.protocol.name=dubbo
   dubbo.protocol.port=12345
   
   ## Dubbo Registry
   # 这里如果只是本地调用，可以只要 dubbo.registry.address=N/A
   dubbo.registry.protocol=zookeeper
   dubbo.registry.address=127.0.0.1:2181
   ```

3. 写接口

   ```java
   @DubboService(version = "1.0.0")
   ```

# 消费者

1. 加依赖

   ```xml
   <!--和服务一样-->
   ```

2. 写配置

   ```properties
   # 如果是本地测试，这些都可以不要
   # Base packages to scan Dubbo Component: @org.apache.dubbo.config.annotation.Service
   dubbo.scan.base-packages=com.example.userweb.controller
   
   # Dubbo Application
   ## The default value of dubbo.application.name is ${spring.application.name}
   ## dubbo.application.name=${spring.application.name}
   
   # Dubbo Protocol
   dubbo.protocol.name=dubbo
   dubbo.protocol.port=12346
   
   ## Dubbo Registry
   dubbo.registry.register=false
   dubbo.registry.protocol=zookeeper
   dubbo.registry.address=127.0.0.1:2181
   ```

3. 引用

   ```java
   // 本地引用：@DubboReference(version = "1.0.0",url = "dubbo://127.0.0.1:12345")
   @DubboReference(version = "1.0.0")
   ```