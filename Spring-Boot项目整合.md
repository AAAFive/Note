# `Spring-Boot`

## `Spring-Boot`基础配置

### `pom.xml`配置

```html
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.2.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
	<!-- Spring-Boot Web 启动器 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    
    <!-- Spring-Boot 单元测试启动器 -->
	<dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
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

### 启动`Spring-Boot应用`

```java
@SpringBootApplication
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```







## `Spring-Boot`整合`Spring-JDBC`

### `pom.xml`配置

```xml
<!-- MySQL 连接器 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.45</version>
</dependency>

<!-- Spring-JDBC 启动器 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

### 配置数据源

在 `application.properties` 中添加

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test
spring.datasource.username=root
spring.datasource.password=1=23.
```

### 测试

基于`Spring-Boot-Test`进行测试

```java
@SpringBootTest
public class SpringJDBCTest {

    @Autowired
    JdbcTemplate template;
    
    static class User{
        private Integer userId;
        private String userName;
        
        // Add Getter and Setter ..
        
        // Override toString ..
    }
    
    static class UserMapper implements RowMapper<User>{
        @Override
        public User mapRow(ResultSet rs, int rowNum) throws SQLException {
            User user = new User();
            user.setUserId(rs.getInt("userId"));
            user.setUserName(rs.getString("userName"));
            return user;
        }
    }
    
    @Test
    public void findAll(){
        String SQL = "select * from user";
        List<User> data = template.query( SQL, new UserMapper() );
		System.out.println( data );
    }
   
    @Test
    public void add(){
        String SQL = "insert into user(userName) value(?)"；
        template.update(SQL,"root");
        this.findAll();
    }
}
```







## `Spring-Boot`整合`MyBatis`

### `pom.xml`配置

```xml
<!-- MySQL 连接器 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.45</version>
</dependency>

<!-- MyBatis 启动器 -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>

<!-- MyBatis 分页插件 -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.5</version>
</dependency>
```

### 配置数据源

在 `application.properties` 中添加

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test
spring.datasource.username=root
spring.datasource.password=1=23.
```

### 创建实体类

```java
package com.entitys;

public class User{
    private Integer userId;
    private String userName;
    
    // Add Getter and Setter ..
        
    // Override toString ..
}
```

### 创建`Mapper`接口

```java
package com.mappers;

public interface UserMapper {
    @Select("select * from `user`")
    public List<User> findAll();
}
```

### 创建控制器

查询所有 和 分页查询

```java
package com.contollers;

@RestController
public class UserManage {

    @Autowired
    UserMapper userMapper;

    // 查询所有
    @RequestMapping("findAll")
    public List<User> findAll(){
        return userMapper.findAll();
    }
    
    // 分页查询
    @RequestMapping("findByPage")
    public List<User> findByPage(Integer page,Integer limit){
        
        // 设置 页数 和 显示的条数
        PageHelper.startPage(page, limit);
        
        List<User> data = userMapper.findAll();
        
        // PageInfo 是 PageHelper 的内置的分页信息类
        PageInfo info= new PageInfo(data);
        return data; 
    }
}
```

### 启动时扫描`Mapper`接口

```java
@SpringBootApplication
@MapperScan("com.mappers")
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```

### 基于 `Spring-Boot-Test` 测试

```java
@SpringBootTest
@RunWith(SpringRunner.class)
public class ApplicationTest {
    @Autowired
    UserMapper mapper;

    @Test
    public void test(){
        System.out.println(mapper.findAll());
    }
}
```







## `Spring-Boot`整合`MyBatis-Plus`

### `pom.xml`配置

```xml
<!-- MySQL 连接器 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.45</version>
</dependency>

<!-- MyBatis-Plus 启动器 -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.3.2</version>
</dependency>
```

### 配置数据源

在`application.properties`中添加

```properties
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test
spring.datasource.username=root
spring.datasource.password=1=23.

# 启用驼峰式命名法
mybatis-plus.configuration.map-underscore-to-camel-case=false
```

### 创建实体类

```java
package com.entitys;

public class User{
    private Integer userId;
    private String userName;
    
    // Add Getter and Setter ..
        
    // Override toString ..
}
```

### 创建`Mapper`接口

让`Mapper`接口继承于`BaseMapper`接口

```java
package com.mappers;

public interface UserMapper extends BaseMapper<User>{
    
}
```

### 创建控制器

```java
package com.controllers;

@RestController
public class UserInfoManage{
    
    @Autowired
    UserMapper userMapper;

    @RequestMapping("findAll")
    public List<User> findAll(){
        return userInfoMapper.selectByMap(null);
    }
}
```

### 启动时扫描`Mapper`接口

```java
@SpringBootApplication
@MapperScan("com.mappers")
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```







## `Spring-Boot`整合`JSP`

### `pom.xml`配置

```xml
<!-- jstl标签 -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>
<!-- jasper -->
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
    <scope>provided</scope>
</dependency>
```

### 配置视图解析器

```properties
spring.mvc.view.prefix=/WEB-INF/JSP/
spring.mvc.view.suffix=.jsp
```

### 创建`webapp`

在`main`文件夹下创建`webapp`

```
-- main
    +- webapp
        +- WEB-INF
            += JSP
                += index.jsp
            += web.xml
```

### 编辑`index.jsp`

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>hello ${name}</h1>
</body>
</html>
```

### 创建控制器

```java
package com.contollers;

@Controller
public class SayHello {

    @RequestMapping("/hello/{name}")
    public String hello(ModelAndView mav, @PathVariable String name){
        mav.addObject("name",name);
        return "index";
    }
}
```

### 启动

```java
package com;

@SpringBootApplication
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```







## `Spring-Boot`整合`thymeleaf`

### `pom.xml`配置

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

### `html`头部约束

```html
<html xmlns:th="http://www.thymeleaf.org">
```







## `Spring-Boot`整合`Security`

### `pom.xml`配置

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

### 配置默认用户名和密码

```properties
spring.security.user.name=five
spring.security.user.password=1=23.
```

### 编写`Security`的配置类

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/index")
                .permitAll()	// index 请求所有角色可访问
                .antMatchers("/vip1/**").hasAnyRole("vip1") // vip1 角色权限
                .antMatchers("/vip2/**").hasAnyRole("vip2")
                .antMatchers("/vip3/**").hasAnyRole("vip3");

        http.csrf().disable();	// 关闭 crsf攻击 检测
        
        http.formLogin()
                .loginPage("/tologin")	// 设置登录页面
                .usernameParameter("username")
                .passwordParameter("password")
                .loginProcessingUrl("/login");  // 设置登录的请求
        
        http.logout().logoutSuccessUrl("/index"); // 用户退出后跳转的页面
        
        http.rememberMe(); // 记住我
    }

    
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication()
                .passwordEncoder(new BCryptPasswordEncoder()) // 设置密码加密方式
                .withUser("Five")	// 用户名
                .password(new BCryptPasswordEncoder().encode("1=23.")) // 密码
                .roles("vip1","vip2","vip3") //权限
                .and()	// 使用 and 连接添加多个用户 
                .withUser("root")
                .password(new BCryptPasswordEncoder().encode("1=23."))
                .roles("vip2");
    }
}
```

### 编写基于数据库的`Security`的配置类

编写实体类

```java
@TableName("userinfo")
public class UserInfo {
    private Integer userId;
    private String username;
    private String password;
    private String role;
    
    // Add Getter and Setter ..
        
    // Override toString ..
}
```

编写`Mapper`

```java
@Mapper
public interface UserInfoMapper extends BaseMapper<UserInfo> {
}
```



编写`UserDetailsServiceImpl`

```java
@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    UserInfoMapper userInfoMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        Map where = new HashMap<String,Object>();
        where.put("username",username);

        UserInfo userInfo = userInfoMapper.selectOne( new QueryWrapper<>().allEq(where));

        if( userInfo == null ){
            return null;
        }else{
            Collection<GrantedAuthority> authorities = new ArrayList<>();
            authorities.add(new SimpleGrantedAuthority(userInfo.getRole()));
            User user = new User(userInfo.getUsername(), new BCryptPasswordEncoder().encode(userInfo.getPassword()),authorities);
            System.out.println("管理员信息："+user.getUsername()+"   "+new BCryptPasswordEncoder().encode(userInfo.getPassword())+"  "+user.getAuthorities());
            return user;
        }
    }
}
```

### 编写配置类

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/index")
                .permitAll()
                .antMatchers("/vip1/**").hasAnyAuthority("vip1")
                .antMatchers("/vip2/**").hasAnyAuthority("vip2")
                .antMatchers("/vip3/**").hasAnyAuthority("vip3");

        http.csrf().disable();
        http.formLogin()
                .loginPage("/tologin")
                .usernameParameter("username")
                .passwordParameter("password")
                .loginProcessingUrl("/login");

        http.logout().logoutSuccessUrl("/index");
    }

    @Autowired
    UserDetailsServiceImpl userDetailsService;


    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(new BCryptPasswordEncoder());
    }
}
```



### 创建控制器

```java
@Controller
public class SecurityTest {

    @RequestMapping("index")
    public String index(){
        return "index";
    }

    @RequestMapping("tologin")
    public String tologin(){
        return "login";
    }

    @RequestMapping("vip1/{page}")
    @Response
    public String vip1(@PathVariable String page){
        return "This is the "+ page +" page for VIP1";
    }
    @RequestMapping("vip2/{page}")
    @Response
    public String vip2(@PathVariable String page){
        return "This is the "+ page +" page for VIP2";
    }
    @RequestMapping("vip3/{page}")
    @Response
    public String vip3(@PathVariable String page){
        return "This is the "+ page +" page for VIP3";
    }
}

```

### 登录页面

```html
<form action="/login" method="post">
    <table>
        <tr>
            <td>用户名</td>
            <td>
                <input type="text" name="username">
            </td>
        </tr>
        <tr>
            <td>密码</td>
            <td>
                <input type="password" name="password">
            </td>
        </tr>
        <tr>
            <td colspan="2">
                <input type="submit">
            </td>
        </tr>
    </table>
</form>
```

### 首页

```html
<h1>首页</h1>
<hr>
<h2>VIP1</h2>
<ul>
    <li><a href="/vip1/1">to like 1</a></li>
    <li><a href="/vip1/2">to like 2</a></li>
    <li><a href="/vip1/3">to like 3</a></li>
</ul>
<hr>
<h2>VIP2</h2>
<ul>
    <li><a href="/vip2/1">to like 1</a></li>
    <li><a href="/vip2/2">to like 2</a></li>
    <li><a href="/vip2/3">to like 3</a></li>
</ul>
<hr>
<h2>VIP3</h2>
<ul>
    <li><a href="/vip3/1">to like 1</a></li>
    <li><a href="/vip3/2">to like 2</a></li>
    <li><a href="/vip3/3">to like 3</a></li>
</ul>
```







## `Spring-Boot`文件上传

### 封装`UploadFile`组件

```java
@Component
public class UploadFile {

    private String uploadPath;
    private MultipartFile file;
    private String fileName;
    private String url;

    public UploadFile setUploadPath(String uploadPath) {
        if( uploadPath.endsWith("/") ){
            this.uploadPath = uploadPath;
        }else{
            this.uploadPath = uploadPath+"/";
        }
        return this;
    }

    public UploadFile setFile(MultipartFile file) {
        this.file = file;
        return this;
    }

    public String getFileName() {
        return fileName;
    }

    public String getUrl() {
        return this.url = this.uploadPath+this.fileName;
    }

    public boolean upload(){
        if("".equals(file.getOriginalFilename())){
            return false;
        }
        String fileName = UUID.randomUUID()+"-"+file.getOriginalFilename();
        File file1 = new File(uploadPath+fileName);
        if( !file1.exists() ){
            file1.mkdirs();
        }
        try {
            file.transferTo(file1);
        } catch (IOException e) {
            e.printStackTrace();
            return false;
        }
        this.fileName = file1.getName();
        return true;
    }

    public boolean upload( String uploadPath, MultipartFile file ){
        this.setUploadPath(uploadPath);
        this.setFile(file);
        return this.upload();
    }

}
```

### 创建资源映射配置类

```java
@Configuration
public class MyWebMvcConfigurer implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/imgs/**").
                addResourceLocations("file:/D:/AAA/");
    }
}
```



### 创建控制器

```java
@Controller
public class Upload {

    @Autowired
    UploadFile uploadFile;

    @RequestMapping("/upload")
    @ResponseBody
    public String upload( @RequestParam("file")MultipartFile file ){
        String url = "D:/AAA";

        uploadFile.upload(url,file);

        return "Upload Success "+uploadFile.getFileName();
    }

    @RequestMapping("file")
    public String toUpload(){
        return "file";
    }

}
```

### 编写`file.html`页面

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="/upload" method="post" enctype="multipart/form-data">
        <input id="file" type="file" name="file">
        <input type="submit">
    </form>
</body>
</html>
```







## `Spring-Boot`文件下载

### 封装`DownloadFile`组件

```java
@Component
public class DownloadFile {

    public ResponseEntity<FileSystemResource> download(String url){
        File file = new File(url);
        return this.download(file);
    }

    public ResponseEntity<FileSystemResource> download(File file) {
        if (file == null) {
            return null;
        }
        HttpHeaders headers = new HttpHeaders();
        headers.add("Cache-Control", "no-cache, no-store, must-revalidate");
        headers.add("Content-Disposition", "attachment; filename=" + file.getName());
        headers.add("Pragma", "no-cache");
        headers.add("Expires", "0");
        headers.add("Last-Modified", new Date().toString());
        headers.add("ETag", String.valueOf(System.currentTimeMillis()));

        return ResponseEntity
                .ok()
                .headers(headers)
                .contentLength(file.length())
                .contentType(MediaType.parseMediaType("application/octet-stream"))
                .body(new FileSystemResource(file));
    }
}
```

### 创建控制器

```java
@Controller
public class Upload {

    @Autowired
    DownloadFile downloadFile;

    @RequestMapping("/download")
    public ResponseEntity<FileSystemResource> dowload(HttpServletResponse response){
        String url = this.getClass().getClassLoader().getResource("").getPath()+"imgs/1.jpg";

        return downloadFile.download(new File(url));
    }

}
```















# `MyBatis-Plus`

## 代码生成器

```java
package com.utils;

import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;
import com.baomidou.mybatisplus.core.toolkit.StringUtils;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.*;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

import java.util.Scanner;

public class MyBatisPlusGenerator {

    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入" + tip + "：");
        System.out.println(help.toString());
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotEmpty(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    public static void main(String[] args) {
        AutoGenerator mpg = new AutoGenerator();
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("Five");
        gc.setOpen(false);
        mpg.setGlobalConfig(gc);

        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/test?useUnicode=true&useSSL=false&characterEncoding=utf8");
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("1=23.");
        mpg.setDataSource(dsc);

        PackageConfig pc = new PackageConfig();
        pc.setModuleName(scanner("模块名"));
        pc.setParent("com.test");
        mpg.setPackageInfo(pc);

        TemplateConfig templateConfig = new TemplateConfig();

        templateConfig.setXml(null);
        mpg.setTemplate(templateConfig);

        StrategyConfig strategy = new StrategyConfig();
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        strategy.setEntityLombokModel(true);
        strategy.setRestControllerStyle(true);

        strategy.setSuperEntityColumns("id");
        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
        strategy.setControllerMappingHyphenStyle(true);
        strategy.setTablePrefix(pc.getModuleName() + "_");
        mpg.setStrategy(strategy);
        mpg.execute();
    }
}

```





