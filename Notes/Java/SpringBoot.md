# SpringBoot

Spring Boot是一个基于Spring的套件，它帮我们预组装了Spring的一系列组件，以便以尽可能少的代码和配置来开发基于Spring的Java应用程序。

- 官网：https://spring.io/projects/spring-boot#overview

### 打包应用

在`pom.xml`中加入以下配置：

```xml
<project ...>
    ...
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

执行命令打包：

```sh
$ mvn clean package
```

打包后我们在`target`目录下可以看到两个jar文件：

```
$ ls
...
springboot-exec-jar-1.0-SNAPSHOT.jar
springboot-exec-jar-1.0-SNAPSHOT.jar.original
```

其中，`springboot-exec-jar-1.0-SNAPSHOT.jar.original`是Maven标准打包插件打的jar包，它只包含我们自己的Class，不包含依赖，而`springboot-exec-jar-1.0-SNAPSHOT.jar`是Spring Boot打包插件创建的包含依赖的jar，可以直接运行：

```sh
$ java -jar springboot-exec-jar-1.0-SNAPSHOT.jar
```
