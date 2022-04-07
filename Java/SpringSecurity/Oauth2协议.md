# Oauth2协议
## 1. Oauth2协议简介
- 第三方认证技术方案最主要是解决认证协议的通用标准问题，因为要实现跨系统认证，各系统之间要遵循一定的接口协议。
- OAUTH协议为用户资源的授权提供了一个安全的、开放而又简易的标准。同时，任何第三方都可以使用OAUTH认证服务，任何服务提供商都可以实现自身的OAUTH认证服务，因而OAUTH是开放的。业界提供了OAUTH的多种实现如PHP、JavaScript，Java，Ruby等各种语言开发包，大大节约了程序员的时间，因而OAUTH是简易的。互联网很多服务如Open API，很多大公司如Google，Yahoo，Microsoft等都提供了OAUTH认证服务，这些都足以说明OAUTH标准逐渐成为开放资源授权的标准。
- Oauth协议目前发展到2.0版本，1.0版本过于复杂，2.0版本已得到广泛应用。
- 参考：[官方网站](https://datatracker.ietf.org/doc/html/rfc6749)
## 2. Oauth2简单案例
- 第三方网站使用微信认证登录
  流程图:
  ![20220406223810](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220406223810.png)
  1. 首先微信用户点击豆瓣网微信授权登录按钮后，豆瓣网会将请求通过URL重定向的方式跳转至微信用户授权界面；
   
  2. 此时微信用户实际上是在微信上进行身份认证，与豆瓣网并无交互了，这一点非常类似于我们使用网银支付的场景；

  3. 用户使用微信客户端扫描二维码认证或者输入用户名密码后，微信会验证用户身份信息的正确性，如正确，则认为用户确认授权微信登录豆瓣网，此时会先生成一个临时凭证，并携带此凭证通过用户浏览器将请求重定向回豆瓣网在第一次重定向时携带的callBackUrl地址；

  4. 之后用户浏览器会携带临时凭证code访问豆瓣网服务，豆瓣网则通过此临时凭证再次调用微信授权接口，获取正式的访问凭据access_token；

  5. 在豆瓣网获取到微信授权访问凭据access_token后，此时用户的授权基本上就完成了，后续豆瓣网要做的只是通过此token再访问微信提供的相关接口，获取微信允许授权开发的用户信息，如头像，昵称等，并据此完成自身的用户逻辑及用户登录会话逻辑；
   
## 3. Oauth2协议4种模式
- OAuth2.0协议中定义了以下四个角色：
  1. resource owner（资源拥有者）
   即能够有权授予对保护资源访问权限的实体。例如我们使用通过微信账号登陆豆瓣网，而微信账号信息的实际拥有者就是微信用户，也被称为最终用户。

  2. resource server（资源服务器）
   承载受保护资源的服务器，能够接收使用访问令牌对受保护资源的请求并响应，它与授权服务器可以是同一服务器，也可以是不同服务器。在上述例子中该角色就是微信服务器。

  3. client（客户端）
   代表资源所有者及其授权发出对受保护资源请求的应用程序。在上面的例子中豆瓣网就是这样的角色。

  4. authorization server（授权服务器）
   认证服务器，即服务提供商专门用来处理认证授权的服务器。例如微信开放平台提供的认证服务的服器。                                                                                          

- OAuth2.0协议中有四种模式:
  授权码模式（authorization code）
  简化模式（implicit）
  密码模式（resource owner password credentials）
  客户端模式（client credentials）


   1. 授权码模式
   - 授权码模式（authorization code）是功能最完整、流程最严密的授权模式。它的特点是通过客户端的后台服务器，与“服务提供商”的认证服务进行互动（如微信开放平台），我们前面以微信账号登录豆瓣网的流程就是授权码模式的实现。这种模式下授权代码并不是第三方网站直接从资源所有者获取，而是通过授权服务器（authorization server）作为中介来获取，授权认证的过程也是资源所有者直接通过授权服务器进行身份认证，避免了资源所有者身份凭证与第三方网站共享的可能，因此是十分安全的。
   - 适用范围:安全性要求高，它适用于那些有后端的 Web 应用。授权码通过前端传送，令牌则是储存在后端，而且所有与资源服务器的通信都在后端完成。这样的前后端分离，可以避免令牌泄漏。
  
   - 实现流程:
    ① A网站提供一个链接，用户点击后就会跳转到B网站，授权用户数据给A网站使用。下面就是A网站跳转B网站的一个示意链接。
      ~~~
      https://b.com/oauth/authorize?response_type=code&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read
      ~~~
      上面 URL 中，response_type参数表示要求返回授权码（code），client_id参数让 B 知道是谁在请求，redirect_uri参数是 B 接受或拒绝请求后的跳转网址，scope参数表示要求的授权范围（这里是只读）。
      ![20220407221504](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220407221504.png)

     ② 用户跳转后，B 网站会要求用户登录，然后询问是否同意给予 A 网站授权。用户表示同意，这时 B 网站就会跳回redirect_uri参数指定的网址。跳转时，会传回一个授权码，就像下面这样。
     ~~~
     https://a.com/callback?code=AUTHORIZATION_CODE
     ~~~
     上面 URL 中，code参数就是授权码。
     ![20220407221557](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220407221557.png)
     上面 URL 中，code参数就是授权码。 

     ③ A 网站拿到授权码以后，就可以在后端，向 B 网站请求令牌。
      ~~~
      https://b.com/oauth/token?client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=authorization_code&code=AUTHORIZATION_CODE&redirect_uri=CALLBACK_URL
      ~~~
      上面 URL 中，client_id参数和client_secret参数用来让 B 确认 A 的身份（client_secret参数是保密的，因此只能在后端发请求），grant_type参数的值是AUTHORIZATION_CODE，表示采用的授权方式是授权码，code参数是上一步拿到的授权码，redirect_uri参数是令牌颁发后的回调网址。
      ![20220407221716](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220407221716.png)

      ④ B网站收到请求以后，就会颁发令牌。具体做法是向redirect_uri指定的网址，发送一段 JSON 数据。
      ~~~
      {    
        "access_token":"ACCESS_TOKEN",
        "token_type":"bearer",
        "expires_in":2592000,
        "refresh_token":"REFRESH_TOKEN",
        "scope":"read",
        "uid":100101,
        "info":{...}
      }
     ~~~
    上面 JSON 数据中，access_token字段就是令牌，A 网站在后端拿到了。
    ![20220407221908](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220407221908.png)
  
2. 简化模式
   - 简化模式是对授权码模式的简化，用于在浏览器中使用脚本语言如JS实现的客户端中，它的特点是不通过第三方网站的服务器，而是直接在浏览器中向认证服务器申请令牌，跳过了“授权码临时凭证”这个步骤。其所有的步骤都在浏览器中完成，令牌对访问者是可见的，且第三方网站不需要认证。如果我们使用此种授权方式来实现微信登录豆瓣网的过程的话.
   ![20220407214413](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220407214413.png)

   - 适用范围:Web 应用是纯前端应用,或者安全要求不高的场景，并且令牌的有效期必须非常短，通常就是会话期间（session）有效，浏览器关掉，令牌就失效了。
   - 实现流程:
     ① A 网站提供一个链接，要求用户跳转到 B 网站，授权用户数据给 A 网站使用。
     ~~~
     https://b.com/oauth/authorize?response_type=token&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read
     ~~~
     上面 URL 中，response_type参数为token，表示要求直接返回令牌。
     
     ② 用户跳转到 B 网站，登录后同意给予 A 网站授权。这时，B 网站就会跳回redirect_uri参数指定的跳转网址，并且把令牌作为 URL 参数，传给 A 网站。
     ~~~
     https://a.com/callback#token=ACCESS_TOKEN
     ~~~
     上面 URL 中，token参数就是令牌，A 网站因此直接在前端拿到令牌。
     
     注意，令牌的位置是 URL 锚点（fragment），而不是查询字符串（querystring），这是因为 OAuth 2.0 允许跳转网址是 HTTP 协议，因此存在"中间人攻击"的风险，而浏览器跳转时，锚点不会发到服务器，就减少了泄漏令牌的风险。
     ![20220407223415](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220407223415.png)

3. 密码模式
   - 在密码模式中，用户需要向客户端提供自己的用户名和密码，客户端使用这些信息向“服务提供商”索要授权。这相当于在豆瓣网中使用微信登录，我们需要在豆瓣网输入微信的用户名和密码，然后由豆瓣网使用我们的微信用户名和密码去向微信服务器获取授权信息。
  ![20220407214845](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220407214845.png)
 
   - 适用范围:这种模式一般用在用户对第三方网站高度信任的情况下，因为虽然协议规定第三方不得存储用户密码，但是实际上这一点并不是特别好强制约束。就像在豆瓣网输入了微信的用户名和密码，豆瓣网存储不存储我们并不是很清楚，所以安全性是非常低的。因此一般情况下是不会考虑使用这种模式进行授权的。
   
   - 实现流程:
     ① A 网站要求用户提供 B 网站的用户名和密码。拿到以后，A 就直接向 B 请求令牌。
     ~~~
     https://oauth.b.com/token?grant_type=password&username=USERNAME&password=PASSWORD&client_id=CLIENT_ID
     ~~~
     上面 URL 中，grant_type参数是授权方式，这里的password表示"密码式"，username和password是 B 的用户名和密码。

     ② B 网站验证身份通过后，直接给出令牌。注意，这时不需要跳转，而是把令牌放在 JSON 数据里面，作为 HTTP 回应，A 因此拿到令牌。
 
4. 客户端模式
   - 客户端模式是指客户端以自己的名义，而不是以用户的名义，向“服务提供方”进行认证。严格地说，客户端模式并不属于OAuth2.0协议所要解决的问题。在这种模式下，用户并不需要对客户端授权，用户直接向客户端注册，客户端以自己的名义要求“服务提供商”提供服务。
   - 适用范围:没有前端的命令行应用，即在命令行下请求令牌。针对第三方应用的，而不是针对用户的，即有可能多个用户共享同一个令牌。docker就是采用这种方式
   
   - 实现流程:
    ① A 应用在命令行向 B 发出请求。
     ~~~
     https://oauth.b.com/token?grant_type=client_credentials&client_id=CLIENT_ID&client_secret=CLIENT_SECRET
     ~~~ 
     上面 URL 中，grant_type参数等于client_credentials表示采用凭证式，client_id和client_secret用来让 B 确认 A 的身份。
     
     ② B 网站验证通过以后，直接返回令牌。

- 令牌的使用
  A 网站拿到令牌以后，就可以向 B 网站的 API 请求数据了。
  此时，每个发到 API 的请求，都必须带有令牌。具体做法是在请求的头信息，加上一个Authorization字段，令牌就放在这个字段里面。
  ~~~
  curl -H "Authorization: Bearer ACCESS_TOKEN" \
  "https://api.b.com"
  ~~~
  上面命令中，ACCESS_TOKEN就是拿到的令牌。

- 刷新令牌
  - 令牌的有效期到了，如果让用户重新走一遍上面的流程，再申请一个新的令牌，很可能体验不好，而且也没有必要。OAuth 2.0 允许用户自动更新令牌。
  - 具体方法是，B 网站颁发令牌的时候，一次性颁发两个令牌，一个用于获取数据，另一个用于获取新的令牌（refresh token 字段）。令牌到期前，用户使用 refresh token 发一个请求，去更新令牌。
   ~~~
   https://b.com/oauth/token?grant_type=refresh_token&client_id=CLIENT_ID&
   client_secret=CLIENT_SECRET&refresh_token=REFRESH_TOKEN
   ~~~
   - 上面 URL 中，grant_type参数为refresh_token表示要求更新令牌，client_id参数和client_secret参数用于确认身份，refresh_token参数就是用于更新令牌的令牌。B 网站验证通过以后，就会颁发新的令牌。

## 4. SpringSecurity + Oauth2 + JWT
- 授权者模式下一个第三方网站请求资源服务器上的用户信息
1. 资源服务和授权服务的依赖
~~~java
   <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.9.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
        <spring-cloud.version>Greenwich.SR2</spring-cloud.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-oauth2</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
~~~

1. 第三方网站请求授权服务接口
~~~java
@Controller
public class LoginController {

    @RequestMapping("/otherLogin")
    public String otherLogin(@RequestParam("code") String code) {
        Map mapStr = new HashMap();
        mapStr.put("grant_type", "authorization_code");
        mapStr.put("code", code);
        mapStr.put("client_id", "admin");
        mapStr.put("redirect_uri", "http://localhost:8081/otherLogin");
        mapStr.put("scope", "all");

        String url = "http://localhost:8080/oauth/token";
        String result = HttpUtil.createPost(url)
                .basicAuth("admin", "112233")
                .formStr(mapStr)
                .execute()
                .body();
        JSONObject jsonObject = JSONUtil.parseObj(result);
        String accessToken = (String) jsonObject.get("access_token");
        System.out.println(accessToken);

        String auth = HttpUtil.createPost("http://localhost:8080/user/getCurrentUser")
                .bearerAuth(accessToken)
                .execute()
                .body();
        System.out.println(auth);
        return "redirect:register.html";
    }
}
~~~

3. 授权服务器配置类
~~~java
@Configuration
@EnableAuthorizationServer
public class AuthorizationServerConfig extends AuthorizationServerConfigurerAdapter {

    @Autowired
    private PasswordEncoder passwordEncoder;

    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws
            Exception {
        clients.inMemory()
                //配置client_id
                .withClient("admin")
                //配置client-secret
                .secret(passwordEncoder.encode("112233"))
                //配置访问token的有效期
                .accessTokenValiditySeconds(36000)
                //配置刷新token的有效期
                .refreshTokenValiditySeconds(864000)
                //配置redirect_uri，用于授权成功后跳转
                .redirectUris("http://localhost:8081/otherLogin")
                //配置申请的权限范围
                .scopes("all")
                //配置grant_type，表示授权类型
                .authorizedGrantTypes("authorization_code");
    }
}
~~~

4. 资源服务器配置类
~~~java
@Configuration
@EnableResourceServer
public class ResourceServerConfig extends ResourceServerConfigurerAdapter {

    @Override
    public void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .anyRequest()
                .authenticated()
                .and()
                .requestMatchers()
                .antMatchers("/user/**");//配置需要保护的资源路径
    }

}
~~~

[客户端代码](https://gitee.com/caoxiaokun/spring-security)
[授权服务和资源服务代码](https://gitee.com/caoxiaokun/spring-security-oauth2)