
## 权限管理

> 基本上涉及到用户参与的系统都要进行权限管理，权限管理属于系统安全的范畴，权限管理实现 `对用户访问系统的控制`，按照 `安全规则` 或者 `安全策略` 控制用户、`可以访问而且只能访问自己波授权的资源。`

权限管理主要包括两部分认证和授权

### 认证授权

认证: 就是判断一个用户是否为合法用户的处理过程.
	如: 用户登录,判断是本平台的用户吗? 
	
授权: 即访问控制，控制谁能访问哪些资源
	如: 用户认证后,判断用户是否可以访问图片资源

### 解决方案

和其他领域不同，在 Java 企业级开发中，安全管理框架非常少，目前比较常见的就是：

-   Shiro
    -   Shiro 本身是一个老牌的安全管理框架，有着众多的优点，例如轻量、简单、易于集成、可以在JavaSE环境中使用等。不过，在微服务时代，Shiro 就显得力不从心了，在微服务面前和扩展方面，无法充分展示自己的优势。

-   开发者自定义
    也有很多公司选择自定义权限，即自己开发权限管理。但是一个系统的安全，不仅仅是登录和权限控制这么简单，我们还要考虑种各样可能存在的网络政击以及防彻策略，从这个角度来说，开发者白己实现安全管理也并非是一件容易的事情，只有大公司才有足够的人力物力去支持这件事情。

-   Spring Security
    - Spring Security,作为spring 家族的一员，在和 Spring 家族的其他成员如 Spring Boot Spring Clond等进行整合时，具有其他框架无可比拟的优势，同时对 OAuth2 有着良好的支持，再加上Spring Cloud对 Spring Security的不断加持（如推出 Spring Cloud Security )，让 Spring Securiy 不知不觉中成为微服务项目的首选安全管理方案。

## 整体架构

> 在的架构设计中，`认证`和`授权` 是分开的，无论使用什么样的认证方式。都不会影响授权，这是两个独立的存在，这种独立带来的好处之一，就是可以非常方便地整合一些外部的解决方案。

![](http://img.zfqit.top/img/202302091712491.png)

### 认证

#### AuthenticationManager

在Spring Security中认证是由`AuthenticationManager`接口来负责的，接口定义为：

```java
public interface AuthenticationManager { Authentication authenticate(Authentication authentication) throws AuthenticationException; }
```

-   返回 Authentication 表示认证成功
-   返回 AuthenticationException 异常，表示认证失败。

> AuthenticationManager 主要实现类为 ProviderManager，在 ProviderManager 中管理了众多 AuthenticationProvider 实例。在一次完整的认证流程中，Spring Security 允许存在多个 AuthenticationProvider ，用来实现多种认证方式，这些 AuthenticationProvider 都是由 ProviderManager 进行统一管理的。

相当于 `ProviderManager` 是管理验证方式,而 `AuthenticationProvider` 实例是具体验证方式, 如  账号密码, 手机号, 第三方登录等等

![](http://img.zfqit.top/img/202302091721152.png)

#### Authentication

认证以及认证成功的信息主要是由 Authentication 的实现类进行保存的，其接口定义为：

```java
public interface Authentication extends Principal, Serializable { 
	Collection<? extends GrantedAuthority> getAuthorities(); 
	Object getCredentials(); 
	Object getDetails(); Object getPrincipal(); 
	boolean isAuthenticated(); 
	void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException;
}
```

-   getAuthorities 获取用户权限信息
-   getCredentials 获取用户凭证信息，一般指密码
-   getDetails 获取用户详细信息
-   getPrincipal 获取用户身份信息，用户名、用户对象等
-   isAuthenticated 用户是否认证成功

#### SecurityContextHolder

SecurityContextHolder 用来获取登录之后用户信息。Spring Security 会将登录用户数据保存在 Session 中。但是，为了使用方便,Spring Security在此基础上还做了一些改进，其中最主要的一个变化就是线程绑定。当用户登录成功后,Spring Security 会将登录成功的用户信息保存到 SecurityContextHolder 中。SecurityContextHolder 中的数据保存默认是通过ThreadLocal 来实现的，使用 ThreadLocal 创建的变量只能被当前线程访问，不能被其他线程访问和修改，也就是用户数据和请求线程绑定在一起。当登录请求处理完毕后，Spring Security 会将SecurityContextHolder 中的数据拿出来保存到 Session 中，同时将 SecurityContexHolder 中的数据清空。以后每当有请求到来时，Spring Security 就会先从 Session 中取出用户登录数据，保存到 SecurityContextHolder 中，方便在该请求的后续处理过程中使用，同时在请求结束时将 SecurityContextHolder 中的数据拿出来保存到 Session 中，然后将 Security SecurityContextHolder 中的数据清空。这一策略非常方便用户在 Controller、Service 层以及任何代码中获取当前登录用户数据。

### 授权

当完成认证后，接下米就是授权了。在 Spring Security 的授权体系中，有两个关键接口，

#### AccessDecisionManager

> AccessDecisionManager (访问决策管理器)，用来决定此次访问是否被允许。

![](http://img.zfqit.top/img/202302091725710.png)

#### AccessDecisionVoter

> AccessDecisionVoter (访问决定投票器)，投票器会检查用户是否具备应有的角色，进而投出赞成、反对或者弃权票。

![](http://img.zfqit.top/img/202302091725090.png)

AccesDecisionVoter 和 AccessDecisionManager 都有众多的实现类，在 AccessDecisionManager 中会换个遍历 AccessDecisionVoter，进而决定是否允许用户访问，因而 AaccesDecisionVoter 和 AccessDecisionManager 两者的关系类似于 AuthenticationProvider 和 ProviderManager 的关系。

#### ConfigAttribute

> ConfigAttribute，用来保存授权时的角色信息

![](http://img.zfqit.top/img/202302091726906.png)

在 Spring Security 中，用户请求一个资源(通常是一个接口或者一个 Java 方法)需要的角色会被封装成一个 ConfigAttribute 对象，在 ConfigAttribute 中只有一个 getAttribute方法，该方法返回一个 String 字符串，就是角色的名称。一般来说，角色名称都带有一个 `ROLE_` 前缀，投票器 AccessDecisionVoter 所做的事情，其实就是比较用户所具各的角色和请求某个 资源所需的 ConfigAtuibute 之间的关系。