# YEB

## 1、后端

### 1.1 登录验证码

1. 引入依赖（谷歌验证码）

   ```xml
   <dependency>
     <groupId>com.github.axet</groupId>
     <artifactId>kaptcha</artifactId>
     <version>0.0.9</version>
   </dependency>
   ```

2. 验证码配置类

   ```java
   /**
    * 验证码配置类
    */
   @Configuration
   public class CaptchaConfig {
     @Bean
     public DefaultKaptcha defaultKaptcha() {
       //验证码生成器
       DefaultKaptcha defaultKaptcha = new DefaultKaptcha();
       //配置
       Properties properties = new Properties();
       //是否有边框
       properties.setProperty("kaptcha.border", "yes");
       //设置边框颜色
       properties.setProperty("kaptcha.border.color", "105,179,90");
       //边框粗细度，默认为1
       // properties.setProperty("kaptcha.border.thickness","1");
       //验证码
       properties.setProperty("kaptcha.session.key", "code");
       //验证码文本字符颜色 默认为黑色
       properties.setProperty("kaptcha.textproducer.font.color", "blue");
       //设置字体样式
       properties.setProperty("kaptcha.textproducer.font.names", "宋体,楷体,微软雅黑");
       //字体大小，默认40
       properties.setProperty("kaptcha.textproducer.font.size", "30");
       //验证码文本字符内容范围 默认为abced2345678gfynmnpwx
       // properties.setProperty("kaptcha.textproducer.char.string", "");
       //字符长度，默认为5
       properties.setProperty("kaptcha.textproducer.char.length", "4");
       //字符间距 默认为2
       properties.setProperty("kaptcha.textproducer.char.space", "4");
       //验证码图片宽度 默认为200
       properties.setProperty("kaptcha.image.width", "100");
       //验证码图片高度 默认为40
       properties.setProperty("kaptcha.image.height", "40");
       Config config = new Config(properties);
       defaultKaptcha.setConfig(config);
       return defaultKaptcha;
     }
   }
   ```

3. CaptchaController

   ```java
   @RestController
   public class CaptchaController {
     @Autowired
     private DefaultKaptcha defaultKaptcha;
     @ApiOperation("验证码")
     @GetMapping(value = "/captcha", produces = "image/jpeg")
     public void captcha(HttpServletRequest request, HttpServletResponse response) {
       //-----------------处理响应头，以传输图片数据----------------------
       //定义response输出类型为image/jpeg
       response.setDateHeader("Expires",0);
       //Set standard HTTP/1.1 no-cache headers
       response.setHeader("Cache-Control", "no-store, no-cache, must-revalidate");
       //Set IE extended HTTP/1.1 no-cache headers (use addHeader).
       response.addHeader("Cache-Control", "post-check=0, pre-check=0");
       //Set standard HTTP/1.0 no-cache header.
       response.setHeader("Pragma", "no-cache");
       //return a jpeg
       response.setContentType("image/jpeg");
       //---------------------------生成验证码 begin ----------------------
       //获取验证码文本内容
       String text = defaultKaptcha.createText();
       System.out.println("验证码内容：" + text);
       //将文本内容放入session
       request.getSession().setAttribute("captcha", text);
       //根据文本验证码内容创建图形验证码
       BufferedImage image = defaultKaptcha.createImage(text);
       ServletOutputStream outputStream = null;
       try {
         outputStream = response.getOutputStream();
         //输出流输出图片，格式为jpg
         ImageIO.write(image, "jpg", outputStream);
         outputStream.flush();
       } catch (IOException e) {
         e.printStackTrace();
       } finally {
         if (outputStream != null) {
           try {
             outputStream.close();
           } catch (IOException e) {
             e.printStackTrace();
           }
         }
       }
       //---------------------------生成验证码 end ----------------------
     }
   }
   ```

   > 当然还要**放行**这个接口，在SpringSecurity配置类中
   >
   > ```java
   > @Override
   > public void configure(WebSecurity web) throws Exception {
   >   web.ignoring().antMatchers(
   >     "/login",
   >     "/logout",
   >     "/css/**",
   >     "/js/**",
   >     "/index.html",
   >     "favicon.ico",
   >     "/doc.html",
   >     "/webjars/**",
   >     "/swagger-resources/**",
   >     "/v2/api-docs/**",
   >     "/captcha"
   >   );
   > }
   > ```
   >
   > `@RequestMapping`的`produces`属性是明确指出接口的返回值类型，默认是JSON

4. 在**登录接口**中添加<u>对验证码的校验</u>

   ```java
   String captcha = (String) request.getSession().getAttribute("captcha");
   if (StringUtils.isEmpty(captcha) || !captcha.equalsIgnoreCase(code)) {
     return RespBean.error("验证码输入错误，请重新输入！");
   }
   ```

### 1.2 根据url验证权限

1. 在Dao层和Service层实现根据id获取角色，并在loadUserByUsername时设置到UserDetails中

   ```java
   //SpringConfig中
   @Bean
   @Override
   public UserDetailsService userDetailsService() {
     return username -> {
       Admin admin = adminService.getAdminByUserName(username);
       if (admin != null) {
         admin.setRoles(adminService.getRoles(admin.getId()));
         return admin;
       }
       throw new UsernameNotFoundException("用户名不存在");
     };
   }
   ```

2.  配置**过滤器**(`FilterInvocationSecurityMetadataSource`)，根据请求url设置所需角色

   ```java
   /**
    * 权限控制
    * 根据请求url分析角色
    */
   @Component
   public class CustomFilter implements FilterInvocationSecurityMetadataSource {
     @Autowired
     private IMenuService menuService;
   
     AntPathMatcher antPathMatcher = new AntPathMatcher();
   
     @Override
     public Collection<ConfigAttribute> getAttributes(Object o) throws IllegalArgumentException {
       //获取请求的url
       String requestUrl = ((FilterInvocation) o).getRequestUrl();
       List<Menu> menus = menuService.getMenusWithRole();
       for (Menu menu : menus) {
         //判断请求url与菜单角色是否匹配
         if (antPathMatcher.match(menu.getUrl(), requestUrl)) {
           String[] str = menu.getRoles().stream().map(Role::getName).toArray(String[]::new);
           return SecurityConfig.createList(str);
         }
       }
       //没匹配的url默认登录即可访问
       return SecurityConfig.createList("ROLE_LOGIN");
     }
   
     @Override
     public Collection<ConfigAttribute> getAllConfigAttributes() {
       return null;
     }
   
     @Override
     public boolean supports(Class<?> aClass) {
       return false;
     }
   }
   ```

3.  配置**管理器**(`AccessDecisionManager`)，判断当前用户是否具有所需角色

   ```java
   /**
   * 判断用户角色
   */
   @Component
   public class CustomUrlDecisionManager implements AccessDecisionManager {
   
     @Override
     public void decide(Authentication authentication, Object o, Collection<ConfigAttribute> collection) throws AccessDeniedException, InsufficientAuthenticationException {
       for (ConfigAttribute configAttribute : collection) {
         //当前URL所需角色(这都是在CustomFilter中设置的)
         String needRole = configAttribute.getAttribute();
         //当前所需角色是登录后即可访问
         if ("ROLE_LOGIN".equals(needRole)) {
           //判断是否登录
           if (authentication instanceof AnonymousAuthenticationToken) {
             throw new AccessDeniedException("尚未登录，无权访问");
           } else return;
         }
         Collection<? extends GrantedAuthority> authorities = authentication.getAuthorities();
         for (GrantedAuthority authority : authorities) {
           if (authority.getAuthority().equals(needRole)) {
             return;
           }
         }
       }
       throw new AccessDeniedException("权限不足");
     }
   
     @Override
     public boolean supports(ConfigAttribute configAttribute) {
       return false;
     }
   
     @Override
     public boolean supports(Class<?> aClass) {
       return false;
     }
   }
   ```

4. 在SpringSecurity中进行**动态权限配置**

   ```java
   @Override
   protected void configure(HttpSecurity http) throws Exception {
     //使用JWT，不需要csrf
     http.csrf()
       .disable()
       //基于token，不需要session
       .sessionManagement()
       .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
       .and()
       .authorizeRequests()
       //所有请求都要认证
       .anyRequest()
       .authenticated()
       /**************这里是 ***************/
       //动态权限配置
       .withObjectPostProcessor(new ObjectPostProcessor<FilterSecurityInterceptor>() {
         @Override
         public <O extends FilterSecurityInterceptor> O postProcess(O o) {
           o.setAccessDecisionManager(customUrlDecisionManager);
           o.setSecurityMetadataSource(customFilter);
           return o;
         }
       })
       .and()
       //禁用缓存
       .headers()
       .cacheControl();
     //添加jwt登录授权过滤器
     http.addFilterBefore(jwtAuthenticationTokenFilter(), UsernamePasswordAuthenticationFilter.class);
     //添加自定义未授权和未登录异常结果返回
     http.exceptionHandling()
       .accessDeniedHandler(restfulAccessDeniedHandler)
       .authenticationEntryPoint(restAuthorizationEntryPoint);
   }
   ```

   

   

   

   

   













## 2、前端

