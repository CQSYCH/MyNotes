# 			Spring AOP

### 1、Spring AOP 基于XML的配置

```xml
<bean id="executionTimeLoggingSpringAop"        class="MySpring.SpringAop.ExecutionTimeLoggingSpringAOP" />
    <aop:config>
        <aop:pointcut id="executionTimeLoggingPointcut"
                      expression="execution(public * MySpring.SpringService.SysUserImpl.getSysUserById*(..))" />
        <aop:advisor id="executionTimeLoggingAdvisor"
                     advice-ref="executionTimeLoggingSpringAop"
                     pointcut-ref="executionTimeLoggingPointcut" />
    </aop:config>
```

### 2、类型签名表达式 

​    为了根据类型（如果接口、类名或者包）过滤方法，Spring AOP提供了within关键字。类型签名模式如下，其中可以使用package name或者class name替换type name。

​     Within（<Type Name>）

接下来列举一些类型签名用法的示例：

* within(com.wiley..*)：该通知将匹配com.wiley包及其子包中所有类中的所有方法。
* within(com.wiley.spring.ch8.MyService)：该通知将匹配MyService类中的所有。
* within(MyServiceInterface+)：该通知将匹配所有实现了MyServiceInterface的类的所有方法。
* within(com.wiley.spring.ch8.MyBaseService+)：该通知将匹配MyBaseService类以及其子类的所有方法。

### 3、方法签名表达式

​    如果想根据方法签名进行过滤，可以使用关键字【excution】

​    表达式：excution(<scope><retuen-type><full-qualified-class-name>.*(parameters))

​    此时，对于与给定的作用域、返回类型、完全限定类名以及参数相匹配的方法，都会应用指定的通知。方法的作用域可以是公共的，保护的或者私有的。如果不想使用参数过滤，可以指定两个点“..”,以表明方法可以接受任何数量和任何类型的参数

* excution(*com.wiley.spring.ch8.MyBean.*\*(..))：该通知将匹配MyBean中的所有方法。
* excution(public *com.wiley.spring.ch8.MyBean.\*(..))：该通知将匹配MyBean中的所有公共方法。
* excution(public String com.wiley.spring.ch8.MyBean.\*(..))：该通知将匹配MyBean中返回Spring的所有公共方法
* excution(public * com.wiley.spring.ch8.MyBean.\*(long,..))：该通知将匹配MyBean中第一个参数被定义为long的所有公共方法。

### 4、其他替代的切入点指示符































