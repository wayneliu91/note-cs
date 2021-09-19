# 是什么？

jvm给开发人员留的扩展接口，让开发人员能在启动时和运行时操纵`class文件`。

# 能干什么？

javaagent的主要的功能如下：

- 可以在`加载class文件之前`修改字节码；
- 可以在`运行期`修改已经加载的类的字节码（这种情况有很多限制）
- 其他一些小众功能：
  - 获取所有已经被加载过的类
  - 获取所有已经被初始化过了的类（执行过了clinit方法，是上面的一个子集）
  - 获取某个对象的大小
  - 将某个jar加入到bootstrapclasspath里作为高优先级被bootstrapClassloader加载
  - 将某个jar加入到classpath里供AppClassloard去加载
  - 设置某些native方法的前缀，主要在查找native方法的时候做规则匹配

# 原理

在java5时，jdk新增了一个`java.lang.instrument.Instrumentation` 接口，提供在运行时操纵类的`class文件`的功能。

Java Agent是一个Jar包，只是启动方式和普通Jar包有所不同，对于普通的Jar包，通过指定类的main函数进行启动，但是Java Agent并不能单独启动，必须依附在一个Java应用程序运行。

# 使用步骤

1. 编写一个agent类，并创建premain方法：

注意：premain方法的参数是固定的。

```java
public class PreMainAgent {
    public static void premain(String agentParam, Instrumentation instrumentation) {
        AgentBuilder.Transformer transformer = new AgentBuilder.Transformer() {
            @Override
            public DynamicType.Builder<?> transform(DynamicType.Builder<?> builder, TypeDescription typeDescription,
                                                    ClassLoader classLoader, JavaModule javaModule) {
                return builder.method(ElementMatchers.any())
                              .intercept(MethodDelegation.to(TimeInterceptor.class));
            }
        };
        new AgentBuilder.Default().type(ElementMatchers.nameStartsWith("com.service"))
                                  .transform(transformer)
                                  .installOn(instrumentation);
    }
}

public class TimeInterceptor {
    @RuntimeType
    public static Object intercept(@Origin Method method, @SuperCall Callable<?> callable) throws Exception {
        long start = System.currentTimeMillis();
        try {
            return callable.call();
        } finally {
            long end = System.currentTimeMillis();
            System.out.println(method.getName() + ":" + (end - start) + "ms");
        }
    }
}
```

2. 告诉jvm，premain方法的位置（有2种方式）

   1. 创建并编辑`resources/META-INF/MANIFEST.MF`文件，在文件中指定premain方法所在的类`Premain-Class:example.TestAgent`，当打jar包时将该文件一起打包

   2. 如果使用maven打包，在pom.xml中使用插件（这里有几个插件都能用）

      ```xml
      <dependency>
          <groupId>net.bytebuddy</groupId>
          <artifactId>byte-buddy</artifactId>
          <version>1.10.13</version>
      </dependency>
      <dependency>
          <groupId>net.bytebuddy</groupId>
          <artifactId>byte-buddy-agent</artifactId>
          <version>1.10.13</version>
      </dependency>
      
      <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
          <configuration>
              <archive>
                  <manifest>
                      <addClasspath>true</addClasspath>
                  </manifest>
                  <manifestEntries>
                      <Premain-Class>com.hexuan.agent.demo1.preMainAgentClz</Premain-Class>
                      <Agent-Class>com.hexuan.agent.demo1.agentMainAgentClz</Agent-Class>
                  </manifestEntries>
              </archive>
          </configuration>
      </plugin>
      
      <!--或者-->
      <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>3.1.0</version>
          <configuration>
              <appendAssemblyId>false</appendAssemblyId>
              <descriptorRefs>
                  <descriptorRef>jar-with-dependencies</descriptorRef>
              </descriptorRefs>
              <archive>
                  <manifest>
                      <addClasspath>true</addClasspath>
                  </manifest>
                  <manifestEntries>
                      <Premain-Class>org.example.LinkAgent</Premain-Class>
                      <Can-Redefine-Classes>true</Can-Redefine-Classes>
                      <Can-Retransform-Classes>true</Can-Retransform-Classes>
                  </manifestEntries>
              </archive>
          </configuration>
          <executions>
              <execution>
                  <id>make-assembly</id>
                  <phase>package</phase>
                  <goals>
                      <goal>single</goal>
                  </goals>
              </execution>
          </executions>
      </plugin>
      
      ```

3. 在启动时，在jvm启动参数中使用`-javaagent`参数告诉jvm整个agent jar包的位置，找到jar包才能找到MANIFEST文件，才能根据文件中的内容找到对应的类，然后才能调用类的premain方法。

   ```shell
   java -javaagent:/usr/local/example/java-agent-demo.jar
   ```

# 使用案例

热部署工具JRebel，各种线上诊断工具（btrace, greys），阿里开源的arthas。