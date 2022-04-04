# SpringSecurity安全框架
## 1. SpringSecurity简介
### 1.1 安全框架概述
- 安全框架是解决系统安全问题的框架。如果没有安全框架，我们需要手动处理每个资源的访问控制，非常麻烦。使用安全框架，我们可以通过配置的方式实现对资源的访问限制。
- 关于安全方面的两个主要区域是“认证”和“授权”（或者访问控制），一般来说，Web 应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分，这两点也是 Spring Security 重要核心功能。
  - 认证:验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认证过程。通俗点说就是系统认为用户是否能登录.
  - 授权:验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。通俗点讲就是系统判断用户是否有权限去做某些事情。

### 1.2 Shiro安全框架
- Apache 旗下的轻量级权限控制框架。一个功能强大且易于使用的Java安全框架,提供了认证,授权,加密,和会话管理。
- 特点:
  - 轻量级:Shiro 主张的理念是把复杂的事情变简单。针对对性能有更高要求的互联网应用有更好表现。
  - 通用性:
    - 好处：不局限于 Web 环境，可以脱离 Web 环境使用。
    - 缺陷：在 Web 环境下一些特定的需求需要手动编写代码定制。
### 1.3 SpringSecurity安全框架
- Spring Security：Spring Security是一个高度自定义的安全框架。利用 Spring IoC/DI和AOP功能，为系统提供了声明式安全访问控制功能，减少了为系统安全而编写大量重复代码的工作。使用 Spring Secruity 的原因有很多，但大部分都是发现了 javaEE的 Servlet 规范或 EJB 规范中的安全功能缺乏典型企业应用场景。同时认识到他们在 WAR 或 EAR 级别无法移植。因此如果你更换服务器环境，还有大量工作去重新配置你的应用程序。使用 Spring Security解决了这些问题，也为你提供许多其他有用的、可定制的安全功能。正如你可能知道的两个应用程序的两个主要区域是“认证”和“授权”（或者访问控制）。这两点也是 Spring Security 重要核心功能。“认证”，是建立一个他声明的主体的过程（一个“主体”一般是指用户，设备或一些可以在你的应用程序中执行动作的其他系统），通俗点说就是系统认为用户是否能登录。“授权”指确定一个主体是否允许在你的应用程序执行一个动作的过程。通俗点讲就是系统判断用户是否有权限去做某些事情。
- SpringSecurity历史:
  Spring Security 以“The Acegi Secutity System for Spring”的名字始于2003年年底。其前身为 acegi 项目。起因是 Spring 开发者邮件列表中一个问题，有人提问是否考虑提供一个基于 Spring 的安全实现。限制于时间问题，开发出了一个简单的安全实现，但是并没有深入研究。几周后，Spring 社区中其他成员同样询问了安全问题，代码提供给了这些人。2004 年 1 月份已经有 20 人左右使用这个项目。随着更多人的加入，在 2004 年 3 月左右在 sourceforge 中建立了一个项目。在最开始并没有认证模块，所有的认证功能都是依赖容器完成的，而 acegi 则注重授权。但是随着更多人的使用，基于容器的认证就显现出了不足。acegi 中也加入了认证功能。大约 1 年后 acegi 成为 Spring子项目。在 2006 年 5 月发布了 acegi 1.0.0 版本。2007 年底 acegi 更名为Spring Security
- SpringSecurity 特点：
   - 和 Spring 无缝整合。
   - 全面的权限控制。
   - 专门为 Web 开发而设计。
       - 旧版本不能脱离 Web 环境使用。
       - 新版本对整个框架进行了分层抽取，分成了核心模块和 Web 模块。单独引入核心模块就可以脱离 Web 环境。
   - 重量级。
## 2. SpringSecurity快速开始
- 在SpringBoot项目中使用SpringSecurity我们只需要引入依赖即可实现入门案例。
~~~java
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
~~~
- 引入依赖后我们在尝试去访问之前的接口就会自动跳转到一个SpringSecurity的默认登陆页面，默认用户名是user,密码会输出在控制台。	必须登陆之后才能对接口进行访问。

## 3. SpringSecurity基本原理
### 3.1 SpringSecurity本质
- SpringSecurity的原理其实就是一个过滤器链，内部包含了提供各种功能的过滤器。
~~~java
org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter
org.springframework.security.web.context.SecurityContextPersistenceFilter
org.springframework.security.web.header.HeaderWriterFilter
org.springframework.security.web.csrf.CsrfFilter
org.springframework.security.web.authentication.logout.LogoutFilter
org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter
org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter
org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter
org.springframework.security.web.savedrequest.RequestCacheAwareFilter
org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter
org.springframework.security.web.authentication.AnonymousAuthenticationFilter
org.springframework.security.web.session.SessionManagementFilter
org.springframework.security.web.access.ExceptionTranslationFilter
org.springframework.security.web.access.intercept.FilterSecurityInterceptor
~~~
![image-20211214145824903](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/image-20211214145824903.png)
1. WebAsyncManagerIntegrationFilter：将 Security 上下文与 Spring Web 中用于处理异步请求映射的 WebAsyncManager 进行集成。
2. SecurityContextPersistenceFilter：在每次请求处理之前将该请求相关的安全上下文信息加载到 SecurityContextHolder 中，然后在该次请求处理完成之后，将SecurityContextHolder 中关于这次请求的信息存储到一个“仓储”中，然后将SecurityContextHolder 中的信息清除，例如在 Session 中维护一个用户的安全信息就是这个过滤器处理的。
3. HeaderWriterFilter：用于将头信息加入响应中。
4. CsrfFilter：用于处理跨站请求伪造。
5. LogoutFilter：用于处理退出登录。
6. UsernamePasswordAuthenticationFilter：用于处理基于表单的登录请求，从表单中获取用户名和密码。默认情况下处理来自 /login 的请求。从表单中获取用户名和密码时，默认使用的表单 name 值为 username 和 password，这两个值可以通过设置这个过滤器的 usernameParameter 和 passwordParameter 两个参数的值进行修改。
7. DefaultLoginPageGeneratingFilter：如果没有配置登录页面，那系统初始化时就会配置这个过滤器，并且用于在需要进行登录时生成一个登录表单页面。
8. BasicAuthenticationFilter：检测和处理 http basic 认证。
9. RequestCacheAwareFilter：用来处理请求的缓存。
10. SecurityContextHolderAwareRequestFilter：主要是包装请求对象 request。
11. AnonymousAuthenticationFilter：检测 SecurityContextHolder 中是否存在Authentication 对象，如果不存在为其提供一个匿名 Authentication。
12. SessionManagementFilter：管理 session 的过滤器
13. ExceptionTranslationFilter：处理 AccessDeniedException 和AuthenticationException 异常。
14. FilterSecurityInterceptor：可以看做过滤器链的出口。
15. RememberMeAuthenticationFilter：当用户没有登录而直接访问资源时, 从 cookie 里找出用户的信息, 如果 Spring Security 能够识别出用户提供的 remember me cookie, 用户将不必填写用户名和密码, 而是直接登录进入系统，该过滤器默认不开启。

- 重点看三个过滤器：
  1. FilterSecurityInterceptor
  2. ExceptionTranslationFilter
  3. UsernamePasswordAuthenticationFilter

![image-20211214144425527](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/image-20211214144425527.png)

1. FilterSecurityInterceptor:是一个方法级的权限过滤器, 基本位于过滤链的最底部,负责权限校验的过滤器。
   ![20220401220125](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220401220125.png)
   
   super.beforeInvocation(fi) 表示查看之前的 filter 是否通过。

   fi.getChain().doFilter(fi.getRequest(), fi.getResponse());表示真正的调用后台的服务。
2. ExceptionTranslationFilter:：是个异常过滤器，用来处理在认证授权过程中抛出的异常处理。过滤器链中抛出的任何AccessDeniedException和AuthenticationException。
   ![20220401220228](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220401220228.png)
3. UsernamePasswordAuthenticationFilter ：对/login 的 POST 请求做拦截，校验表单中用户名，密码。负责处理我们在登陆页面填写了用户名密码后的登陆请求。入门案例的认证工作主要有它负责。
   
   ![20220401220240](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220401220240.png)

### 3.2 SpringSecurity过滤器启动原理
1. 使用springsecurity配置过滤器（DelegatingFilterProxy）；
2. 该类的doFilter方法会通过initDelegate(wac)先获取到过滤器bean(FilterChainProxy)，通过他去初始化其他的filter（delegate.init(this.getFilterConfig());）；
3. FilterChainProxy的doFilter方法会通过getFilters(fwRequest)方法获取到WebApplicationContext配置中的所有配置过的的filter；
4. 紧接着开始往后加载其他的filter
   
![20220401220736](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220401220736.png)

## 4. SpringSecurity认证
### 4.1 登陆校验流程
![image-20211215093906256](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/image-20211215093906256.png)
### 4.2 认证流程详解
![image-20211214150315167](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/image-20211214150315167.png)
1. Authentication接口: 它的实现类，表示当前访问系统的用户，封装了用户相关信息。
2. AuthenticationManager接口：定义了认证Authentication的方法 
3. UserDetailsService接口：加载用户特定数据的核心接口。里面定义了一个根据用户名查询用户信息的方法。
4. UserDetails接口：提供核心用户信息。通过UserDetailsService根据用户名获取处理的用户信息要封装成UserDetails对象返回。然后将这些信息封装到Authentication对象中。
- 源码解析
   1. Authentication接口的实现类是UsernamePasswordAuthenticationToken类,它除了封装了用户信息还标记了当前用户是否通过认证,它通过构造方法是否传入权限集合来判断是否通过认证.
   ![](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/微信图片编辑_20220404141304.jpg)
   2. AuthenticationManager接口的实现类是ProviderManager类,而ProviderManager类中的authenticate方法就是用来认证的用户,这个方法中又调用了DaoAuthenticationProvider类的authenticate方法进行认证
   ![](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/微信图片编辑_20220404141602.jpg)
   1. DaoAuthenticationProvider类是AbstractUserDetailsAuthenticationProvider的子类,DaoAuthenticationProvider类中的authenticate方法实际上是调用父类的authenticate方法
   ![](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/微信图片编辑_20220404141703.jpg)
   父类的authenticate方法中又调用了一个抽象方法retrieveUser
   ![](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/微信图片编辑_20220404141748.jpg)
   子类 DaoAuthenticationProvider类实现了这个抽象方法,这个方法中调用了UserDetailsService类的loadByUsername方法获取数据库中的用户信息,然后和传入的用户信息进行比对
   ![](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/微信图片编辑_20220404141904.jpg)
   当认证成功后,返回一个通过参数含权限集合的构造方法构造的Authentication类,即 Authentication类中super.setAuthenticated(true);
   ![](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/微信图片编辑_20220404141854.jpg)
### 4.3 认证代码实现
#### 4.3.1 思路分析
- 登录
	- 自定义登录接口  
      - 调用ProviderManager的方法进行认证 如果认证通过生成jwt
	  - 把用户信息存入redis中
	- 自定义UserDetailsService 
      - 在这个实现类中去查询数据库

- 校验：
	- 定义Jwt认证过滤器
	    - 获取token
      - 解析token获取其中的userid
      - 从redis中获取用户信息
       - 存入SecurityContextHolder
#### 4.3.2 代码准备工作
1. 添加依赖
~~~java
<!--redis依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <!--fastjson依赖-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.33</version>
        </dependency>
        <!--jwt依赖-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.0</version>
        </dependency>
~~~
2. 添加Redis相关配置
~~~java
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.serializer.SerializerFeature;
import com.fasterxml.jackson.databind.JavaType;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.type.TypeFactory;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.SerializationException;
import com.alibaba.fastjson.parser.ParserConfig;
import org.springframework.util.Assert;
import java.nio.charset.Charset;

/**
 * Redis使用FastJson序列化
 * 
 * @author sg
 */
public class FastJsonRedisSerializer<T> implements RedisSerializer<T>
{

    public static final Charset DEFAULT_CHARSET = Charset.forName("UTF-8");

    private Class<T> clazz;

    static
    {
        ParserConfig.getGlobalInstance().setAutoTypeSupport(true);
    }

    public FastJsonRedisSerializer(Class<T> clazz)
    {
        super();
        this.clazz = clazz;
    }

    @Override
    public byte[] serialize(T t) throws SerializationException
    {
        if (t == null)
        {
            return new byte[0];
        }
        return JSON.toJSONString(t, SerializerFeature.WriteClassName).getBytes(DEFAULT_CHARSET);
    }

    @Override
    public T deserialize(byte[] bytes) throws SerializationException
    {
        if (bytes == null || bytes.length <= 0)
        {
            return null;
        }
        String str = new String(bytes, DEFAULT_CHARSET);

        return JSON.parseObject(str, clazz);
    }


    protected JavaType getJavaType(Class<?> clazz)
    {
        return TypeFactory.defaultInstance().constructType(clazz);
    }
}
~~~

~~~java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig {

    @Bean
    @SuppressWarnings(value = { "unchecked", "rawtypes" })
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory connectionFactory)
    {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);

        FastJsonRedisSerializer serializer = new FastJsonRedisSerializer(Object.class);

        // 使用StringRedisSerializer来序列化和反序列化redis的key值
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(serializer);

        // Hash的key也采用StringRedisSerializer的序列化方式
        template.setHashKeySerializer(new StringRedisSerializer());
        template.setHashValueSerializer(serializer);

        template.afterPropertiesSet();
        return template;
    }
}
~~~

3. 响应类
~~~java
import com.fasterxml.jackson.annotation.JsonInclude;

/**
 * @Author 三更  B站： https://space.bilibili.com/663528522
 */
@JsonInclude(JsonInclude.Include.NON_NULL)
public class ResponseResult<T> {
    /**
     * 状态码
     */
    private Integer code;
    /**
     * 提示信息，如果有错误时，前端可以获取该字段进行提示
     */
    private String msg;
    /**
     * 查询到的结果数据，
     */
    private T data;

    public ResponseResult(Integer code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    public ResponseResult(Integer code, T data) {
        this.code = code;
        this.data = data;
    }

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public T getData() {
        return data;
    }

    public void setData(T data) {
        this.data = data;
    }

    public ResponseResult(Integer code, String msg, T data) {
        this.code = code;
        this.msg = msg;
        this.data = data;
    }
}
~~~

4. 工具类
~~~java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.JwtBuilder;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64;
import java.util.Date;
import java.util.UUID;

/**
 * JWT工具类
 */
public class JwtUtil {

    //有效期为
    public static final Long JWT_TTL = 60 * 60 *1000L;// 60 * 60 *1000  一个小时
    //设置秘钥明文
    public static final String JWT_KEY = "sangeng";

    public static String getUUID(){
        String token = UUID.randomUUID().toString().replaceAll("-", "");
        return token;
    }
    
    /**
     * 生成jtw
     * @param subject token中要存放的数据（json格式）
     * @return
     */
    public static String createJWT(String subject) {
        JwtBuilder builder = getJwtBuilder(subject, null, getUUID());// 设置过期时间
        return builder.compact();
    }

    /**
     * 生成jtw
     * @param subject token中要存放的数据（json格式）
     * @param ttlMillis token超时时间
     * @return
     */
    public static String createJWT(String subject, Long ttlMillis) {
        JwtBuilder builder = getJwtBuilder(subject, ttlMillis, getUUID());// 设置过期时间
        return builder.compact();
    }

    private static JwtBuilder getJwtBuilder(String subject, Long ttlMillis, String uuid) {
        SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;
        SecretKey secretKey = generalKey();
        long nowMillis = System.currentTimeMillis();
        Date now = new Date(nowMillis);
        if(ttlMillis==null){
            ttlMillis=JwtUtil.JWT_TTL;
        }
        long expMillis = nowMillis + ttlMillis;
        Date expDate = new Date(expMillis);
        return Jwts.builder()
                .setId(uuid)              //唯一的ID
                .setSubject(subject)   // 主题  可以是JSON数据
                .setIssuer("sg")     // 签发者
                .setIssuedAt(now)      // 签发时间
                .signWith(signatureAlgorithm, secretKey) //使用HS256对称加密算法签名, 第二个参数为秘钥
                .setExpiration(expDate);
    }

    /**
     * 创建token
     * @param id
     * @param subject
     * @param ttlMillis
     * @return
     */
    public static String createJWT(String id, String subject, Long ttlMillis) {
        JwtBuilder builder = getJwtBuilder(subject, ttlMillis, id);// 设置过期时间
        return builder.compact();
    }

    public static void main(String[] args) throws Exception {
        String token = "eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiJjYWM2ZDVhZi1mNjVlLTQ0MDAtYjcxMi0zYWEwOGIyOTIwYjQiLCJzdWIiOiJzZyIsImlzcyI6InNnIiwiaWF0IjoxNjM4MTA2NzEyLCJleHAiOjE2MzgxMTAzMTJ9.JVsSbkP94wuczb4QryQbAke3ysBDIL5ou8fWsbt_ebg";
        Claims claims = parseJWT(token);
        System.out.println(claims);
    }

    /**
     * 生成加密后的秘钥 secretKey
     * @return
     */
    public static SecretKey generalKey() {
        byte[] encodedKey = Base64.getDecoder().decode(JwtUtil.JWT_KEY);
        SecretKey key = new SecretKeySpec(encodedKey, 0, encodedKey.length, "AES");
        return key;
    }
    
    /**
     * 解析
     *
     * @param jwt
     * @return
     * @throws Exception
     */
    public static Claims parseJWT(String jwt) throws Exception {
        SecretKey secretKey = generalKey();
        return Jwts.parser()
                .setSigningKey(secretKey)
                .parseClaimsJws(jwt)
                .getBody();
    }
}
~~~

~~~java
import java.util.*;
import java.util.concurrent.TimeUnit;

@SuppressWarnings(value = { "unchecked", "rawtypes" })
@Component
public class RedisCache
{
    @Autowired
    public RedisTemplate redisTemplate;

    /**
     * 缓存基本的对象，Integer、String、实体类等
     *
     * @param key 缓存的键值
     * @param value 缓存的值
     */
    public <T> void setCacheObject(final String key, final T value)
    {
        redisTemplate.opsForValue().set(key, value);
    }

    /**
     * 缓存基本的对象，Integer、String、实体类等
     *
     * @param key 缓存的键值
     * @param value 缓存的值
     * @param timeout 时间
     * @param timeUnit 时间颗粒度
     */
    public <T> void setCacheObject(final String key, final T value, final Integer timeout, final TimeUnit timeUnit)
    {
        redisTemplate.opsForValue().set(key, value, timeout, timeUnit);
    }

    /**
     * 设置有效时间
     *
     * @param key Redis键
     * @param timeout 超时时间
     * @return true=设置成功；false=设置失败
     */
    public boolean expire(final String key, final long timeout)
    {
        return expire(key, timeout, TimeUnit.SECONDS);
    }

    /**
     * 设置有效时间
     *
     * @param key Redis键
     * @param timeout 超时时间
     * @param unit 时间单位
     * @return true=设置成功；false=设置失败
     */
    public boolean expire(final String key, final long timeout, final TimeUnit unit)
    {
        return redisTemplate.expire(key, timeout, unit);
    }

    /**
     * 获得缓存的基本对象。
     *
     * @param key 缓存键值
     * @return 缓存键值对应的数据
     */
    public <T> T getCacheObject(final String key)
    {
        ValueOperations<String, T> operation = redisTemplate.opsForValue();
        return operation.get(key);
    }

    /**
     * 删除单个对象
     *
     * @param key
     */
    public boolean deleteObject(final String key)
    {
        return redisTemplate.delete(key);
    }

    /**
     * 删除集合对象
     *
     * @param collection 多个对象
     * @return
     */
    public long deleteObject(final Collection collection)
    {
        return redisTemplate.delete(collection);
    }

    /**
     * 缓存List数据
     *
     * @param key 缓存的键值
     * @param dataList 待缓存的List数据
     * @return 缓存的对象
     */
    public <T> long setCacheList(final String key, final List<T> dataList)
    {
        Long count = redisTemplate.opsForList().rightPushAll(key, dataList);
        return count == null ? 0 : count;
    }

    /**
     * 获得缓存的list对象
     *
     * @param key 缓存的键值
     * @return 缓存键值对应的数据
     */
    public <T> List<T> getCacheList(final String key)
    {
        return redisTemplate.opsForList().range(key, 0, -1);
    }

    /**
     * 缓存Set
     *
     * @param key 缓存键值
     * @param dataSet 缓存的数据
     * @return 缓存数据的对象
     */
    public <T> BoundSetOperations<String, T> setCacheSet(final String key, final Set<T> dataSet)
    {
        BoundSetOperations<String, T> setOperation = redisTemplate.boundSetOps(key);
        Iterator<T> it = dataSet.iterator();
        while (it.hasNext())
        {
            setOperation.add(it.next());
        }
        return setOperation;
    }

    /**
     * 获得缓存的set
     *
     * @param key
     * @return
     */
    public <T> Set<T> getCacheSet(final String key)
    {
        return redisTemplate.opsForSet().members(key);
    }

    /**
     * 缓存Map
     *
     * @param key
     * @param dataMap
     */
    public <T> void setCacheMap(final String key, final Map<String, T> dataMap)
    {
        if (dataMap != null) {
            redisTemplate.opsForHash().putAll(key, dataMap);
        }
    }

    /**
     * 获得缓存的Map
     *
     * @param key
     * @return
     */
    public <T> Map<String, T> getCacheMap(final String key)
    {
        return redisTemplate.opsForHash().entries(key);
    }

    /**
     * 往Hash中存入数据
     *
     * @param key Redis键
     * @param hKey Hash键
     * @param value 值
     */
    public <T> void setCacheMapValue(final String key, final String hKey, final T value)
    {
        redisTemplate.opsForHash().put(key, hKey, value);
    }

    /**
     * 获取Hash中的数据
     *
     * @param key Redis键
     * @param hKey Hash键
     * @return Hash中的对象
     */
    public <T> T getCacheMapValue(final String key, final String hKey)
    {
        HashOperations<String, String, T> opsForHash = redisTemplate.opsForHash();
        return opsForHash.get(key, hKey);
    }

    /**
     * 删除Hash中的数据
     * 
     * @param key
     * @param hkey
     */
    public void delCacheMapValue(final String key, final String hkey)
    {
        HashOperations hashOperations = redisTemplate.opsForHash();
        hashOperations.delete(key, hkey);
    }

    /**
     * 获取多个Hash中的数据
     *
     * @param key Redis键
     * @param hKeys Hash键集合
     * @return Hash对象集合
     */
    public <T> List<T> getMultiCacheMapValue(final String key, final Collection<Object> hKeys)
    {
        return redisTemplate.opsForHash().multiGet(key, hKeys);
    }

    /**
     * 获得缓存的基本对象列表
     *
     * @param pattern 字符串前缀
     * @return 对象列表
     */
    public Collection<String> keys(final String pattern)
    {
        return redisTemplate.keys(pattern);
    }
}
~~~

~~~java
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class WebUtils
{
    /**
     * 将字符串渲染到客户端
     * 
     * @param response 渲染对象
     * @param string 待渲染的字符串
     * @return null
     */
    public static String renderString(HttpServletResponse response, String string) {
        try
        {
            response.setStatus(200);
            response.setContentType("application/json");
            response.setCharacterEncoding("utf-8");
            response.getWriter().print(string);
        }
        catch (IOException e)
        {
            e.printStackTrace();
        }
        return null;
    }
}
~~~
5. 实体类
~~~java
import java.io.Serializable;
import java.util.Date;


/**
 * 用户表(User)实体类
 *
 * @author 三更
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User implements Serializable {
    private static final long serialVersionUID = -40356785423868312L;
    
    /**
    * 主键
    */
    private Long id;
    /**
    * 用户名
    */
    private String userName;
    /**
    * 昵称
    */
    private String nickName;
    /**
    * 密码
    */
    private String password;
    /**
    * 账号状态（0正常 1停用）
    */
    private String status;
    /**
    * 邮箱
    */
    private String email;
    /**
    * 手机号
    */
    private String phonenumber;
    /**
    * 用户性别（0男，1女，2未知）
    */
    private String sex;
    /**
    * 头像
    */
    private String avatar;
    /**
    * 用户类型（0管理员，1普通用户）
    */
    private String userType;
    /**
    * 创建人的用户id
    */
    private Long createBy;
    /**
    * 创建时间
    */
    private Date createTime;
    /**
    * 更新人
    */
    private Long updateBy;
    /**
    * 更新时间
    */
    private Date updateTime;
    /**
    * 删除标志（0代表未删除，1代表已删除）
    */
    private Integer delFlag;
}
~~~

#### 4.3.3 数据库准备工作
- 创建用户表
~~~java
CREATE TABLE `sys_user` (
  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `user_name` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '用户名',
  `nick_name` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '昵称',
  `password` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '密码',
  `status` CHAR(1) DEFAULT '0' COMMENT '账号状态（0正常 1停用）',
  `email` VARCHAR(64) DEFAULT NULL COMMENT '邮箱',
  `phonenumber` VARCHAR(32) DEFAULT NULL COMMENT '手机号',
  `sex` CHAR(1) DEFAULT NULL COMMENT '用户性别（0男，1女，2未知）',
  `avatar` VARCHAR(128) DEFAULT NULL COMMENT '头像',
  `user_type` CHAR(1) NOT NULL DEFAULT '1' COMMENT '用户类型（0管理员，1普通用户）',
  `create_by` BIGINT(20) DEFAULT NULL COMMENT '创建人的用户id',
  `create_time` DATETIME DEFAULT NULL COMMENT '创建时间',
  `update_by` BIGINT(20) DEFAULT NULL COMMENT '更新人',
  `update_time` DATETIME DEFAULT NULL COMMENT '更新时间',
  `del_flag` INT(11) DEFAULT '0' COMMENT '删除标志（0代表未删除，1代表已删除）',
  PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COMMENT='用户表'
~~~
- 引入MybatisPuls和mysql驱动的依赖
~~~java
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.3</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
~~~
- 配置数据库信息
~~~java
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/sg_security?characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
~~~
- 定义Mapper接口
~~~java
public interface UserMapper extends BaseMapper<User> {
}
~~~
- 修改User实体类
~~~java
类名上加@TableName(value = "sys_user") ,id字段上加 @TableId
~~~
- 配置Mapper扫描
~~~java
@SpringBootApplication
@MapperScan("com.sangeng.mapper")
public class SimpleSecurityApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SimpleSecurityApplication.class);
        System.out.println(run);
    }
}
~~~

#### 4.3.4 UserDetailsService接口
- 当什么也没有配置的时候，账号和密码是由 Spring Security 定义生成的。而在实际项目中 账号和密码都是从数据库中查询出来的。 所以我们要通过自定义逻辑控制认证逻辑。如果需要自定义逻辑时，只需要实现 UserDetailsService 接口即可。接口定义如下：
![20220401224310](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220401224310.png)

- 返回值 UserDetails
  这个类是系统默认的用户"主体"
  ~~~java
   // 表示获取登录用户所有权限
   Collection<? extends GrantedAuthority> getAuthorities();
   // 表示获取密码
   String getPassword();
   // 表示获取用户名
   String getUsername();
   // 表示判断账户是否过期
   boolean isAccountNonExpired();
   // 表示判断账户是否被锁定
   boolean isAccountNonLocked();
   // 表示凭证{密码}是否过期
   boolean isCredentialsNonExpired();
   // 表示当前用户是否可用
   boolean isEnabled();
  ~~~
   以下是 UserDetails 实现类

   ![20220401224549](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220401224549.png)

   一般使用User 这个实体类  
   ![20220401224621](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220401224621.png)

这里我们自己定义UserDetails实现类
- 代码示例
 UserDetailsService实现类
 ~~~java
@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        //根据用户名查询用户信息
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getUserName,username);
        User user = userMapper.selectOne(wrapper);
        //如果查询不到数据就通过抛出异常来给出提示
        if(Objects.isNull(user)){
            throw new RuntimeException("用户名或密码错误");
        }
        //TODO 根据用户查询权限信息 添加到LoginUser中
        
        //封装成UserDetails对象返回 
        return new LoginUser(user);
    }
}
~~~
 UserDetails实现类
 ~~~java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class LoginUser implements UserDetails {

    private User user;


    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUserName();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
~~~
PS: 如果要测试，需要往用户表中写入用户数据，并且如果你想让用户的密码是明文存储，需要在密码前加{noop}。
![image-20211216123945882](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/image-20211216123945882.png)

#### 4.3.5 PasswordEncoder接口
- PasswordEncoder接口是对密码进行加密的接口
~~~java
// 表示把参数按照特定的解析规则进行解析
String encode(CharSequence rawPassword);
// 表示验证从存储中获取的编码密码与编码后提交的原始密码是否匹配。如果密码匹
配，则返回 true；如果不匹配，则返回 false。第一个参数表示需要被解析的密码。第二个
参数表示存储的密码。
boolean matches(CharSequence rawPassword, String encodedPassword);
// 表示如果解析的密码能够再次进行解析且达到更安全的结果则返回 true，否则返回
false。默认返回 false。
default boolean upgradeEncoding(String encodedPassword) {
return false;
} 
~~~
- 实现类我们一般用BCryptPasswordEncoder类
  - BCryptPasswordEncoder 是 Spring Security 官方推荐的密码解析器，平时多使用这个解析器。
  - BCryptPasswordEncoder 是对 bcrypt 强散列方法的具体实现。是基于 Hash 算法实现的单向加密。可以通过 strength 控制加密强度，默认 10.
- 只需要使用把BCryptPasswordEncoder对象注入Spring容器中，SpringSecurity就会使用该PasswordEncoder来进行密码校验。
~~~java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {


    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

}
~~~
#### 4.3.6 登录接口
- 前后端分离写法
  1. 接下我们需要自定义登陆接口，然后让SpringSecurity对这个接口放行,让用户访问这个接口的时候不用登录也能访问。
  2. 在接口中我们通过AuthenticationManager的authenticate方法来进行用户认证,所以需要在SecurityConfig中配置把AuthenticationManager注入容器。
  3. 认证成功的话要生成一个jwt，放入响应中返回。并且为了让用户下回请求时能通过jwt识别出具体的是哪个用户，我们需要把用户信息存入redis，可以把用户id作为key。
~~~java
@RestController
public class LoginController {

    @Autowired
    private LoginServcie loginServcie;

    @PostMapping("/user/login")
    public ResponseResult login(@RequestBody User user){
        return loginServcie.login(user);
    }
}
~~~

~~~java
@Service
public class LoginServiceImpl implements LoginServcie {

    @Autowired
    private AuthenticationManager authenticationManager;
    @Autowired
    private RedisCache redisCache;

    @Override
    public ResponseResult login(User user) {
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(user.getUserName(),user.getPassword());
        Authentication authenticate = authenticationManager.authenticate(authenticationToken);
        if(Objects.isNull(authenticate)){
            throw new RuntimeException("用户名或密码错误");
        }
        //使用userid生成token
        LoginUser loginUser = (LoginUser) authenticate.getPrincipal();
        String userId = loginUser.getUser().getId().toString();
        String jwt = JwtUtil.createJWT(userId);
        //authenticate存入redis
        redisCache.setCacheObject("login:"+userId,loginUser);
        //把token响应给前端
        HashMap<String,String> map = new HashMap<>();
        map.put("token",jwt);
        return new ResponseResult(200,"登陆成功",map);
    }
}
~~~
- SpringSecurity配置类放行登录接口
~~~java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {


    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                //关闭csrf
                .csrf().disable()
                //不通过Session获取SecurityContext
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                // 对于登录接口 允许匿名访问
                .antMatchers("/user/login").anonymous()
                // 除上面外的所有请求全部需要鉴权认证
                .anyRequest().authenticated();
    }

    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}
~~~

#### 4.3.7 自定义Jwt过滤器
1. 我们需要自定义一个过滤器，这个过滤器会去获取请求头中的token，对token进行解析取出其中的userid。
2. 使用userid去redis中获取对应的LoginUser对象。
3. 然后封装Authentication对象存入SecurityContextHolder
~~~java
@Component
public class JwtAuthenticationTokenFilter extends OncePerRequestFilter {

    @Autowired
    private RedisCache redisCache;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        //获取token
        String token = request.getHeader("token");
        if (!StringUtils.hasText(token)) {
            //放行
            filterChain.doFilter(request, response);
            return;
        }
        //解析token
        String userid;
        try {
            Claims claims = JwtUtil.parseJWT(token);
            userid = claims.getSubject();
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException("token非法");
        }
        //从redis中获取用户信息
        String redisKey = "login:" + userid;
        LoginUser loginUser = redisCache.getCacheObject(redisKey);
        if(Objects.isNull(loginUser)){
            throw new RuntimeException("用户未登录");
        }
        //存入SecurityContextHolder
        //TODO 获取权限信息封装到Authentication中
        UsernamePasswordAuthenticationToken authenticationToken =
                new UsernamePasswordAuthenticationToken(loginUser,null,null);
        SecurityContextHolder.getContext().setAuthentication(authenticationToken);
        //放行
        filterChain.doFilter(request, response);
    }
}
~~~
- SpringSecurity配置类添加自定义接口
~~~java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {


    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }


    @Autowired
    JwtAuthenticationTokenFilter jwtAuthenticationTokenFilter;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                //关闭csrf
                .csrf().disable()
                //不通过Session获取SecurityContext
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                // 对于登录接口 允许匿名访问
                .antMatchers("/user/login").anonymous()
                // 除上面外的所有请求全部需要鉴权认证
                .anyRequest().authenticated();

        //把token校验过滤器添加到过滤器链中
        http.addFilterBefore(jwtAuthenticationTokenFilter, UsernamePasswordAuthenticationFilter.class);
    }

    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}
~~~
#### 4.3.8 退出接口
- 前后端分离写法
  - 只需要定义一个登陆接口，然后获取SecurityContextHolder中的认证信息，删除redis中对应的数据即可。
~~~java
  @Service
  public class LoginServiceImpl implements LoginServcie {

    @Autowired
    private AuthenticationManager authenticationManager;
    @Autowired
    private RedisCache redisCache;

    @Override
    public ResponseResult login(User user) {
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(user.getUserName(),user.getPassword());
        Authentication authenticate = authenticationManager.authenticate(authenticationToken);
        if(Objects.isNull(authenticate)){
            throw new RuntimeException("用户名或密码错误");
        }
        //使用userid生成token
        LoginUser loginUser = (LoginUser) authenticate.getPrincipal();
        String userId = loginUser.getUser().getId().toString();
        String jwt = JwtUtil.createJWT(userId);
        //authenticate存入redis
        redisCache.setCacheObject("login:"+userId,loginUser);
        //把token响应给前端
        HashMap<String,String> map = new HashMap<>();
        map.put("token",jwt);
        return new ResponseResult(200,"登陆成功",map);
    }

    @Override
    public ResponseResult logout() {
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        LoginUser loginUser = (LoginUser) authentication.getPrincipal();
        Long userid = loginUser.getUser().getId();
        redisCache.deleteObject("login:"+userid);
        return new ResponseResult(200,"退出成功");
    }
}
~~~

## 5. SpringSecurity鉴权
### 5.1 SpringSecurity鉴权流程
- 在SpringSecurity中，会使用默认的FilterSecurityInterceptor来进行权限校验。在FilterSecurityInterceptor中会从SecurityContextHolder获取其中的Authentication，然后获取其中的权限信息。当前用户是否拥有访问当前资源所需的权限。
![](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220404153532.png)
- ExceptionTranslationFilter 过滤器
  该过滤器是用于处理异常的，不需要我们配置，对于前端提交的请求会直接放行，捕获后 续抛出的异常并进行处理（例如：权限访问限制）。具体源码如下：
  ~~~java
  public class ExceptionTranslationFilter extends GenericFilterBean {
  	public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
			throws IOException, ServletException {
		HttpServletRequest request = (HttpServletRequest) req;
		HttpServletResponse response = (HttpServletResponse) res;

		try {
			//(1) 对前端的请求直接放行,不必要拦截
			chain.doFilter(request, response);
			logger.debug("Chain processed normally");
		}
		catch (IOException ex) {
			throw ex;
		}
		catch (Exception ex) {
			//(2) 捕获后续出现的异常进行处理
			Throwable[] causeChain = throwableAnalyzer.determineCauseChain(ex);
			//访问需要认证的资源,但当前请求未认证所抛出的异常
			RuntimeException ase = (AuthenticationException) throwableAnalyzer
					.getFirstThrowableOfType(AuthenticationException.class, causeChain);

			if (ase == null) {
				//访问权限受限的资源所抛出的异常
				ase = (AccessDeniedException) throwableAnalyzer.getFirstThrowableOfType(
						AccessDeniedException.class, causeChain);
			}

			if (ase != null) {
				if (response.isCommitted()) {
					throw new ServletException("Unable to handle the Spring Security Exception because the response is already committed.", ex);
				}
				handleSpringSecurityException(request, response, chain, ase);
			}
			else {
				if (ex instanceof ServletException) {
					throw (ServletException) ex;
				}
				else if (ex instanceof RuntimeException) {
					throw (RuntimeException) ex;
				}
				throw new RuntimeException(ex);
			}
		}
	}
    ~~~

- FilterSecurityInterceptor 过滤器
  FilterSecurityInterceptor 是过滤器链的最后一个过滤器，该过滤器是过滤器链 的最后一个过滤器，根据资源权限配置来判断当前请求是否有权限访问对应的资源。如果访问受限会抛出相关异常，最终所抛出的异常会由前一个过滤器 ExceptionTranslationFilter 进行捕获和处理。具体源码如下：
  ~~~java
  public class FilterSecurityInterceptor extends AbstractSecurityInterceptor implements Filter {
  //...
	//过滤器的 dofilter() 方法
	public void doFilter(ServletRequest request, ServletResponse response,
			FilterChain chain) throws IOException, ServletException {
		FilterInvocation fi = new FilterInvocation(request, response, chain);
		//执行 invoke()方法
		invoke(fi);
	}
  //...
  
	public void invoke(FilterInvocation fi) throws IOException, ServletException {
		if ((fi.getRequest() != null)
				&& (fi.getRequest().getAttribute(FILTER_APPLIED) != null)
				&& observeOncePerRequest) {
			fi.getChain().doFilter(fi.getRequest(), fi.getResponse());
		}
		else {
			// first time this request being called, so perform security checking
			if (fi.getRequest() != null && observeOncePerRequest) {
				fi.getRequest().setAttribute(FILTER_APPLIED, Boolean.TRUE);
			}

			//(1) 根据资源权限配置来判断当前请求是否有权限访问对应的资源.如果不能访问,则抛出相应的异常
			InterceptorStatusToken token = super.beforeInvocation(fi);

			try {
				//(2) 访问相关资源,通过 SpringMVC 的核心组件 DispatcherServlet 进行访问
				fi.getChain().doFilter(fi.getRequest(), fi.getResponse());
			}
			finally {
				super.finallyInvocation(token);
			}
			super.afterInvocation(token, null);
		}
	}
  ~~~
需要注意，Spring Security 的过滤器链是配置在 SpringMVC 的核心组件DispatcherServlet 运行之前。也就是说，请求通过 Spring Security 的所有过滤器， 不意味着能够正常访问资源，该请求还需要通过 SpringMVC 的拦截器链。

### 5.2 RBAC权限模型
- RBAC:Role-Based Access Control 基于角色的权限控制。这是目前最常被开发者使用也是相对易用、通用权限模型。
![image-20211222110238165](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/image-20211222110238165.png)
### 5.3 基于角色或权限访问
- 基于配置类实现
  1. hasAuthority方法
     ~~~java
     http.authorizeRequests() // 认证配置
        .antMatchers("/","/login","/hello").permitAll() //设置哪些路径可以直接访问，不需要认证
        .antMatchers("/findAll").hasAuthority("admin") //hasAuthority()，当前登录的用户，只有具有了admin权限才可以访问这个路径
        .anyRequest() // 任何请求
        .authenticated() // 都需要身份验证
        .and().csrf().disable();
     ~~~
  2. hasAnyAuthority方法
     ~~~java
     http.authorizeRequests() // 认证配置
        .antMatchers("/","/login","/hello").permitAll() //设置哪些路径可以直接访问，不需要认证
        .antMatchers("/findAll").hasAnyAuthority("admin,manager") //hasAnyAuthority()，当前登录的用户，有其中一个权限就能访问这个路径
        .anyRequest() // 任何请求
        .authenticated() // 都需要身份验证
        .and().csrf().disable();
     ~~~
  3. hasRole方法
     ~~~java
     http.authorizeRequests() // 认证配置
        .antMatchers("/","/login","/hello").permitAll() //设置哪些路径可以直接访问，不需要认证
        .antMatchers("/findAll").hasRole("sale") //hasRole()，只有具有了sale角色的才能访问该路径
        .anyRequest() // 任何请求
        .authenticated() // 都需要身份验证
        .and().csrf().disable();
     ~~~
  4. hasAnyRole
     ~~~java
     http.authorizeRequests() // 认证配置
        .antMatchers("/","/login","/hello").permitAll() //设置哪些路径可以直接访问，不需要认证
        .antMatchers("/findAll").hasAnyRole("sale,admin") //hasAnyRole()，只有具有了sale或admin角色的才能访问该路径
        .anyRequest() // 任何请求
        .authenticated() // 都需要身份验证
        .and().csrf().disable()
     ~~~
- 基于注解实现
  1. @Secured
    - 判断是否具有角色，另外需要注意的是这里匹配的字符串需要添加前缀“ROLE_“
    - 进入方法前校验权限
    - 使用注解先要开启注解功能:在启动类上加@EnableGlobalMethodSecurity(securedEnabled=true)
    ~~~java
    @GetMapping("/testSecured")
    @Secured({"ROLE_管理员","ROLE_普通用户"}) //校验权限的注解
    public String testSecured(){
        return "hello,Secured";
    }
    ~~~

  2. @PreAuthorize
    - 进入方法前的权限验证
    - 先开启注解功能：在启动类上加 @EnableGlobalMethodSecurity(prePostEnabled = true)
     ~~~java
     @GetMapping("/testPreAuthorize")
     @PreAuthorize("hasAnyAuthority('menu:system')")
     public String testPreAuthorize(){
         System.out.println("PreAuthorize");
         return "hello PreAuthorize";
     }
     ~~~  
  3. @PostAuthorize
    - 注解使用并不多，在方法执行后再进行权限验证
    - 使用注解先要开启注解功能:在启动类上加@EnableGlobalMethodSecurity(prePostEnabled = true)
     ~~~java
     @GetMapping("/testPostAuthorize")
     @PostAuthorize("hasAnyAuthority('menu:system')")
     public String preAuthorize(){
         System.out.println("test--PostAuthorize");
         return "PostAuthorize";
    }
     ~~~   
  4. @PreFilter
    - 进入控制器之前对数据进行过滤,对入参进行过滤
     ~~~java
     @RequestMapping("getTestPreFilter")
     @PreAuthorize("hasRole('ROLE_管理员')")
     @PreFilter(value = "filterObject.id%2==0")
     public List<Users> getTestPreFilter(@RequestBody List<Users> list){
         list.forEach(t-> {
             System.out.println(t.toString());
        });
        return list;
    }
     ~~~
  5. @PostFilter
    - 对返回参数进行过滤,对出参进行过滤
     ~~~java
     @RequestMapping("/getAll")
     @PreAuthorize("hasRole('ROLE_管理员')")
     @PostFilter("filterObject.username == 'admin1'")
     public List<Users> getAllUser(){
         System.out.println("PostFilter");
         List<Users> list = new ArrayList<>();
         list.add(new Users(1,"admin1","6666"));
         list.add(new Users(2,"admin2","888"));
         return list;
    }
     ~~~

## 5. SpringSecurity其他解决方案
### 5.1 通过处理器进行认证
- 我们也可以不自己写登录和注销接口,通过实现SpringSecurity提供的接口来进行登录和注销,简而言之,就是将登录和注销的逻辑写到处理器中,当调用接口时或走处理器里面的逻辑
#### 5.1.1 认证成功处理器
- 实际上在UsernamePasswordAuthenticationFilter进行登录认证的时候，如果登录成功了是会调用AuthenticationSuccessHandler的方法进行认证成功后的处理的。AuthenticationSuccessHandler就是登录成功处理器。
- 我们也可以自己去自定义成功处理器进行成功后的相应处理。
自定义认证成功处理器类
~~~java
@Component
public class SGSuccessHandler implements AuthenticationSuccessHandler {

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {
        System.out.println("认证成功了");
    }
}
~~~

SpringSecurity配置类中配置认证处理器
~~~java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private AuthenticationSuccessHandler successHandler;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.formLogin().successHandler(successHandler);

        http.authorizeRequests().anyRequest().authenticated();
    }
}
~~~
#### 5.1.2 认证失败处理器
- 实际上在UsernamePasswordAuthenticationFilter进行登录认证的时候，如果认证失败了是会调用AuthenticationFailureHandler的方法进行认证失败后的处理的。AuthenticationFailureHandler就是登录失败处理器。
- 我们也可以自己去自定义失败处理器进行失败后的相应处理。  
  
自定义认证失败处理器
~~~java
@Component
public class SGFailureHandler implements AuthenticationFailureHandler {
    @Override
    public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException exception) throws IOException, ServletException {
        System.out.println("认证失败了");
    }
}
~~~

SpringSecurity配置认知失败处理器
~~~java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private AuthenticationSuccessHandler successHandler;

    @Autowired
    private AuthenticationFailureHandler failureHandler;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.formLogin()
//                配置认证成功处理器
                .successHandler(successHandler)
//                配置认证失败处理器
                .failureHandler(failureHandler);

        http.authorizeRequests().anyRequest().authenticated();
    }
}
~~~
#### 5.1.3 登出成功处理器
自定义登出成功处理器
~~~java
@Component
public class SGLogoutSuccessHandler implements LogoutSuccessHandler {
    @Override
    public void onLogoutSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {
        System.out.println("注销成功");
    }
}
~~~
SpringSecurity配置登出处理器
~~~java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private AuthenticationSuccessHandler successHandler;

    @Autowired
    private AuthenticationFailureHandler failureHandler;

    @Autowired
    private LogoutSuccessHandler logoutSuccessHandler;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.formLogin()
//                配置认证成功处理器
                .successHandler(successHandler)
//                配置认证失败处理器
                .failureHandler(failureHandler);

        http.logout()
                //配置注销成功处理器
                .logoutSuccessHandler(logoutSuccessHandler);

        http.authorizeRequests().anyRequest().authenticated();
    }
}
~~~  

### 5.2 自定义失败处理
- 当权限不足时,会默认跳转403页面
 ![1648994127(1)](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/1648994127(1).png)
- 我们还希望在认证失败或者是授权失败的情况下也能和我们的接口一样返回相同结构的json，这样可以让前端能对响应进行统一的处理。要实现这个功能我们需要知道SpringSecurity的异常处理机制。
- 在SpringSecurity中，如果我们在认证或者授权的过程中出现了异常会被ExceptionTranslationFilter捕获到。在ExceptionTranslationFilter中会去判断是认证失败还是授权失败出现的异常。
- 如果是认证过程中出现的异常会被封装成AuthenticationException然后调用AuthenticationEntryPoint对象的方法去进行异常处理。
- 如果是授权过程中出现的异常会被封装成AccessDeniedException然后调用AccessDeniedHandler对象的方法去进行异常处理。
- 所以如果我们需要自定义异常处理，我们只需要自定义AuthenticationEntryPoint和AccessDeniedHandler然后配置给SpringSecurity即可。

自定义实现类
~~~java
@Component
public class AccessDeniedHandlerImpl implements AccessDeniedHandler {
    @Override
    public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException accessDeniedException) throws IOException, ServletException {
        ResponseResult result = new ResponseResult(HttpStatus.FORBIDDEN.value(), "权限不足");
        String json = JSON.toJSONString(result);
        WebUtils.renderString(response,json);

    }
}
~~~

~~~java
@Component
public class AuthenticationEntryPointImpl implements AuthenticationEntryPoint {
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        ResponseResult result = new ResponseResult(HttpStatus.UNAUTHORIZED.value(), "认证失败请重新登录");
        String json = JSON.toJSONString(result);
        WebUtils.renderString(response,json);
    }
}
~~~

SpringSecurity配置自定义失败处理
先注入接口
~~~java
 @Autowired
    private AuthenticationEntryPoint authenticationEntryPoint;

    @Autowired
    private AccessDeniedHandler accessDeniedHandler;
~~~

使用HttpSecurity对象的方法去配置
~~~java
  http.exceptionHandling().authenticationEntryPoint(authenticationEntryPoint).
                accessDeniedHandler(accessDeniedHandler);
~~~

### 5.3 前后端不分离
#### 5.3.1 认证
1. 我们使用自己的登录页面
登陆页面传参默认为username,password
~~~html
<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/html">
<head>
    <meta charset="UTF-8">
    <title>登录页面</title>
</head>
<body>
<form action="/login" method="post">
    用户：<input type="text" name="username" /></br>
    密码：<input type="password" name="password" /></br>
    记住我：<input type="checkbox" name="remember-me" value="true"/><br/>
    <input type="submit" value="登录">
</form>

<a href="http://localhost:8080/oauth/authorize?response_type=code&client_id=admin&redirect_uri=http://localhost:8081/otherLogin">
    其他app登录
</a>

</body>
</html>
~~~

SpringSecurity配置使用自定义登录页面
~~~java
http.formLogin()
                //自定义登录页面
                .loginPage("/login.html")
~~~

2. 我们使用SpringSecurity默认的认证代码,配置SpringSecurity的登录请求url为/login,当然还要根据你自己需求实现UserDetails接口和PasswordEncoder接口
~~~java
http.formLogin()
                //自定义登录页面
                .loginPage("/login.html")
                //登录请求接口地址
                .loginProcessingUrl("/login")
~~~

3. 编写登录成功和失败的请求接口或者是使用实现登录成功处理器接口和登录失败处理器接口
   请求接口的方式
   ~~~java
   @Controller
   public class LoginController {
       @PostMapping("/toSuccessLogin")
       public String toSuccessLogin() {
           return "redirect:successLogin.html";
        }

        @PostMapping("/toErrorLogin")
        public String toErrorLogin() {
            return "redirect:errorLogin.html";
        }
    }
   ~~~
   实现处理器的方式
   ~~~java
   public class MySuccessHandler implements AuthenticationSuccessHandler {
      
       private String targetUrl;
       
       public MySuccessHandler(String targetUrl) {
           this.targetUrl = targetUrl;
        }
        
        @Override
        public void onAuthenticationSuccess(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Authentication authentication) throws IOException, ServletException {
            httpServletResponse.sendRedirect(targetUrl);
        }
    }
   ~~~
   ~~~java
   public class MyFailureHandler implements AuthenticationFailureHandler {
       
       private String targetUrl;
       
       public MyFailureHandler(String targetUrl) {
           this.targetUrl = targetUrl;
        }
        
        @Override
        public void onAuthenticationFailure(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, AuthenticationException e) throws IOException, ServletException {
            httpServletResponse.sendRedirect(targetUrl);
        }
    }
   ~~~

SpringSecurity配置
~~~java
http.formLogin()
                //自定义登录页面
                .loginPage("/login.html")
                //登录请求接口地址
                .loginProcessingUrl("/login")
                //登录成功后发送的请求路径,接口必须为Post请求
                .successForwardUrl("/toSuccessLogin")
                //自定义成功处理器
                //.successHandler(new MySuccessHandler("/successLogin.html"))
                //登录失败后发送的请求路径,接口必须为Post请求
                .failureForwardUrl("/toErrorLogin");
                //自定义失败处理器
                //.failureHandler(new MyFailureHandler("/errorLogin.html"));
~~~

4. 放行静态资源
~~~java
 http.authorizeRequests()
                //放行登录失败页面静态资源
                .antMatchers("/errorLogin.html").permitAll()
                //放行登录页面静态资源
                .antMatchers("/login.html").permitAll()
                //所有请求都需要登录
                .anyRequest().authenticated();
       

        http.csrf().disable();
~~~

#### 5.3.2 登出
- 登出我们使用SpringSecurity的默认代码,只需要配置登出路径为/logout,然后配置登出后跳转路径

~~~java
http.logout()
                .logoutUrl("/logout")
                .logoutSuccessUrl("http://baidu.com");
~~~

## 6.SpringSecurity知识点杂谈
### 6.1 访问控制url匹配
1. anyRequest()
   - 在之前认证过程中我们就已经使用过 anyRequest()，表示匹配所有的请求。一般情况下此方法都会使用，设置全部内容都需要进行认证。
    ~~~java
    .anyRequest().authenticated(); 
    ~~~

2. antMatcher()
   - 方法定义如下
   ~~~java
   public C antMatchers(String... antPatterns)  
   ~~~
   - 参数是不定向参数，每个参数是一个 ant 表达式，用于匹配 URL规则。
   - ant表达式规则如下：
     - ? ： 匹配一个字符
     - *：匹配 0 个或多个字符
     - ** ：匹配 0 个或多个目录
   - 示例1:在实际项目中经常需要放行所有静态资源，下面演示放行 js 文件夹下所有脚本文件。
   ~~~java
   .antMatchers("/js/**","/css/**").permitAll() 
   ~~~
   - 示例2:只要是.js 文件都放行
   ~~~java
   .antMatchers("/**/*.js").permitAll()
   ~~~

3. regexMatchers()
   - 使用正则表达式进行匹配。和 antMatchers() 主要的区别就是参数， antMatchers() 参数是 ant 表达式， regexMatchers() 参数是正则表达式。
   - 示例:所有以.js 结尾的文件都被放行。
   ~~~java
   .regexMatchers( ".+[.]js").permitAll()
   ~~~
   - 无论是 antMatchers() 还是 regexMatchers() 都具有两个参数的方法，其中第一个参数都是HttpMethod ，表示请求方式，当设置了 HttpMethod 后表示只有设定的特定的请求方式才执行对应的权限设置。
  
4. mvcMatchers()
   - mvcMatchers()适用于配置了 servletPath 的情况。
   - servletPath 就是所有的 URL 的统一前缀。在 SpringBoot 整合SpringMVC 的项目中可以在application.properties 中添加下面内容设置 ServletPath
    ~~~java
    spring.mvc.servlet.path=/xxxx
    ~~~
   - 在 Spring Security 的配置类中配置 .servletPath() 是 mvcMatchers()返回值特有的方法，antMatchers()和 regexMatchers()没有这个方法。在 servletPath() 中配置了 servletPath 后，mvcMatchers()直接写 SpringMVC 中@RequestMapping()中设置的路径即可。
    ~~~java
    .mvcMatchers("/demo").servletPath("/xxxx").permitAll()
    ~~~
    - 如果不习惯使用 mvcMatchers()也可以使用 antMatchers()，下面代码和上面代码是等效
    ~~~java
    .antMatchers("/xxxx/demo").permitAll()

    ~~~

### 6.2 内置访问控制方法
1. permitAll()
   - 表示所匹配的 URL 任何人都允许访问。

2. authenticated()
   - 表示所匹配的 URL 都需要被认证才能访问。

3. anonymous()
   - 表示可以匿名访问匹配的URL。和permitAll()效果类似，只是设置为 anonymous()的 url会执行 filter 链中
  
4. denyAll()
   - 表示所匹配的 URL 都不允许被访问

5. rememberMe()
   - “remember me”的用户允许访问

6. fullyAuthenticated()
   - 如果用户不是被 remember me 的，才可以访问。

### 6.3 Access方法
- 无论是hasAuthority,hasRole,permitAll,rememberMe等方法都是调用access方法
  ![1648991317(1)](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/1648991317(1).png)
- access方法参数就是String字符串,这个字符串是access表达式
- [SpringSecurity官网](https://docs.spring.io/spring-security/site/docs/5.3.4.RELEASE/reference/html5/#el-access)对access表达式定义:  

### 6.4 跨域问题
- 跨域是指浏览器出于安全的考虑，使用 XMLHttpRequest对象发起 HTTP请求时必须遵守同源策略，否则就是跨域的HTTP请求，默认情况下是被禁止的。 同源策略要求源相同才能正常进行通信，即协议、域名、端口号都完全一致。
- 前后端分离项目，前端项目和后端项目一般都不是同源的，所以肯定会存在跨域请求的问题。
1. SpringBoot配置，运行跨域请求
   ~~~java
   @Configuration
   public class CorsConfig implements WebMvcConfigurer {
       
        @Override
        public void addCorsMappings(CorsRegistry registry) {
            // 设置允许跨域的路径
            registry.addMapping("/**")
                // 设置允许跨域请求的域名
                .allowedOriginPatterns("*")
                // 是否允许cookie
                .allowCredentials(true)
                // 设置允许的请求方式
                .allowedMethods("GET", "POST", "DELETE", "PUT")
                // 设置允许的header属性
                .allowedHeaders("*")
                // 跨域允许时间
                .maxAge(3600);
        }
    }
   ~~~
2.开启SpringSecurity的跨域访问
   ~~~java
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                //关闭csrf
                .csrf().disable()
                //不通过Session获取SecurityContext
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                // 对于登录接口 允许匿名访问
                .antMatchers("/user/login").anonymous()
                // 除上面外的所有请求全部需要鉴权认证
                .anyRequest().authenticated();

        //添加过滤器
        http.addFilterBefore(jwtAuthenticationTokenFilter, UsernamePasswordAuthenticationFilter.class);

        //配置异常处理器
        http.exceptionHandling()
                //配置认证失败处理器
                .authenticationEntryPoint(authenticationEntryPoint)
                .accessDeniedHandler(accessDeniedHandler);

        //允许跨域
        http.cors();
    }
   ~~~
### 6.5 CSRF攻击
- 跨站请求伪造（英语：Cross-site request forgery），也被称为 one-click attack 或者 session riding，通常缩写为 CSRF 或者 XSRF， 是一种挟制用户在当前已 登录的 Web 应用程序上执行非本意的操作的攻击方法。跟跨网站脚本（XSS）相比，XSS 利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任。
- 跨站请求攻击，简单地说，是攻击者通过一些技术手段欺骗用户的浏览器去访问一个 自己曾经认证过的网站并运行一些操作（如发邮件，发消息，甚至财产操作如转账和购买 商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去运行。 这利用了 web 中用户身份验证的一个漏洞：简单的身份验证只能保证请求发自某个用户的浏览器，却不能保证请求本身是用户自愿发出的。
  [CSDN关于CSRF文章](https://blog.csdn.net/freeking101/article/details/86537087)
- SpringSecurity去防止CSRF攻击的方式就是通过csrf_token。后端会生成一个csrf_token，前端发起请求的时候需要携带这个csrf_token,后端会有过滤器进行校验，如果没有携带或者是伪造的就不允许访问。
- 我们可以发现CSRF攻击依靠的是cookie中所携带的认证信息。但是在前后端分离的项目中我们的认证信息其实是token，而token并不是存储中cookie中，并且需要前端代码去把token设置到请求头中才可以，所以CSRF攻击也就不用担心了。
- 从 Spring Security 4.0 开始，默认情况下会启用 CSRF 保护，以防止 CSRF 攻击应用 程序，Spring Security CSRF 会针对 PATCH，POST，PUT 和 DELETE 方法进行防护。
- 如果是使用JWT要关闭CSRF防护

### 6.6 SecurityContextHolder类
![20220404170445](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220404170445.png)

![20220404170505](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220404170505.png)

- SpringSecurity将用户信息保存在SecurityContextHolder类中,当过滤器或者其他地方需要获取用户信息时可以直接从SecurityContextHolder类中拿,该类其实是对ThreadLocal的封装，- SecurityContextHolder类源码：
  ~~~java
  public class SecurityContextHolder {
	// ~ Static fields/initializers
	// =====================================================================================

	public static final String MODE_THREADLOCAL = "MODE_THREADLOCAL";
	public static final String MODE_INHERITABLETHREADLOCAL = "MODE_INHERITABLETHREADLOCAL";
	public static final String MODE_GLOBAL = "MODE_GLOBAL";
	public static final String SYSTEM_PROPERTY = "spring.security.strategy";
	private static String strategyName = System.getProperty(SYSTEM_PROPERTY);
	private static SecurityContextHolderStrategy strategy;
	private static int initializeCount = 0;

	//(1) 最先执行
	static {
		initialize();
	}

	private static void initialize() {
		if (!StringUtils.hasText(strategyName)) {
			//默认使用 MODE_THREADLOCAL 模式
			strategyName = MODE_THREADLOCAL;
		}

		if (strategyName.equals(MODE_THREADLOCAL)) {
			//默认使用 ThreadLocalSecurityContextHolderStrategy 创建 strategy ,其内部使用 ThreadLocal 对 SecurityContext 进行存储
			strategy = new ThreadLocalSecurityContextHolderStrategy();
		}
		else if (strategyName.equals(MODE_INHERITABLETHREADLOCAL)) {
			strategy = new InheritableThreadLocalSecurityContextHolderStrategy();
		}
		else if (strategyName.equals(MODE_GLOBAL)) {
			strategy = new GlobalSecurityContextHolderStrategy();
		}
		else {
			try {
				Class<?> clazz = Class.forName(strategyName);
				Constructor<?> customStrategy = clazz.getConstructor();
				strategy = (SecurityContextHolderStrategy) customStrategy.newInstance();
			}
			catch (Exception ex) {
				ReflectionUtils.handleReflectionException(ex);
			}
		}
		initializeCount++;
	}
  
	public static void clearContext() {
		//清空当前线程对应的 ThreadLocal<SecurityContext> 的存储
		strategy.clearContext();
	}
	
	public static SecurityContext getContext() {
		//注意:如果当前线程对应的 ThreadLocal<SecurityContext> 没有任何对象存储,
		//strategy.getContext() 会创建并返回一个空的 SecurityContext 对象,
		//并且该空的 SecurityContext 对象会存入 ThreadLocal<SecurityContext>
		return strategy.getContext();
	}

	public static void setContext(SecurityContext context) {
		//设置当前线程对应的 ThreadLocal<SecurityContext> 的存储
		strategy.setContext(context);
	}
   ~~~


[参考的部分笔记地址](https://www.cnblogs.com/lzghyh/p/14744040.html#_label1)