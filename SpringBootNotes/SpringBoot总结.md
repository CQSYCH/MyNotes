##								SpringBoot入门篇

##1、SpringBoot与Maven包

（一）、pom文件引入父级SpringBoot包

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>1.4.3.RELEASE</version>
</parent>
```

（二）、引入相关Jar

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

（三）、数据库连接

```xml
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.6</version>
</dependency>
```

##2、yml配置文件

（一）、SpringBoot使用一个全局的配置文件,配置文件的名称是固定的

- application.properties	
- application.yml

配置文件的作用:修改SpringBoot的自动配置默认值（SpringBoot底层默认自动配置好了）

yml(YAML-YAML Ain't Markup Language)语言的文件 ，以数据为中心，比json、xml等更适合做配置文件

（二）、yml配置更为简洁

```java
server:
  port: 8099
```

（三）、yml基本语法

K:(空格)v :表示一对键值对（空格必须有）；

以空格的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的

```yaml
server:
    port: 8081
    path: /home
```

属性的值区分大小写；

（四）、YAML值的写法

- 字面量:普通的值（数字、字符串、布尔）;

		K: v : 字面直接来写；

		字符串默认不用加上单引号或者双引号；
		
		"": 双引号，不会转义字符串里面的特殊字符；特殊字符会作为本身想表达的意思
		
				name: “zhangsan \n ok” :输出 ------>zhangsan 换行 ok
		
		‘’:  单引号，会转义特殊字符，特殊字符最终只是一个普通的字符串数据
		
				name: 'zhangsan \n ok' :输出-------->zhangsan \n ok

​       

- 对象、map（属性和值-键值对）;

  K: v : 在下一行来写对象的属性和值的关系（注意缩进）

  ​	对象还是K: v的方式

  ```yaml
  	friends:
  
  		lastName: zhangsan
  
  		age: 20
  
  ```

  行内写法

  ```YAML
  friends: {lastName: zhangsan,age: 20}
  ```

  

- 数组（List、Set）:

  用 - 值表示数组中的一个元素

  ```yaml
  pets: 
   - cat
   - dog
   - goods
  ```

  行内写法

  ```yaml
  pets: [cat,dog,goods]
  ```


## 3、使用热部署

描述:Spring Boot提供了 Spring-boot-devtools,它能在修改类或配置文件的时候自动重新加载Spring Boot应用

引入:在Pom文件中添加如下依赖

```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional> 
</dependency> 
```

##4、配置文件占位符

```java
 ${random.int}    随机整数
 ${random.uuid}   随机GUID
 ${Employee.age}  其他配置变量
 ${Employee.hobby} 其他配置变量
```



## 5、Profile Spring 多环境支持

- **Profile**是Spring对不同环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境

- 多Profile文件形式:

 			-格式:application-{profile}.properties

​			-开发环境配置: application-dev.properties

​			-生产环境配置: application-prod.properties

- 多profile文档块模式

- 激活命令

  ​		-命令行  --spring.profiles.active=dev

  ​		-配置文件 spring.profiles.active=dev

  ​		-jvm参数 -Dspring.profiles.active=dev

## 6、Spring YML文档块配置

- Yml配置中 **--** 用来区分每个文档块的作用域s

```yaml
server:
  port: 8000
spring:
  profiles:
    active: dev 激活Spring配置
---
#开发环境配置
server:
  port: 8001
spring:
  profiles: dev
  datasource:
      driver-class-name: com.mysql.jdbc.Driver
      url: jdbc:mysql://localhost:3306/yd_portal?useUnicode=true&characterEncoding=utf-8&useSSL=false
      username: AdminTest
      password: Admin123
mybatis:
  type-aliases-package: project.day01.mapper
  mapper-locations: classpath:mybatis/mapper/*.xml
---
#生产环境
server:
  port: 8002
spring:
  profiles: prod
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/yd_portal?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: AdminTest
    password: Admin123
mybatis:
  type-aliases-package: project.day01.mapper
  mapper-locations: classpath:mybatis/mapper/*.xml

```



