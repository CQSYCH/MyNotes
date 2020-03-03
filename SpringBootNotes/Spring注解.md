

#1、				Spring注解

### 一、@Controller

Spring Mvc注解，表示此类用于负责处理Web请求

### 二、@RestController

- Spring Mvc注解，表示此类用于负责处理Web请求

- RESTFul风格，用于接口json数据传输

- ＠RestController 相当于＠Controller和＠ResponseBody

### 三、@RequestMapping

Spring Mvc注解，表示如果请求路径匹配，被注解的方法将被调用

### 四、@ResponseBody

表示此方法返回的是文本而不是视图名称

### 五、@PropertySource

```java
@PropertySource(value = "classpath:Other.properties")  加载指定的配置文件
```

### 六、@ConfigurationProperties

```java
@ConfigurationProperties(prefix = "Employee") 从配置文件中获取指定数据，默认从全局配置中获取
```

application.yml 配置如下:

```yaml
Employee:
  name: zhangxueyou
  age: 19
  sex: 男
  intro: 为梦想而生.........
  hobby:
    - 唱歌
    - 跳舞
    - 游泳
    - 写代码
    - 赚钱
    - 当老板
    - 玩游戏
    - 游山玩水
```

### 七、@ImportResource

@ImportResource: 导入Spring的配置文件，让配置文件里面的内容在Spring中生效

SpringBoot不提倡这种方式，提倡使用配置类

```java
@MapperScan("project.day01.mapper") Mapper接口的扫描包
@ImportResource(locations = {"classpath:bean.xml"}) 将配置的内容加载到Spring IOC容器
@SpringBootApplication SpringBoot的启动类
public class EntryApplication {
    public static void main(String[] args) {
        SpringApplication.run(EntryApplication.class,args);
    }
}
```

### 八、@Configuration

@Configuration 指定当前类为一个配置类，替代之前的Spring配置文件

```java
/**
 * @ClassName MyAppConfig
 * @Description 配置类，指明当前类是一个配置类，用来替代之前的Spring配置文件
 * @Author Sun Yu Cheng
 * @Date 2019/8/10 17:01
 * @Version v1.0
 * 配置文件中: <Bean id="" class="" ></Bean>
 **/
@Configuration
public class MyAppConfig {
    /**
     * 将方法的返回值添加到容器中，容器中这个组件的id就是这个方法名 id="helloConfig"
     * @return String
     */
    @Bean
    public String helloConfig(){
        System.out.println("helloIOC,这个组件已经加载到Spring配置中了");
        return "helloIOC";
    }
}
```

###九、@ComponentScan 

- @ComponentScan  扫描指定的类路径，如果该路径下的类（POJO）使用了IOC注解，则将该类加入到IOC容器中

```java
@Configuration
@ComponentScan
/**
 * @ComponentScan 扫描 MyAppConfig 包路径下满足注解的POJO
 */
public class MyAppConfig {
    
}
```

- 由于加入了 **excludeFilters** 的配置，使标注了**＠Service **的类将不被 IOC 容器扫描注入

```java
@ComponentScan(basePackages = "project.day01. * ”, 
               excludeFilters ={
        @ComponentScan.Filter(classes = {EmployeeService.class})
}) 
```



### 十、消除歧义性一一＠Primary 和＠Quelifier 

#### 1、＠Primary

- 注解＠Primary，它是一个修改优先权的注解，

- ＠Primary的含义告诉 Spring IOC 容器， 当发现有多个同样类型的 Bean 时,请优先使用我进行注入

  ```java
  /**
   * 当@Autowired 接口时，优先注入被@Primary标识的Bean来获得最高优先级
   */
  @Component
  @Primary
  public class Cat implements Animal{
      
  }
  
  @Component
  public class Dog implements Animal{
      
  }
  
  @Component
  public class BussinessPerson implements Person{
      @Autowired
      /**
  	 * 这里将被注入最高优先级的Cat
   	 */
      private Animal animal = null;
     
  }
  ```

- 如果多个同类型的Bean都被加上@Primary，那么IOC容器将不知道谁的优先级更高

#### 2、＠Quelifier 

- ＠Quelifier 的配置项 value 需要一个字符串去定义，将与＠Autowired 组合在一起，通过类型和名称一起找到 Bean

  ```java
  @Component
  @Primary
  public class Cat implements Animal{
      
  }
  
  @Component
  public class Dog implements Animal{
      
  }
  
  @Component
  public class BussinessPerson implements Person{
      @Autowired
      /**
  	 * 这里将被注入Dog
   	 */
      @Quelifier("dog")
      private Animal animal = null;   
  }
  ```

- 构造方法注入Spring IOC

  ```java
  @Component
  @Primary
  public class Cat implements Animal{
      
  }
  
  @Component
  public class Dog implements Animal{
      
  }
  
  @Component
  public class BussinessPerson implements Person{
      private Animal animal = null;
       /**
  	 * 这里将被注入Dog
   	 */
      public BussinessPerson(@Autowired @Qualifier("dog”) Animal animal){
           this.animal = animal ;                                  
       } 
  }
  ```

  

#2、Spring IOC容器下Bean的生命周期 

- IOC下Bean的生命周期图

  <img src="./../img/Spring_IOC_Bean的生命周期.jpg" width="100%"/>

- **@ComponentScan** 中还有一个配置项 **lazyInit** ，只可以配置 Boolean 值，且默认值为 false，也就是 默认不进行**延迟初始化**，因此在默认的情况下 Spring 会对 Bean 进行实例化和依赖注入对应的属性值。 

- 延迟初始化,在 **调用Bean** 的时候才做初始化

  ```java
  @ComponentScan (basePackages ＝"project.day01. *", lazyinit = true) 
  ```

- Spring对Bean的控制过程
  <img src="./../img/Spring_IOC控制过程.jpg" width="100%"/>

- **注意**: 即使你定义了 ApplicationContextAware 接口，但是有时候并不会调用，这要根据你的 IOC容来决定。 我们知道，Spring IOC 容器最低的要求是实现 BeanFactory 接 口，而不是实现 ApplicationContext 接口 。 对于那些没有实现 ApplicationContext 接口的容器，在生命周期对 应的 ApplicationContextAware 定义的方法也是不会被调用的，只有实现了 ApplicationContext 接口的容器，才会在生命周期调用 ApplicationContextAware 所定义的 setApplicationContext 方法。 

- 加入生命周期接口和自定义

  ```java
  @Component 
  public class BussinessPerson implements Person, BeanNameAware, BeanFactoryAware, ApplicationContextAware, InitializingBean, DisposableBean{
      private Animal animal = null;
      
      @Override
      public void service () {
         this.animal.use(); 
      }
      @Override
      @Autowired
      @Qualifier("dog")
      public void setAnimal (Animal animal){
          System.out.println("延迟依赖注入");
          this.animal= animal；
      }
      @Override
      public void setBeanName (String beanName) {
          System.out.println("<"+ this.getClass().getSimpleName()+＂ 】 调用 BeanNameAware 的 setBeanName"）；
      }
  }
  ```

  