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