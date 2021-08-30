1. ..BeanFactory

2. BeanDefinitionRegistry

3. RootBeanDefinition.setConstructorArgumentValues

4. DefaultListableBeanFactory 继承1，2

5. BeanFactory = PropertiesBeanDefinitionReader(BeanDefinitionRegistry registry) —用来从配置文件读取BeanDefinition

6. XmlBeanFactory extends DefaultListableBeanFactory — 简化写法

7. <constructor-arg **type="int"**> 选择指定构造函数

8. BeanFactory childContainer = new XmlBeanFactory(new ClassPathResource("子容器配置文件路
   径"),parentContainer); 父容器的概

9. <constructor-arg index="0">
    <bean id="djNewsListener" class="..impl.DowJonesNewsListener"> </bean> 

   </constructor-arg>  内部bean 其它类无法引用

10. property支持list set map

11. byName和byType类型的自动绑定模式是针对property的自动绑定  —定义bean时添加属性自动注入，基本用不到

12. <bean parent abstract> 1属性用来继承配置 2是否初始化

13. BeanFactoryPostProcessor 后面讲

14. <bean> 定义时可以不new 而是配置工厂方法

15. FactoryBean  工厂实现此接口 使用此接口时 ，spring自动调用工厂方法，如果是想要工厂 需要getBean("&name")

16. <bean id="mockPersister" class="..impl.MockNewsPersister"> 

    **<lookup-method name="getNewsBean" bean="newsBean"/>** 

    </bean>   方法注入，解决方法获取prototype的问题

17.  BeanFactoryPostProcessor（容器启动前阶段） -> propertyPostProcessor.postProcessBeanFactory(beanFactory);  替换数据库的占位符

18. CustomEditorConfigurer 也是一个子类 里面会应用PropertyEditor来解析xml string->java类

19. SystemProperties 可获取启动参数

20. 继承 PropertyEditorSupport 避免继承所有的方法CustomEditorConfigurer.setCustomEditors(customerEditors)，自定义解析字符格式

21. PropertyEditorRegistrar 实现上面功能  customEditors和PropertyEditorRegistrar都是CustomEditorConfigurer的属性

22. bean的一生

    * BeanPostProcessor

    *  容器启动阶段

    * afterPropertiesSet

    * init-method

    * BeanPostProcessor

      * ApplicationContextAwareProcessor 实现此接口 用于和Aware接口配合
      * 可以自行实现 某个业务类的解码编码 80页

    * ![image-20190312134258481](/note/zlinks/pic/image-20190312134258481.png)

    * 注意：@Bean只是加载配置，并没有创建呢

      

23. Bean的实例化与BeanWrapper 策略模式

    * InstantiationStrategy 策略接口
    * SimpleInstantiationStrategy
    * CglibSubclassingInstantiationStrategy （默认方法）继承上面 支持方法
    * 返回结果通过BeanWrapper包装
    * BeanWrapper 实现PropertyAccessor 可以以统一的方式对对象属性进行访问
    * BeanWrapper 使用CustomEditorConfigurer中的PropertyEditor

24. Aware 接口 当对象实例化完成并且相关属性以及依赖设置完成之后，Spring容器会检查当前对象实例是否实 

    现了一系列的以Aware命名结尾的接口定义。 比如BeanFactoryAware 注入BeanFactory，ApplicationContextAware

    同理注入

25. 对象如果实现InitializingBean 则调用其 afterProperties()方法
    1. beanFactory.initializeBean 会调用init-method方法
    2. 切面可触发initializeBean()  — 如果找不到调用类可能是没下载source文件
26. DisposableBean 销毁时执行
27. registerShutdownHook  spring用此注册销毁

28. ApplicationContext
29. Resource
30. ResourceLoader  虚线是实现
31. ResourcePatternResolver impl -> PathMatchingResourcePatternResolver
    
1. ![image-20190312155956593](/note/zlinks/pic/image-20190312155956593.png)
    
32. p202
33. PropertyEditorRegistry 注册处， BeanWrapper继承此接口
34. PropertyEditorRegistrySupport 干了实事 ，
35. MessageSource
36. 事件发布 p109
37. ApplicationListener 需要被注册到容器中生效



38. **第六章 Spring IoC拓展**

39. AutowiredAnnotationBeanPostProcessor 实现了 BeanPostProcessor 实例化bean定义的
    过程中，来检查当前对象是否有@Autowired标注的依赖需要注入

40. CommonAnnotationBeanPostProcessor  使@Resource生效 （包含在了扫描注解中）

41. ControlFlowPointcut 指定当某个方法被某个类调用时生效

42. DelegatingIntroductionINterceptor 拦截对象而不是类 这块不清楚

43. ProxyFactoryBean p190

    

44. **事务** p366

45. ![image-20190313093535487](../../../note/zlinks/pic/image-20190313093535487.png)

    

46. DataSourceUtil 管理jdbc的连接  hibernate的SessionFactoryUtil

47. TransactionStatus(事务进行时状态) 

    * 包含Savepoint能力

48.  TransactionDefinition(事务属性相关)

    * 传播行为
      * PRPPAGATION_SUPPORTS 可以不定义
      * PRPPAGATION_NOT_SUPPORTS 定义之后无法读取之前业务改的值
      * 嵌套事务 PROPAGATION_NESTED
        * ![image-20190313113243455](../../../note/zlinks/pic/image-20190313113243455.png)
      * 必须有事务
      * 必须没有

49. transactionTemplate 继承了 DefaultTransactionDefinition 故可以直接控制属性

50. TransactionAttribute继承TrabnsactionDefinition 增加rollbackOn方法 异常回滚功能

51. PlatformTransactionManager实现接口TransactionStatus，TransactionDefinition 实现类：

    1. mybatis->DataSourceTransactonManager
    2. jpa->JpaTransactionManager¸A稍微·、
    3. 】=【-098wqaAQWE67\2 ≥

52. DataSourceTransactionManager作为切入点  p399

53. AbstractPlatformTransactionManager更多关注的是事务处理过程中的总体逻辑，而跟

    具体事务资源相关的处理则交给了具体的子类来实现 （abstract用法）

    * 比如检查rollBackOnly状态

54. transactionImterceptor实现声明式事务 切面

55. threadLocal实现多数据源切换

56. 策略模式 诸如选择cglib还是反射  oo编程思想

57. 分布式事务

    1. ApplicationServer协调TransactionManager和ResouceManager之间的关系

58. MVC  p439

59. org.springframework.transaction.support.TransactionSynchronization

    1. 这个类可以实现事务提交之后做些操作
    2. 统筹了消息，和数据库
    3. 对于数据库内部不应称作 rollback 而是一种异常处理
    4. ![image-20190313204153996](/Users/harlan/b/note/zlinks/pic/image-20190313204153996.png)
    5. 经过约30秒5、6次重试连接失败 后报错
    6. ![image-20190314090044865](../../../note/zlinks/pic/image-20190314090044865.png)
    7. ![image-20190314090120597](../../../note/zlinks/pic/image-20190314090120597.png)































































































































































































































































































end