[spring的各种构建教程]: https://spring.io/guides#getting-started-guides

#### 1. bean装载到Spring应用上下文的生命周期

> 1. Spring对bean进行实例化
> 2. Spring将值和bean的引用注入到bean对应的属性中
> 3. 如果bean实现了BeanNameAware接口，Spring将bean的ID传递给setBeanName()方法
> 4. 如果bean实现了BeanFactoryAware接口，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入
> 5. 如果bean实现了ApplicationContextAware接口，Spring将调用setApplicationContext()方法，将bean所在的应用上下文的引用传入进来
> 6. 如果bean实现了BeanPostProcessor接口，Spring将调用它们的postProcessBeforeInitialization()方法
> 7. 如果bean实现了InitializingBean接口，Spring将调用它们的afterPropertiesSet()方法，类似地，如果bean使用init-method声明了初始化方法，该方法也会被调用
> 8. 如果bean实现了BeanPostProcessor接口，Spring将调用它们的postProcessAfterInitialization()方法
> 9. 此时，bean已经准备就绪，可以被应用程序使用了，它们将一直驻留在应用上下文中，直到该应用上下文被销毁
> 10. 如果bean实现了DisposableBean接口，Spring将调用它的destory()接口方法。同样，如果bean使用destory-method声明了销毁方法，该方法也会被调用

#### Spring ioc容器

> 容器负责管理应用中组件的生命周期和组件间的依赖

#### Spring的装配bean的三种方式

> 自动化配置、基于Java的显示配置、基于XML的显示配置

#### Spring，环境与profile，条件化bean，处理自动装配的歧义性，运行值注入

> 环境与profile：配置不同环境的配置，如数据源的配置；注解使用@Profile；应用上下文根据哪个profile处于激活状态进行加载：active优先，再到default
>
> 条件化bean：即在特定条件下才会创建bean；注解使用@Conditional
>
> 处理自动装配的歧义性：由于自动装配只能装备单个bean，如果有多个相同的则会报错；注解使用 限定符@Qulifier
>
> 运行值注入：①属性占位符 ②SpEL表达式（高级推荐，拥有很多特性）

#### Spring bean 作用域

> bean默认的作用域是单例，修改使用注解@Scope，原型或单例作用域常量使用ConfigurableBeanFactory.SCOPE_#；WEB的作用域常用使用WebApplicationContext.SCOPE\_#，proxyMode=ScopedProxyMode设置代理模式：使用JDK动态代理还是CGLIB代理
>
> 1. singleton：单例
> 2. prototype：原型
> 3. request：请求
> 4. session：会话
> 5. application：WEB应用上下文

#### Spring 面向切面

>AOP：advice通知、pointcut切点、join point连接点
>
>Spring AOP：意思让切面插入到方法执行的周围，基于动态代理实现，即Sring对AOP的支持只能方法拦截；
>
>AspectJ框架，不仅支持方法，还能属性与构造方法；

#### Spring MVC工作流程

> 1. 客户端发送请求至前端控制器DispatcherServlet
> 2. 前端控制器会查询处理映射器（handler mapping）
> 3. 处理映射器根据URL信息找到对应的控制器
> 4. 控制器处理请求信息
> 5. 控制器处理完请求信息后，它将请求连同ModelAndView对象发送回前端控制器DispatcherServlet
> 6. 前端控制器DispatcherServlet将会使用视图解析器（view resolver）进行解析，得到特定视图
> 7. 前端控制器DispatcherServlet将使用数据库模型渲染输出的视图返回给客户端

#### Spring MVC 常用到的技术

> 配置 DispatcherServlet 和 ContextLoaderListener 上下文
>
> 上传文件：
>
> - 配置multipart解析器处理multipart形式的数据，使用MultipartFile接受上传文件，可配置上传路径、文件大小、请求总大小等
> - 在低版本Servlet上，可以使用part形式接受上传文件，它不需要配置MultipartResolver
>
> 处理异常：
>
> - 特定的Spring异常将会自动映射为指定的HTTP状态码
> - 异常类上使用@ResponseStatus注解，从而将其异常映射到一个HTTP状态码
> - 使用控制器通知（类上注解：@ControllerAdvice），在其方法上使用@ExceptionHandler注解，使用其处理异常
>
> 跨重定向请求传递数据：
>
> - 通过URL模板进行重定向（值只能传递简单的 字符串和数字类型）
> - 使用flash属性，即用RedirectAttributes添加flash属性（值可以传递对象）

