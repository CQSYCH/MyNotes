

#1、		搭建SpringBoot项目步骤

### 一、创建一个Maven工程项目，在Pom文件中引入SpringBoot依赖

Web工程依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.4.3.RELEASE</version>
  </parent>
  <dependencies>
	 <dependency>
      	<groupId>org.springframework.boot</groupId>
      	<artifactId>spring-boot-starter-web</artifactId>
     </dependency>
  </dependencies>
```

### 二、创建启动类，并引入SpringBoot启动类注解

@SpringBootApplication: 标记一个类为SpringBoot的启动类，程序的入口，此时SpringBoot可以运行起来了，只不过缺少表现层，页面会出现404错误

对于 Spring Boot 应用，建议启动程序的包名层次最高，其他类均在其下，这样 Spring Boot 默认自动搜索启动程序之下的所有类。

```java
/**
 * @ClassName SpringBootEntry
 * @Description 启动类
 * @Author Sun Yu Cheng
 * @Date 2019/8/1 0:44
 * @Version v1.0
 **/
@SpringBootApplication
public class SpringBootEntry {
    public static void main(String[] args){
        SpringApplication.run(SpringBootEntry.class,args);
    }
}
```

### 三、创建控制器

@RestController:标记一个类为Rest控制器类，通常和@RequestMapping一起使用

@RequestMapping:标记一个方法为Url可访问方法

```java
/**
 * @ClassName HomeController
 * @Description 控制器类
 * @Author Sun Yu Cheng
 * @Date 2019/8/1 0:45
 * @Version v1.0
 **/
@RestController
public class HomeController {
    @RequestMapping("/home")
    public String Home(){
        return "hello world;";
    }
}
```

