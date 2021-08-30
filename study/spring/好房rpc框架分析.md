1. - 
     结合调用链，分析每个类的作用，思考它的设计模式。

     1. jar包截图

        image2019-3-14_9-25-28.png

        。

        - ![平安城科建管云研发团队 > 19/03/14-好房rpc框架分析 > image2019-3-14_9-25-28.png](/Users/harlan/b/note/zlinks/pic/hf.png)。

     2. 首先了解下spring启动大体过程

        - 容器启动阶段包括Bean创建过程中 ： 调用实现BeanFactoryPostProcessor接口的类   用来解析xml ,解析现在流行的javaBean配置，替换数据库占位符等。这个会随着加载Bean配置被PostProcessorRegistrationDelegate穿插调用。
        - Bean初始化前：加载完所有的配置后，对于每个Bean的创建都会调用实现BeanPostProcessor接口的类  比如ApplicationContextAwareProcessor， 这个类是用来处理Aware接口的，项目常用的SpringContextHolder即实现了Aware。
        - Bean初始化：创建对象，不说了。
        - Bean初始化后：调用实现BeanPostProcessor接口的类 可以记录创建后的时间等。
        - 在容器所有Bean加载和初始化完毕后：调用实现SmartLifecycle接口的类。
        - 另外，Spring事件类：ApplicationEvent  ApplicationListener， 这是spring代为实现的一个普通的同步的观察者模式。自己写也用不了多少代码。

     3. 全局起点EnableHaofangRPC类

        - 

          ````java @Import({HaofangRPCConfiguration.RPCPackageRegistrar.class, HaofangRPCConfiguration.class, RPCSwaggerConfiguration.RPCSwaggerDocketRegistrar.class})`

          @Import 起源自xml中的import标签，这里import了三个类，但是分为两种：

        1. 1. 此类实现 ImportBeanDefinitionRegistrar 接口 ，spring会在容器启动前阶段调用此接口的registerBeanDefinitions方法。
              1. HaofangRPCConfiguration.RPCPackageRegistrar  --将EnableHaofangRPC 注解中包扫描配置保存到HaofangRPCConfiguration中。
              2. RPCSwaggerConfiguration.RPCSwaggerDocketRegistrar --根据enableSwaggerSupport 决定是否启用swagger ，启用则加载相关配置，后面再说。
           2. 普通类，HaofangRPCConfiguration相当于把此类的内容放到当前类中。结合@Configuration故import的类的@Bean注解可以生效。

     4. **代码走到HaofangRPCConfiguration**

        - RpcFactory，这是一个缓存，用来缓存rpc生产方和消费方，数据在后面初始化。

        - RPCPostProcessor，这是一个处理器，什么时候处理呢？发现它实现了三大接口，spring按自己的顺序依次执行对应接口方法。

          - BeanFactoryPostProcessor

            执行postProcessBeanFactory  扫描包，找到带有@RPCInterface注解的接口(消费方)生成动态代理，代理类RPCClientProxy。

            ````java RPCClientProxy factory = new RPCClientProxy(clazz, rpcFactory); return Proxy.newProxyInstance(clazz.getClassLoader(), new Class<?>[]{clazz}, factory);`

            然后注册到beanFactory，这个时间点注册以便后面创建Bean时依赖注入。

            

          - BeanPostProcessor

            ```
            postProcessBeforeInitialization -- do nothing
            ```

            postProcessAfterInitialization方法，负责扫描包，处理生产方。

            - 把刚刚创建的Bean、标记@RpcExporter的方法等封装成RPCInvoker对象，然后交给RpcFactory保管。

              

          - ApplicationListener

            ```
            onApplicationEvent(ContextRefreshedEvent event)
            ```

            - Spring容器初始化完成会触发该事件。

            - 遍历所有标记@RpcClient的方法，把url，执行器RPCExecutorType等封装成RPCContext对象，然后交给RpcFactory保管。

              - RPCExecutor目前只有一种实现方式

                ````java RESTTEMPLATE(RestTemplateRPCExecutor.class);`

                

            - 

              ````java RPC.setRpcFactory(rpcFactory);                       // 把缓存交给RPC保管 RPC.setMessageSource(context.getBean(MessageSource.class));  //支持国际化`

              

            - 至此rpc已完成所有初始化(不考虑swagger)。

     5. **运行时之消费者**

           由于BeanFactoryPostProcessor对消费者进行了动态代理，所以我们看RPCClientProxy的invoke方法，执行步骤如下：

        - 

          ````java RPCContext context = rpcFactory.getClient(clazz.getName(), method.getName()); Type returnType = method.getGenericReturnType(); Object result =         RPC.executeForObject(context, context.getContainerType(), context.getRequestType(), returnType, args);`

          

        - 从rpcFactory拿出之前缓存的RPCContext，拼接参数，进行HTTP请求。

        - 把结果反序列化。

     6. **运行时之生产者**

          入口是RPCFilter.doFilter()方法

     7. - 

          ````java RPCInvokeDTO dto = RPC.rpcInvoke(content);         //content 是从request中解析的参数`

          

        - 解析入参为params。

        - ```
          找到rpcFactory保管的RPCInvoker对象，利用反射执行方法。
          ```

          ````java returnVal = invoker.getMethod().invoke(invoker.getBean(), params);`

          

        - 无异常则返回，异常的化支持异常信息的国际化。

        - 写入response，结束。

          

          

          

     8. **话分两头，如果启用了swagger，来看swagger的初始化**

        - 启用RpcSwaggerConfiguration

          ````java //启用RPC Swagger if (enableSwaggerSupport) {     //手动注入RPCSwaggerConfiguration类的实例     BeanDefinitionBuilder beanDefinitionBuilder = BeanDefinitionBuilder.rootBeanDefinition(RPCSwaggerConfiguration.class);     registry.registerBeanDefinition("rpcSwaggerConfiguration", beanDefinitionBuilder.getBeanDefinition());     //添加对spring-boot-devtools的支持     RPCSwaggerCache.RPC_SWAGGER_PROPERTIES.clear();     RPCSwaggerCache.API_DESCRIPTIONS.clear(); }`

          

        - RPCSwaggerPostProcessor

          - 执行postProcessAfterInitialization方法

            扫描有@RPCExporter注解的方法，查看是否有swagger相关注解，然后把相关名称，类等封装成RPCSwaggerProperty缓存到RPCSwaggerCache，其中用到byte-buddy动态字节码生成技术，创造了新的类，以便swagger页面显示参数。

        - RPCSwaggerSupport

          - 实现了SmartLifecycle接口
            把缓存的RPCSwaggerProperty转化成swagger需要的形式，并且缓存到RPCSwaggerCache。

        - RPCApiDescriptionProvider

          - 实现了ApiListingScannerPlugin接口，这个接口被swagger调用。
            使用RPCSwaggerCache    至此swagger初始化完成。

     9. swagger的接口调用

        入口是RPCSwaggerFilter

        .doFilter()方法

        - 大体和RPCFilter.doFilter()相似，只是拦截器路径，入参params解析不同，这里就不赘述了。











 Object result =
                RPC.executeForObject(context, context.getContainerType(), context.getRequestType(), returnType, args);

    Type returnType = method.getGenericReturnType();
    rpcContext.setConnectionTimeout(rpcClient.connectionTimeout() > 0
                                ? rpcClient.connectionTimeout() : config.getConnectionTimeout());
                        rpcContext.setReadTimeout(
                                rpcClient.readTimeout() > 0 ? rpcClient.readTimeout() : config.getReadTimeout());
                        rpcContext.setDefaultErrorMessage(StringUtils.isNotBlank(rpcClient.defaultErrorMessage()) ?
                                rpcClient.defaultErrorMessage() : config.getDefaultErrorMessage());
                        rpcContext.setJsonrpc(config.getJsonrpc());
                        rpcContext.setExecutor(rpcClient.executor());
                        rpcContext.setMethod(rpcClient.value());
                        rpcContext.setSuccessCode(rpcClient.successCode());
                        rpcContext.setUrl(config.getUrl());
                        rpcContext.setProxyHost(config.getProxyHost());
                        rpcContext.setProxyPort(config.getProxyPort());
                        rpcContext.setContainerType(rpcClient.containerType());  !! RPCResult.class;
                        rpcContext.setRequestType(rpcClient.requestType());  !!  RPCRequest.class
                        rpcContext.setPrintResponse(config.isPrintResponse());


​                        

                        主动调用的话，把method写对就行了，，RPCResult.class;也不用填，默认就行。。很多信息都是抓取client上的







王辉点评：

## 加载HaofangRPCConfiguration

1. RpcFactory，这是一个缓存，用来缓存rpc生产方和消费方，数据在后面初始化。
   注册表 or 路由表 是否更贴切一点
2. 把刚刚创建的Bean、标记@RpcExporter的方法等封装成RPCInvoker对象，然后交给RpcFactory保管。
   封装成RPCInvoker对象，注册到RpcFactory.invokerMap中，暴露服务
3. 遍历所有标记@RpcClient的方法，把url，执行器RPCExecutorType等封装成RPCContext对象，然后交给RpcFactory保管。
   java interface在编译时动态绑定RPCContext对象，注册到RpcFactory.clientMap中，绑定外部服务

## 运行时之消费者

框架精髓所在，描述运行机制体现其设计思想：配置个时序图,从调用java interface 到 外部服务

## 运行时之生产者

框架精髓所在，描述运行机制体现其设计思想：配置个时序图,接受请求 RPCFilter 到 RPCExporter interface

## 高性能，所有待调用的方法都有缓存，消费者可快速发起请求，生产者只需执行invoke方法

没有缓存，最多算http请求上下文预加载



