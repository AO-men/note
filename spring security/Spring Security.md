# Spring Security

## 权限管理

基本上涉及到用户参与的系统都要进行权限管理，权限管理属于系统安全范畴，权限管理实现`对用户访问系统的控制`，按照`安全规则`或者`安全策略`控制用户`可以访问而且只能访问自己授权的资源`。

权限管理包括`用户认证`和`授权`两部分，简称`认证授权`。对于需要访问控制的资源用户首先经过身份认证，认证通过后用户具有该资源的访问权限方可访问。

### 认证

**`身份认证`** ，判断用户是否为合法用户的处理过程。

### 授权

**`授权`**，即访问控制，控制谁能访问哪些资源。主体进行身份认证后需要分配权限方可访问系统的资源，对于某些资源没有权限是无法访问的。



## 简介

### 官方定义

- https://spring.io/projects/spring-security

Spring Security是一个功能强大且高度可定制的身份验证和访问控制框架。它是保护基于Spring的应用程序的事实标准。
Spring Security是一个专注于为Java应用程序提供身份验证和授权的框架。与所有Spring项目一样，Spring Security的真正威力在于它可以多么容易地扩展以满足定制需求

> spring security是一个功能强大、可高度定制的`身份验证`和`访问控制`框架。



## 整体架构

`spring security` 的架构设计中，`认证`<Authentication>和`授权`<Authorization>是分开的，无论使用什么样的认证方式。都不会影响授权，这是两个独立的存在。

![image-20220126213500385](D:\软件\typora\笔记资源\图片资源\image-20220126213500385.png)

### 认证

#### AuthenticationManager

在 spring security 中认证是由`AuthenticationManager`接口负责的，接口定义为：

```java
public interface AuthenticationManager {
    Authentication authenticate(Authentication var1) throws AuthenticationException;
}

```

- 返回`Authentication`标识认证成功
- 返回`AuthenticationException`异常，标识认证失败

`AuthenticationManager`主要实现类为`ProviderManager`，在`ProviderManager`中管理了众多`AuthenticationProvider`实例。在一次完整的认证流程中，`Spring Security`允许存在多个`AuthenticationProvider`，用来实现多种认证方式，这些`AuthenticationProvider`都是由`ProviderManager`进行统一管理的。



#### Authentication

`认证`以及`认证成功`的信息主要是由`Authencation`的实现类进行保存的，其接口定义为：

```
public interface Authentication extends Principal, Serializable {
	// 获取用户权限信息
    Collection<? extends GrantedAuthority> getAuthorities();
	// 获取用户凭证信息 一般指密码
    Object getCredentials();
	// 用户用户详细信息
    Object getDetails();
	// 获取用户身份信息 用户名、用户对象
    Object getPrincipal();
	// 用户是否认证成功
    boolean isAuthenticated();
    
    void setAuthenticated(boolean var1) throws IllegalArgumentException;
}
```

#### SecurityContextHolder

`SecurityContextHolder`用户获取登录之后用户信息，`Spring Security`会将登录用户数据保存在`Session`。但是为了使用方便，`Spring Security`在此基础上还早了一些改进。

默认是将SecurityContext存储在threadlocal中，可能是spring考虑到目前大多数为BS应用，一个应用同时可能有多个使用者，每个使用者又对应不同的安全上下，Security Context Holder为了保存这些安全上下文。缺省情况下，使用了ThreadLocal机制来保存每个使用者的安全上下文。因为缺省情况下根据Servlet规范，一个Servlet request的处理不管经历了多少个Filter，自始至终都由同一个线程来完成。这样就很好的保证了其安全性。但是当我们开发的是一个CS本地应用的时候，这种模式就不太适用了。spring早早的就考虑到了这种情况，这个时候我们就可以设置为Global模式仅使用一个变量来存储SecurityContext。比如还有其他的一些应用会有自己的线程创建，并且希望这些新建线程也能使用创建者的安全上下文。这种效果，我们就可以通过将SecurityContextHolder配置成MODE_INHERITABLETHREADLOCAL策略达到。



### 认证

#### AccessDecisionManager

> AccessDecisionManager（访问决策管理器），用来决定此次访问时候允许。

```java
public interface AccessDecisionManager {
    void decide(Authentication var1, Object var2, Collection<ConfigAttribute> var3) throws AccessDeniedException, InsufficientAuthenticationException;

    boolean supports(ConfigAttribute var1);

    boolean supports(Class<?> var1);
}
```

#### AccessDecisionVoter

> AccessDecisionVoter（访问决定投票器），投票器会检查用户是否具备应有的角色，进而投出赞成、反对或者弃权票。

```java
public interface AccessDecisionVoter<S> {
    int ACCESS_GRANTED = 1;
    int ACCESS_ABSTAIN = 0;
    int ACCESS_DENIED = -1;

    boolean supports(ConfigAttribute var1);

    boolean supports(Class<?> var1);

    int vote(Authentication var1, S var2, Collection<ConfigAttribute> var3);
}
```

`AccessDecisionVoter`和`AccessDecisionManager`都有众多的实现类，在`AccessDecisionManager`中会挨个遍历`AccessDecisionVoter`，进而决定是否允许用户访问，因为`AccessDecisionVoter`和`AccessDecisionManager`两者的关系类似于`AuthenticationManager`和`ProivderManager`

#### ConfigAttribute

> ConfigAttribute 用来保存授权时的角色信息

```java
public interface ConfigAttribute extends Serializable {
    // @return 角色名称
    String getAttribute();
}
```

在`spring security`中，用户请求一个资源（通常是一个接口或者一个java方法）需要的角色会封装成一个`ConfigAttribute`对象，在`ConfigAttribute`中有一个`getAttribute`方法，该方法返回一个字符串，就是角色的名称。一般来说，角色名称都带有`ROLE_`前缀，投票器`AccessDecisionVoter`所做的事情，其实就是比较用户所具有的角色和请求某个资源所需的`ConfigAttribute`之间的关系。

## 环境搭建

- spring boot
- spring security

### 创建项目

```markdown
# 1. 创建spring boot 应用
```



```markdown
# 2. 创建controller
```





