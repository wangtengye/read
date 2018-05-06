#### Spring IoC三种注入方式
- 构造方法注入
- setter方法注入
- 接口注入（由于被注入对象实现不必要的接口，带有侵入性，基本用不到）

#### Spring IoC容器
- BeanFactory
- ApplicationContext
![](https://i.imgur.com/gmuqPOP.png)

#### BeanFactory对象绑定方式
- 直接编码
  通过`DefaultListableBeanFactory`注册`BeanDefinition`，每一个受管的对象，在容器中都会有一个`BeanDefinition`的实例（ `instance`）与之相对应，该`BeanDefinition`的实例负责保存对象的所有必要信息，包括其对应的对象的class类型、是否是抽象类、构造方法参数以及其他属性等。
![](https://i.imgur.com/D6N0p0k.png)
- 外部文件
  - properties
  `PropertiesBeanDefinitionReader`读取文件
  - xml
  `XmlBeanDefinitionReader`读取文件
- 注解

#### beans
- default-lazy-init
- default-autowire
- default-dependency-check
- default-init-method
- default-destroy-method

#### bean scope
- singleton
- prototype
- request
- session
- global session
- 自定义 `Scope`接口

#### FactoryBean
通过`id`获得是工厂生产的对象，通过`&id`获取真正的工厂对象。

#### BeanFactory
- 容器启动阶段
- Bean实例化阶段
![](http://7xpuj1.com1.z0.glb.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170513112138.png)
- BeanFactoryPostProcessor
 容器第一阶段后对于`BeanDefinition`做些修改
 - `PropertyPlaceholderConfigurer`
 - `PropertyOverrideConfigurer`
 - `CustomEditorConfigurer`
- 第一阶段后并不是立马初始化Bean,只有显式或隐式调用`getBean()`，`Beans`才会实例化
- Bean生命周期
![](https://pic1.zhimg.com/80/v2-baaf7d50702f6d0935820b9415ff364c_hd.jpg)
- BeanWrapper
- Aware接口
  - BeanNameAware
  - BeanClassLoaderAware
  - BeanFactoryAware
  - ResourceLoaderAware
  - ApplicationEventPublisherAware
  - MessageSourceAware
- BeanPostProcessor
- InitializingBean `<init-method>`
- DisposableBean `<destroy-method>`
  调用销毁方法

#### Application Context
- 相比`BeanFactory`，增加了对于`PostProcessor`的自动识别，`bean`的自动初始化，国际化的信息支持，容器内事件发布等
- 实现类
  - FileSystemXmlApplicationContext
  - ClassPathXmlApplicationContext
  - XmlWebApplicationContext
- 资源加载
  - Resource 
    - ByteArrayResource
    - ClassPathResource
    - FileSystemResource
    - URLResouce
    - InputStreamResource
  - ResourceLoader
    - DefaultResourceLoader
      - 若路径以`classpath:`开头 ，构造`ClassPathResource`
      - 否则，通过URL定位资源（有协议前缀：`file:`,`http:`）
      - 如无法定位，通过`getResourceByPath(String)`，默认返回`ClassPathResource`
    - FileSystemResourceLoader
      - 覆盖 `getResourceByPath(String)`，返回`FileSystemResource`
  - ResourcePatternResolver 返回多个Resource
    - `PathMatchingResourcePatternResolver`
      - 确定资源路径后委托给内部的`ResourceLoader` 来查找和定位资源
      - 不指定`ResourceLoader`，则默认`DefaultResourceLoader`
  - AbstractApplicationContext作为ResourceLoader和ResourcePatternResolver ![](https://i.imgur.com/Usb78Di.png)
- 国际化信息支持 
  - Locale
  - ResourceBundle
  - MessageSource
    - StaticMessageSource
    - ResourceBundleMessageSource
    - ReloadableResourceBundleMessageSource
  - MessageSourceAware和MessageSource的注入 
- 容器内部事件发布
  - 自定义事件发布
    - EventObject
    - EventListener
  - Spring 的容器内事件
    - ApplicationEvent
    - ApplicationListener `Application自动加载`
    - ApplicationContext作为事件发布者
- 多配置模块加载的简化

#### IOC扩展
- 注解 
  - `@Autowired`(`byType)` 原理：反射机制，`AutowiredAnnotationBeanPostProcessor`
    - 属性
    - 构造方法
    - 方法（不局限与setter）
  - `@Resource`(`byName`)
  - `<context:annotation-config>`
  - `<context:component-scan>`
  - `@Component`
  - `@Repository`
  - `@Service`
  - `@Controller`

#### AOP
- 静态
  - `AspectJ` 
  - 编译为字节码,类加载慢，运行快
- 动态
  - `JDK`代理
    - 代理类必须实现接口
  - `CGLIB`代理
    - 代理类可以不用实现接口，通过子类方式增强,所以代理类不能是`final`类    
  - 运行时动态代理，类加载快，运行慢  
- 相关概念
  - `Joinpoint`：`AOP`的织入点
  - `Pointcut`：一组`Joinpoint`
  - `Advice`：织入点需要执行的逻辑
    - `BeforeAdvice`
    - `AfterAdvice`
    - `AroundAdvice`
    - `Introduction`
  - `Aspect`   