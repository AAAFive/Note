# `shiro`权限管理

## 快速开始

创建`Realm`配置文件`shiro.ini`

```ini
[users]
five = 1=23.
root = root
```

测试认证代码

```java
public class QuickStart {

    public static void main(String[] args) {
        // 创建安全管理器对象
        DefaultSecurityManager securityManager = new DefaultSecurityManager();

        // 给安全管理器设置 (域) realm
        securityManager.setRealm(new IniRealm("classpath:shiro.ini"));

        // 给 工具类 设置 安全管理器
        SecurityUtils.setSecurityManager(securityManager);

        // 关键对象 subject 当前用户
        Subject subject = SecurityUtils.getSubject();

        // 创建令牌
        UsernamePasswordToken token = new UsernamePasswordToken("five","1=23.");

        try{
            subject.login(token);
            System.out.println("是否认证: "+subject.isAuthenticated());
        }catch (UnknownAccountException e){		// 用户名不存在
            e.printStackTrace();
        }catch (IncorrectCredentialsException e){	// 密码错误
            e.printStackTrace();
        }

    }
}
```



## 自定义`Realm`

```java
public class MyRealm extends AuthorizingRealm {
    
    // 授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        return null;
    }


    // 认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        
        // token.getPrincipal() 获取用户名信息
        
        String account = "five";
        String password = "1=23.";
        
        if( account.equals(token.getPrincipal()) ){ 
            return new SimpleAuthenticationInfo(
                account,
                password,
                this.getName());
        }

        return null;
    }
}
```

测试

```java
public class MyRealmTest {

    public static void main(String[] args) {
        DefaultSecurityManager securityManager = new DefaultSecurityManager();

        // 使用自定义 Realm
        securityManager.setRealm(new MyRealm());

        SecurityUtils.setSecurityManager(securityManager);

        UsernamePasswordToken token = new UsernamePasswordToken("five","1=23.");

        Subject subject = SecurityUtils.getSubject();

        subject.login(token);

        System.out.println( subject.isAuthenticated() );
    }
}
```



## `MD5`加密

```java
public class MD5test {
    public static void main(String[] args) {
        String password = "";

        // MD5 加密
        password = new Md5Hash("1=23.").toHex();
        System.out.println(password);

        // MD5+盐 加密
        password = new Md5Hash("1=23.","AAAFive").toHex();
        System.out.println(password);

        // MD5+盐+哈希散列 加密
        password = new Md5Hash("1=23.","AAAFive", 1024).toHex();
        System.out.println(password);
    }
}
```

结果

```tex
e4260bc324db9c01759facd8a07e2c8b
4f72620a3cd96396018ecf4fc21785c2
fd5c3e20f38a2d40231c9eed48472c44
```



## 具有`MD5`加密的`Realm`

```java
public class MyRealm extends AuthorizingRealm {
    
    // 授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        return null;
    }


    // 认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        
        // token.getPrincipal() 获取用户名信息
        
        String account = "five";
        
        // MD5加密
        // String password = "e4260bc324db9c01759facd8a07e2c8b";
        
        // MD5+盐 加密
        // String password = "4f72620a3cd96396018ecf4fc21785c2";
        
        // MD5+盐+哈希散列 加密
        String password = "fd5c3e20f38a2d40231c9eed48472c44";
        
        // 盐
        String salt = "AAAFive";
        
        if( account.equals(token.getPrincipal()) ){ 
		
            // BatySource.Util.bytes("AAAFive") 设置 盐
            return new SimpleAuthenticationInfo(
                account,
                password,
                BatySource.Util.bytes(salt),
                this.getName()
            );
        }

        return null;
    }
}
```

测试

```java
package com.test;

import com.realm.MyRealm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.ByteSource;

public class MyRealmTest {

    public static void main(String[] args) {
        DefaultSecurityManager securityManager = new DefaultSecurityManager();

        // 创建自定义realm
        MyRealm realm = new MyRealm();

        // 创建哈希匹配器
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        
       	// 设置哈希匹配器使用md5算法
        credentialsMatcher.setHashAlgorithmName("md5");
        // 设置哈希匹配器的哈希散列次数
        credentialsMatcher.setHashIterations(1024);
        
        // 设置自定义realm使用哈希匹配器
        realm.setCredentialsMatcher(credentialsMatcher);
        
        securityManager.setRealm(realm);

        SecurityUtils.setSecurityManager(securityManager);

        UsernamePasswordToken token = new UsernamePasswordToken("five","1=23.");

        Subject subject = SecurityUtils.getSubject();

        subject.login(token);

        System.out.println( subject.isAuthenticated() );
    }
}
```



## 授权

```java
public class MyRealm extends AuthorizingRealm {
    
    // 授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();
        
        // 设置角色
        simpleAuthorizationInfo.addRole("admin");
        simpleAuthorizationInfo.addRoles(Arrays.asList("a","b","c"));

        // 设置许可		表:增删改查:实例
        simpleAuthorizationInfo.addStringPermission("user:find:1");
        return simpleAuthorizationInfo;
    }


    // 认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        
        // token.getPrincipal() 获取用户名信息
        
        String account = "five";
        
        // MD5+盐+哈希散列 加密
        String password = "fd5c3e20f38a2d40231c9eed48472c44";
        
        // 盐
        String salt = "AAAFive";
        
        if( account.equals(token.getPrincipal()) ){ 
		
            // BatySource.Util.bytes("AAAFive") 设置 盐
            return new SimpleAuthenticationInfo(
                account,
                password,
                ByteSource.Util.bytes(salt),
                this.getName()
            );
        }

        return null;
    }
}
```

测试

```java
public class AuthorizationTest {
    public static void main(String[] args) {
        DefaultSecurityManager securityManager = new DefaultSecurityManager();

        MyRealm realm = new MyRealm();

        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        credentialsMatcher.setHashAlgorithmName("md5");
        credentialsMatcher.setHashIterations(1024);
        realm.setCredentialsMatcher(credentialsMatcher);
        securityManager.setRealm(realm);

        SecurityUtils.setSecurityManager(securityManager);

        UsernamePasswordToken token = new UsernamePasswordToken("five","1=23.");

        Subject subject = SecurityUtils.getSubject();

        subject.login(token);
        
        // 是否具有角色
        subject.hasRole("admin");
        
        // 是否具有授权
        subject.isPermitted("user:find:1");
    }
}

```

