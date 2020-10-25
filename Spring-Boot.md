# `Spring Boot`



# 核心篇

## `Hello Word`

### `pom.xml`配置

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>{{ spring-boot.version }}</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
	<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

### `application.properties`配置

```properties
# 配置 tomcat 端口
server.port=185

# 配置 上下文
server.servlet.context-path=/SpringBootApplicaiton
```

### 编写主程序

```Java
@SpringBootApplication
public class App {
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

### `Hello World`控制层

```java
@RestController
public class HelloController{
    
    @RequestMapping("hello")
    public String hello(){
        return "Hello World"
    }
}
```



## 配置

### 配置文件

`Spring Boot`使用一个全局的配置文件

- `application.properties`

  - 对象，`Map`，键值对语法

  - ```properties
    user.name=five
    user.age=18
    ```

  - `List`，`Set`，数组语法

  - ```properties
    users=five,root,admin
    ```

- `application.yml`

  - 字符串语法

  - ```yaml
    # 字符串默认不加引号，双引号不会转义特殊字符，单引号会转义特殊字符
    name: five
    say1: "hello \n world"		# hello 换号 world
    say2: 'hello \n world'		# hello \n world
    ```

  - 对象，`Map`，键值对语法

  - ```yml
    user: 
    	name: five
    	age: 18
    	
    # 行内语法
    user: { name: five, age: 18 }
    ```

  - `List`，`Set`，数组语法

  - ```yaml
    users:
    	- five
    	- root
    	- power
    	
    # 行内语法
    users: [ five, root, power ]
    ```



### 使用全局配置文件中的配置

在`pom.xml`中添加，使`IDEA`具有配置提示

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

在`application.yml`中添加配置

```yaml
user-info:
  name: five
  age: 18
  sex: true
  hobby:
    - rap
    - 跳舞
    - 篮球
  data:
    code: 0
    msg: 正常
```

创建`UserInfoBean`组件

基于`@ConfigurationProperties`注解

```Java
@Component
@ConfigurationProperties(prefix = "user-info")
public class UserInfoBean {
    private String name;
    private Integer age;
    private List<String> hobby;
    private Map<String,Object> data;
    private Boolean sex;
    
    // And getter and setter
    
    // And toString
}
```

基于`@Value`注解

```java
@Component
public class UserInfoBean {
    
    // ${} 从配置文件中取值
    @Value("${user.name}")
    private String name;
    
    // #{} SpringEL表达式
    @Value("#{28-10}")
    private Integer age;
    
    // @Value 不支持复杂数据注入
    private List<String> hobby;
    private Map<String,Object> data;
    
    @Value("true")
    private Boolean sex;
    
    // And getter and setter
    
    // And toString
}
```

基于`Spring Boot Test`测试

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class AppTest {

    @Autowired
    UserInfoBean userInfoBean;

    @Test
    public void test(){
        System.out.println(userInfoBean);
    }

}
```



### 使用指定配置文件中的配置

在`user-info.properties`中添加配置

```yaml
user-info.name: five
```

创建`UserInfoBean`组件

```java
@Component
// PropertySource 只支持properties文件
@PropertySource(value = { "classpath:user-info.properties" })
@ConfigurationProperties(prefix = "user-info")
public class UserInfoBean {
    private String name;
    private Integer age;
    private List<String> hobby;
    private Map<String,Object> data;
    private Boolean sex;
    
    // And getter and setter
    
    // And toString
}
```

基于`Spring Boot Test`测试

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class AppTest {

    @Autowired
    UserInfoBean userInfoBean;
    
    // @Autowired
    // ApplicationContext ioc;		ioc容器

    @Test
    public void test(){
        System.out.println(userInfoBean);
    }

}
```

> 用`@ImportResource`注解标注在主方法中，用来导入基于`xml`形式的配置文件



### 多环境配置文件支持

#### 基于多文件配置

> `yml`也可以使用这种形式

以`application-环境名.properties`的形式定义某个环境下要使用的配置文件，在主配置文件中声明要使用的环境

在`application-dev.properties`中添加配置

```properties
server.port=8081
```

在`application.properties`中添加配置

```properties
spring.profiles.active=dev
```



#### 在`yml`文件下基于多文档块配置

以`---`划分文档块，`spring.profiles`指定文档块名称，`spring.profiles.active`指定要激活使用的文档块

```yaml
spring:
	profiles: 
		active: dev
		
---

spring: 
	profiles: dev
	
server: 
	port: 8081
```



### 使用指定位置的配置文件

在`application.properties`添加

```properties
spring.config.location=location
```



### 输出自动配置类报告

在`application.properties`添加

```properties
debug=true
```





## 日志

### 基本使用

```java
@SpringBootTest
public class AppTest {

    Logger logger = LoggerFactory.getLogger(this.getClass());

    @Test
    public void log(){
        logger.trace("this is trace");
        logger.debug("this is debug");
        logger.info("this is info");
        logger.warn("this is warn");
        logger.error("this is error");
    }
}

```

### 指定日志级别

在`application.properties`中添加配置

```properties
logging.level.包名=[ trace | debug | info | warn | error ]

# 将日志保存到指定的文件中
# 可以指定路径，如果不指定路径，则在当前项目下创建日志文件
logging.file=文件名

# 将日志保存到指定的目录中
logging.path=路径

# 指定日志在控制台输出的格式
logging.pattern.console=格式
```









