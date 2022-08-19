### spring学习链接
http://c.biancheng.net/spring/attr-injection.html

### 搭建spring项目
上 csdn 看博客就行了，包括普通 java 项目的 spring 写法和基于 maven 的 spring 写法。

### spring底层知识
1、IoC(Inversion of Control)：控制反转。一种设计思想，指导 Java 程序员设计出松耦合、更优良的程序。

spring 通过 IoC 容器管理所有 Java 对象的实例化和初始化，控制对象和对象间的依赖关系。简而言之，创建对象的方式由以往的开发者使用 new 关键词创建改变为统一由 IoC 容器负责管理和创建。

IoC 容器创建并管理的 Java 对象称为 Spring Bean，与使用关键字 new 创建的 Java 对象没有任何区别。

2、为什么要控制反转（为什么要是用IoC容器）？
   
在传统 Java 应用中，存在一个类（调用者）想调用另一个类（被调用者）中的属性和方法的情况，因此需要 new 一个后者的对象。在这种情况下，调用者掌握了被调用者的创建控制权。

在 spring 中，Java 创建对象的控制权掌握在 IoC 容器手中，创建步骤如下：

① 开发人员通过 XML 配置文件、注解、Java 配置类等方式，对 Java 对象进行定义，例如在 XML 配置文件中使用 <bean> 标签、在 Java 类上使用 @Component 注解等。
② Spring 启动时，IoC 容器会自动根据对象定义，将这些对象创建并管理起来。这些被 IoC 容器创建并管理的对象被称为 Spring Bean。
③ 当需要使用某个 Bean 时，直接从 IoC中获取，无需再手动 new。

通过上述步骤可知，原本调用者与被调用者之间的“主动与被动”关系，转变为 IoC 与 Bean 调用者的“主动与被动”关系，控制反转即为原调用者由主动角色变成了被动角色，调用者失去了对对象创建的控制权。

3、DI（Denpendency Injection）：依赖注入。对象和对象之间存在依赖关系，所谓依赖关系，即一个对象会用到另一个对象，即该对象的某个属性为另一个类的对象。

在IoC控制对象的创建的前提条件下，所谓“依赖注入”，即在对象的创建过程中，Spring 自动根据对象间的依赖关系，将当前对象所依赖的对象注入其中。

依赖注入本质上是 Spring Bean 属性注入的一种（ref：引用注入），只不过这个属性刚好是一个对象。

4、IOC工作原理。

IoC 底层通过工厂模式、Java 的反射机制、XML 解析等技术，将代码的耦合度降低到最低限度，其主要步骤如下。

① 在配置文件（例如 Bean.xml）中，对各个对象以及它们之间的依赖关系进行配置；
② 我们可以把 IoC 容器当做一个工厂，这个工厂的产品就是 Spring Bean；
③ 容器启动时会加载并解析这些配置文件，得到对象的基本信息以及它们之间的依赖关系；
④ IoC 利用 Java 的反射机制，根据类名生成相应的对象（即 Spring Bean），并根据依赖关系将这个对象注入到依赖它的对象中。

由于对象的基本信息、对象之间的依赖关系都是在配置文件中定义的，并没有在代码中紧密耦合，因此即使对象发生改变，我们也只需要在配置文件中进行修改即可，而无须对 Java 代码进行修改，这就是 Spring IoC 实现解耦的原理。

5、IoC 容器的实现

① BeanFactory——最简单的IoC容器（顶层接口）
a、懒加载（lazy-load）机制：容器在加载配置文件时并不会立刻创建 Java 对象，只有程序中获取（使用）这个对对象时才会创建。

② ApplicationContext——BeanFactory的子接口，增加了AOP（面向切面编程）、国际化、事务支持等。

a、三个子类：ClassPathXmlApplicationContext（xml，常用）；FileSystemXmlApplicationContext（硬盘文件系统，查找硬盘上的xml配置文件，不常用）；WebXmlApplicationContext（会在一个 web 应用程序的范围内加载在 XML 文件中已被定义的 bean）。

5、bean（配置文件中定义 Bean 的标签）的属性标签
① id：Bean 在 IoC 容器中的唯一标识；
② class：Spring Bean的全类名，IoC 容器根据全类名以反射的方式创建其 Bean 对象；
③ name：作用同id，可以和 id 一起来唯一标识 Bean，不过使用其中一个也够了；
④ scope：Bean 的作用域，见第6条；
⑤ constructor-arg：构造函数注入方式完成 Bean 属性的注入；
⑥ property：setter 方式完成属性注入；
⑦ init-method：指定 Bean 对象的初始化方法（使用指定的构造方法构造 Bean 对象）；
⑧ destory-method：销毁回调（销毁 Bean 对象）；
……未完待续……

6、Bean 的作用域（对应 bean 的属性标签为 scope）
① 默认作用域：singleton，单例作用域，在IoC容器中只会存在一个共享的 Bean 对象实例，并且所有对 bean 的请求，只要 id 与该 bean 定义相匹配，则只会返回 bean 的同一实例。
② prototype 作用域，表示一个 bean 定义对应多个 Bean 对象实例。每次请求 Bean 时都会创建一个新的 Bean 对象，与之前的 Bean 对象不同。Prototype 是原型类型，它在我们创建容器的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象，而且我们每次获取到的对象都不是同一个对象。
注意：一般情况下，如果 Bean 对象的状态会有变化，则作用域设为 property，无变化则设为 singleton。
……未完待续……

7、Bean 的生命周期
Bean的定义——> Bean的初始化——>——Bean的使用——Bean的销毁

8、内部 bean 定义。原理同 Java 内部类的定义， inner bean 即为定义在其他 bean 内部的 bean，因此，在 property 或 constructor-arg 标签下的 bean 即为内部 bean。 
### eg：
   <bean id="outerBean" class="...">
      <property name="target">
         <bean id="innerBean" class="..."/>
      </property>
   </bean>

注意：只需要在 bean 配置文件中体现 bean 间的类与内部类的关系即可，实际上 Java 中的写法还是独立的两个类文件。同理，bean 间的继承关系也只需通过 bean 配置文件来体现，无需在 Java 类的编写上使用 extends 关键字来体现类间继承关系了。

9、Spring 集合注入。
当 Bean 对象的属性为集合类型时，可以以集合的形式完成属性注入。
### eg：
  <!--集合类型为list，可注入一列值，允许重复-->
  <property name="addressList">
    <list>
      <value>INDIA</value>
      <value>Pakistan</value>
      <value>USA</value>
      <value>USA</value>
    </list>
  </property>

  <!--集合类型为set，可注入一列值，不允许重复-->
  <property name="addressSet">
    <set>
      <value>INDIA</value>
      <value>Pakistan</value>
      <value>USA</value>
      <value>USA</value>
    </set>
  </property>

  <!--集合类型为map，可注键值对集合，名称和值的类型可为任意类型-->
  <property name="addressMap">
    <map>
      <entry key="1" value="INDIA"/>
      <entry key="2" value="Pakistan"/>
      <entry key="3" value="USA"/>
      <entry key="4" value="USA"/>
    </map>
    </property>

  <!--集合类行为properties，注入键值对集合，其中名称和值都是字符串类型-->
  <property name="addressProp">
    <props>
      <prop key="one">INDIA</prop>
      <prop key="two">Pakistan</prop>
      <prop key="three">USA</prop>
      <prop key="four">USA</prop>
    </props>
  </property>

  10、spring自动装配Bean
  自动装配指无需用 ref 显式指定 Bean 对象间的依赖关系，而是由Spring容器检查XML配置文件内容，根据某种规则，为调用者Bean注入被依赖的Bean。
  注意：此处仅关心依赖注入，即为调用者 Bean 对象注入被调用者 Bean 对象，不是普通属性注入。

  两种形式：
           在<beans/>标签中指定 default-autowire 属性，对所有的 bean 有效；
           在<bean/>标签中指定 autowire 属性，只对当前 bean 有效。

  default-autowire 与 autowire 的值：
  ① no：不自动装配，必须显式定义，默认配置；
  ② byName：根据 setter 方法名自动进行装配：
    Spring容器查找容器中全部Bean，找出其id与setter方法名去掉set前缀，并小写首字母后同名的Bean来完成注入。如果没有找到匹配的Bean实例，则Spring不会进行任何注入。
  ③ byType：根据 setter 方法的形参类型来自动装配：
    Spring容器查找容器中的全部Bean，如果正好有一个Bean类型与setter方法的形参类型匹配，就自动注入这个Bean；如果找到多个这样的Bean，就抛出一个异常；如果没有找到这样的Bean，则什么都不会发生，setter方法不会被调用。
  ④ constructor: 与byType类似，区别是用于自动匹配构造器的参数：
    如果容器不能恰好找到一个与构造器参数类型匹配的Bean，则会抛出一个异常。
  ⑤ autodetect: Spring容器根据Bean内部结构，自行决定使用constructor或byType
    策略：
    如果找到一个默认的构造函数，那么就会应用byType策略。

  注意：既用了 ref 显式定义依赖关系和自动装配的情况下，显式定义覆盖自动装配。对于复杂的项目，不鼓励使用自动装配，这样会降低依赖透明性。

  11、创建 Bean 的三种方式
  ① 使用构造器创建 Bean
    这是最常见的情况，简单理解就是用构造函数创建对象。
    eg:
    <bean id="Person" class="Person">
        <constructor-arg name="name" value="zhaoqi"></constructor-arg>
        <constructor-arg name="sex" type="boolean" value="true"></constructor-arg>
        <constructor-arg name="age" type="int" value="24"></constructor-arg>
        <constructor-arg name="ID_NUM" value="121212121212122"></constructor-arg>
        <constructor-arg name="personalCar" ref ="Car"></constructor-arg>
    </bean>

  ② 使用静态工厂方法创建 Bean
    在这种情况下，必须指定<bean/>的 class 属性的值，但此时其值不再是 Bean 的全类名，而是 静态工厂类，Spring 通过该属性知道由哪个工厂类来创建 Bean 对象实例。此外，还需使用 factory-method 属性指定静态工厂方法，Spring 最终使用该静态工厂方法创建并返回 Bean 对象实例。如果静态工厂方法需要参数，则使用<constructor-arg.../>元素指定静态工厂方法的参数。
    eg:
    <bean id="myService" class="com.myproject.ba03.ServiceFactory" factory-method="getSomeService"/>
      <constructor-arg value="奔驰"></constructor-arg>
      <constructor-arg value="SUV"></constructor-arg>
      ……
  
  ③ 调用实例工厂方法创建 Bean
    实例工厂方法与静态工厂方法只有一个不同：调用静态工厂方法只需使用工厂类即可，而调用实例工厂方法则需要工厂实例。使用实例工厂方法时，配置Bean实例的<bean.../>元素无须 class 属性，配置实例工厂方法使用 factory-bean 指定工厂实例。
    
    采用实例工厂方法创建Bean的<bean.../>元素时需要指定如下两个属性：
    
    factory-bean: 该属性的值为工厂Bean的id。
    factory-method: 该属性指定实例工厂的工厂方法。

    若调用实例工厂方法时需要传入参数，则使用<constructor-arg.../>元素确定参数值
  eg：
  <bean id="instanceFactoryInstance" factory-bean="myFactory"
          factory-method="createBeanClassInstance"/>
      <constructor-arg value="奔驰"></constructor-arg>
      <constructor-arg value="SUV"></constructor-arg>
      ……

---------------------------------------------written by 王欣泽 --------------------------------------------------
### Aware 接口

```java
Aware接口说明
当Spring容器创建的bean对象在进行具体操作的时候，如果需要容器的其他对象，此时可以将对象实现Aware接口，来满足当前的需要
-> 把容器对象的某些值, 设置到bean对象里面去

for example:
1.实现 ApplicationContextAware 接口, 重写
		void setApplicationContext(ApplicationContext applicationContext) throws BeansException;
		方法, 就可以获取applicationContext进行操作

2.实现 EnvironmentAware 接口, 重写
		void setEnvironment(Environment environment);
		方法, 就可以获取Environment进行操作
      
继承aware接口, 在 invokeAwareMethods 方法中完成属性值的设置工作
doCreateBean -> initializeBean -> invokeAwareMethods, 仅处理三个Aware接口
注意这个方法是private的
      
    /**
     * 回调 bean 中 Aware接口 方法
     *
     * @param beanName
     * @param bean
     */
    private void invokeAwareMethods(String beanName, Object bean) {
        //如果 bean 是 Aware 实例
        if (bean instanceof Aware) {
            //如果bean是BeanNameAware实例
            if (bean instanceof BeanNameAware) {
                //调用 bean 的setBeanName方法
                ((BeanNameAware) bean).setBeanName(beanName);
            }
            //如果bean是 BeanClassLoaderAware 实例
            if (bean instanceof BeanClassLoaderAware) {
                //获取此工厂的类加载器以加载Bean类(即使无法使用系统ClassLoader,也只能为null)
                ClassLoader bcl = getBeanClassLoader();
                if (bcl != null) {
                    //调用 bean 的 setBeanClassLoader 方法
                    ((BeanClassLoaderAware) bean).setBeanClassLoader(bcl);
                }
            }
            //如果bean是 BeanFactoryAware 实例
            if (bean instanceof BeanFactoryAware) {
                // //调用 bean 的 setBeanFactory 方法
                ((BeanFactoryAware) bean).setBeanFactory(AbstractAutowireCapableBeanFactory.this);
            }
        }
    }
```



### ApplicationContext 接口

#### 类图

![image-20220312154015418](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0m9xjej21pw0u0418.jpg)

注意ResourcePatternLoader

#### AbstractApplicationContext

```java
程序启动时会先执行 AbstractApplicationContext 的 static 静态块

AbstractApplicationContext 属性说明:
    // 用来存放beanFactory的后置处理器集合 BFPP
    private final List<BeanFactoryPostProcessor> beanFactoryPostProcessors = new ArrayList<>();
    // 创建上下文的唯一标识, 与 createBeanFactory() 后设置的id值相同
    private String id = ObjectUtils.identityToString(this);
    private String displayName = ObjectUtils.identityToString(this);
    // 激活状态or关闭状态
    private final AtomicBoolean active = new AtomicBoolean();
    private final AtomicBoolean closed = new AtomicBoolean();
    // 锁, 对于refresh和destroy的同步监视器
    private final Object startupShutdownMonitor = new Object();
    // 资源模式处理器
    private ResourcePatternResolver resourcePatternResolver;
    // 刷新前的应用程序监听器集合
    private Set<ApplicationListener<?>> earlyApplicationListeners;
    // 用来存放applicationListeners的集合对象
    private final Set<ApplicationListener<?>> applicationListeners = new LinkedHashSet<>();
    // 刷新前的监听事件集合
    private Set<ApplicationEvent> earlyApplicationEvents;
    
AbstractApplicationContext 两个构造方法:
    public AbstractApplicationContext(@Nullable ApplicationContext parent) {
        this();
        setParent(parent);
    }
    public AbstractApplicationContext() {
        // 在构造方法中会创建资源模式处理器, new PathMatchingResourcePatternResolver()
        // 这个类包含属性: ant表达式的路径匹配器
        // private PathMatcher pathMatcher = new AntPathMatcher(); 创建ant方式的路径匹配器
        this.resourcePatternResolver = getResourcePatternResolver();
    }
		// 在 loadBeanDefinition 时用来解析 location
    @Override
    public Resource[] getResources(String locationPattern) throws IOException {
        return this.resourcePatternResolver.getResources(locationPattern);
    }
```

##### setParent

```java
public void setParent(@Nullable ApplicationContext parent) {
    this.parent = parent;
    if (parent != null) {
        Environment parentEnvironment = parent.getEnvironment();
        if (parentEnvironment instanceof ConfigurableEnvironment) {
            // springMVC会存在父子容器, 需要 merge.
            // 类似方法同 getParentBeanFactory
            getEnvironment().merge((ConfigurableEnvironment) parentEnvironment);
        }
    }
}
```

##### getEnvironment

```java
public ConfigurableEnvironment getEnvironment() {
    if (this.environment == null) {
     	 // 创建抽象父类AbstractEnvironment时, 会调用子类customizePropertySources方法
   		 this.environment = createEnvironment();
    }
    return this.environment;
}
```

#### AbstractRefreshableApplicationContext

```java
AbstractRefreshableApplicationContext 属性说明:
    // 是否允许覆盖bean定义信息
    private Boolean allowBeanDefinitionOverriding;
    // 是否允许循环依赖
    private Boolean allowCircularReferences;
    注意, 这两个属性没有执行初始化, 默认为null. 所以在创建beanFactory时调用customizeBeanFactory方法不会赋值
```

#### AbstractRefreshableConfigApplicationContext

```java
AbstractRefreshableConfigApplicationContext 属性说明:
    // 定义配置路径，默认是个字符串数组
    private String[] configLocations;
```

##### setConfigLocations # resolvePath()

```java
对配置文件进行变量替换${}, 甚至可以替换配置文件名称
protected String resolvePath(String path) {
	return getEnvironment().resolveRequiredPlaceholders(path);
}

① AbstractApplicationContext # getEnvironment 同上
② resolveRequiredPlaceholders 解析必要的占位符
resolveRequiredPlaceholders() 
-> 
AbstractPropertyResolver # doResolvePlaceholders() 
-> 
replacePlaceholders() 
-> 
parseStringValue()
parseStringValue() 是递归调用, 所以可以解析嵌套配置: spring-${abc${def}}.xml

递归调用:
placeholder = parseStringValue(placeholder, placeholderResolver, visitedPlaceholders);
递归后一行:
String propVal = placeholderResolver.resolvePlaceholder(placeholder);
其中, resolvePlaceholder -> PropertySourcesPropertyResolver # getPropertyAsRawString() -> getProperty
convertValueIfNecessary 值转换服务??
将属性 username 取值为 propVal = p'c

result.replace(startIndex, endIndex + this.placeholderSuffix.length(), propVal);
替换配置文件名
```

![image-20220311205717575](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0jmalvj20ey096aa9.jpg)

#### AbstractXmlApplicationContext

```java
AbstractXmlApplicationContext 属性说明:
    // 设置xml文件的验证标志，默认是true
    private boolean validating = true;
    dtd document type definition 和 xsd xml schema definition 定义规范
```



#### ClassPathXmlApplicationContext

```java
ClassPathXmlApplicationContext 属性说明:
	  private Resource[] configResources;
	  
ClassPathXmlApplicationContext 多个构造方法:
	  public ClassPathXmlApplicationContext(
	  				String[] configLocations, boolean refresh, @Nullable ApplicationContext parent)
		  			throws BeansException {
	     	// 调用多个父类的构造方法, 进行相关的对象创建等操作, 包含父类对象属性的赋值操作!
        super(parent);
        // 将配置文件设置到AbstractXmlApplicationContext的属性configLocations当中
        setConfigLocations(configLocations);
        if (refresh) {
        		refresh();
        }
	  }
```
#### AnnotationConfigApplicationContext 



### Environment 接口

#### 类图

![image-20220312175604519](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0my8p4j20s40sumym.jpg)

#### AbstractEnvironment

```java
AbstractEnvironment 构造函数:
    public AbstractEnvironment() {
        customizePropertySources(this.propertySources);
    }
    // 留给子类继承, 在子类生效
    protected void customizePropertySources(MutablePropertySources propertySources) {
    }
    
AbstractEnvironment 属性说明:
		// StandardEnvironment # customizePropertySources, 加载对应的属性资源
    private final MutablePropertySources propertySources = new MutablePropertySources();
```



#### StandardEnvironment

```java
StandardEnvironment 属性说明:
    public static final String SYSTEM_ENVIRONMENT_PROPERTY_SOURCE_NAME = "systemEnvironment";
    public static final String SYSTEM_PROPERTIES_PROPERTY_SOURCE_NAME = "systemProperties";
    
StandardEnvironment 重载方法:
    protected void customizePropertySources(MutablePropertySources propertySources) {
      	propertySources.addLast(
          new PropertiesPropertySource(SYSTEM_PROPERTIES_PROPERTY_SOURCE_NAME, getSystemProperties()));
      	propertySources.addLast(
          new SystemEnvironmentPropertySource(SYSTEM_ENVIRONMENT_PROPERTY_SOURCE_NAME, getSystemEnvironment()));
    }
```



### BeanFactory 接口

#### 类图

![image-20220310173804894](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0e199bj21id0kh0vd.jpg)



```
BeanFactory
HierarchicalBeanFactory 可继承的beanFactory,
	在 springMVC 中有父子容器的概念. 
	子容器找不到bean对象去父容器找 doGetBean() -> getParentBeanFactory()
ConfigurableBeanFactory 可配置的
AbstractAutowireCapableBeanFactory
DefaultListableBeanFactory
```



#### ConfigurableListableBeanFactory 接口

```java
ConfigurableListableBeanFactory 接口方法
    // 自动装配时忽略的类
    void ignoreDependencyType(Class<?> type);
    // 自动装配时忽略的接口
    void ignoreDependencyInterface(Class<?> ifc);
```



#### AbstractBeanFactory

```java
AbstractBeanFactory 属性说明:
    // 必要时使用ClassLoader解析Bean类名称, 默认使用线程上下文类加载器
    private ClassLoader beanClassLoader = ClassUtils.getDefaultClassLoader();
     // 是否缓存bean元数据还是每次访问重新获取它
    private boolean cacheBeanMetadata = true;
```



#### AbstractAutowireCapableBeanFactory

```java
AbstractAutowireCapableBeanFactory 属性说明:
    // 尝试解析循环引用
    private boolean allowCircularReferences = true;
    // 在循环引用的而情况下，是否需要注入一个原始的bean实例
    private boolean allowRawInjectionDespiteWrapping = false;
	
AbstractAutowireCapableBeanFactory 构造方法:
    public AbstractAutowireCapableBeanFactory() {
        super();
        // 忽略要依赖的接口. 在 invokeAwareMethods 中回调
        ignoreDependencyInterface(BeanNameAware.class);
        ignoreDependencyInterface(BeanFactoryAware.class);
        ignoreDependencyInterface(BeanClassLoaderAware.class);
    }
    
    public AbstractAutowireCapableBeanFactory(@Nullable BeanFactory parentBeanFactory) {
        this();
        setParentBeanFactory(parentBeanFactory);
    }
```



#### DefaultListableBeanFactory

实现接口BeanDefinitionRegistry, 定义对BeanDefinition的各种增删改操作

```java
DefaultListableBeanFactory 属性说明:
    // 设置为true时, 定义bean的标签 lookup-method 和 replaced-method才能生效
		private boolean allowBeanDefinitionOverriding = true;
    private final Map<String, BeanDefinition> beanDefinitionMap = new ConcurrentHashMap<>(256);
		private volatile List<String> beanDefinitionNames = new ArrayList<>(256);
```



### BeanDefinition 接口

#### 类图



![image-20220329111219491](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0lhtayj21490f8q4m.jpg)

```java
BeanDefinition 接口属性:

String SCOPE_SINGLETON = ConfigurableBeanFactory.SCOPE_SINGLETON;
		String SCOPE_SINGLETON = "singleton";
		
String SCOPE_PROTOTYPE = ConfigurableBeanFactory.SCOPE_PROTOTYPE;
		String SCOPE_PROTOTYPE = "prototype";
		
// 用户自定义bean的角色类型
int ROLE_APPLICATION = 0;

// 某些复杂的配置类
int ROLE_SUPPORT = 1;

// spring内部使用的bean角色
int ROLE_INFRASTRUCTURE = 2;
```



#### RootBeanDefinition & GenericBeanDefinition

```java
	refresh -> finishBeanFactoryInitialization -> getMergedLocalBeanDefinition
  后面需要合并beanDefinition:
  注意 RootBeanDefinition 和 GenericBeanDefinition 是同级
```





### BeanDefinitionReader 接口

#### 类图

![image-20220313153353025](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0gec9fj20qq0gut9l.jpg)



#### AbstractBeanDefinitionReader

```java
AbstractBeanDefinitionReader 属性:
		private final BeanDefinitionRegistry registry;
		registry, 对 beanDefinition 进行操作
```





### BeanPostProcessor 接口

#### 类图

![image-20220415151409029](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0l38pnj21is0ewdhd.jpg)



#### BeanPostProcessor 接口

```java
BeanPostProcessor 接口方法:

// 初始化方法调用前要进行的处理逻辑
@Nullable
default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		return bean;
}

// 在初始化方法指定后要进行的处理逻辑
@Nullable
default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		return bean;
}
```



#### DestructionAwareBeanPostProcessor 接口

```java

public interface DestructionAwareBeanPostProcessor extends BeanPostProcessor 
接口方法:

// 在bean被销毁前调用
void postProcessBeforeDestruction(Object bean, String beanName) throws BeansException;

// 判断是否要进行销毁，一般情况下都需要
default boolean requiresDestruction(Object bean) {
		return true;
}

```



#### MergedBeanDefinitionPostProcessor 接口

```java

用在AbstractBeanFactory # getMergedLocalBeanDefinition() 中

// 在合并相关beandefinition的时候, 进行相关的处理工作
// spring通过此方法找出所有需要注入的字段，同时做缓存
void postProcessMergedBeanDefinition(RootBeanDefinition beanDefinition, Class<?> beanType, String beanName);

// 用于在BeanDefinition被修改后，清除容器的缓存
default void resetBeanDefinition(String beanName) {
}
```





#### InstantiationAwareBeanPostProcessor 接口

```java
继承自BeanPostProcessor，添加了实例化前，实例化后，属性注入后的处理方法
public interface InstantiationAwareBeanPostProcessor extends BeanPostProcessor 

// 在bean实例化之前调用
@Nullable
default Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) throws BeansException {
		return null;
}

// 在bean实例化之后调用
default boolean postProcessAfterInstantiation(Object bean, String beanName) throws BeansException {
		return true;
}

// 当使用注解的时候，通过这个方法来完成属性的注入
@Nullable
default PropertyValues postProcessProperties(PropertyValues pvs, Object bean, String beanName)
				throws BeansException {
		return null;
}

// 属性注入后执行的方法，在5.1版本被废弃
@Deprecated
@Nullable
default PropertyValues postProcessPropertyValues(
				PropertyValues pvs, PropertyDescriptor[] pds, Object bean, String beanName) throws BeansException {
		return pvs;
}
```



#### SmartInstantiationAwareBeanPostProcessor 接口

```java
继承自InstantiationAwareBeanPostProcessor接口，增加了三个额外处理的方法，由spring内部使用
public interface SmartInstantiationAwareBeanPostProcessor extends InstantiationAwareBeanPostProcessor 

	// 预测bean的类型，主要是在bean还没有创建前我们需要获取bean的类型
	@Nullable
	default Class<?> predictBeanType(Class<?> beanClass, String beanName) throws BeansException {
		return null;
	}
	
	// 完成对构造函数的解析和推断
	// 寻找对应的构造器
  @Nullable
	default Constructor<?>[] determineCandidateConstructors(Class<?> beanClass, String beanName)
			throws BeansException {

		return null;
	}
	
	// 解决循环依赖问题，通过此方法提前暴露一个合格的对象
  default Object getEarlyBeanReference(Object bean, String beanName) throws BeansException {
		return bean;
	}
	
	在 doCreateBean()中调用:
  // 为避免后期循环依赖，可以在bean初始化完成前将创建实例的ObjectFactory加入工厂
  addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));


```





#### AutowiredAnnotationBeanPostProcessor

#### CommonAnnotationBeanPostProcessor

### Aspect 接口

#### 类图

![image-20220531173757792](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0c6geuj21op07hgmq.jpg)





### Advisor 接口

introduction 引界, 引入

对于一个现存的类, introduction 可以为其增加行为, 而不用修改该类的程序 (装饰者模式)



还有 introduction info 接口

#### 类图



![image-20220602161820260](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0ek3o0j20pg0gq410.jpg)







# AbstractApplicationContext # refresh方法

## refresh1 : prepareRefresh();

```java
/**
* 前戏，做容器刷新前的准备工作
* 1、设置容器的启动时间
* 2、设置活跃状态为true
* 3、设置关闭状态为false
* 4、获取Environment对象，并加载当前系统的属性值到Environment对象中
* 5、准备监听器和事件的集合对象，默认为空的集合
*/
prepareRefresh();

// 创建并获取环境对象，验证需要的属性文件是否都已经放入环境中
getEnvironment().validateRequiredProperties();

// 判断刷新前的应用程序监听器集合是否为空，如果为空，则将监听器添加到此集合中
// 注意, 初始化监听器时, springboot有一堆监听器
this.earlyApplicationListeners = new LinkedHashSet<>(this.applicationListeners);
```

### initPropertySource方法及其拓展

```java
// 留给子类覆盖，初始化属性资源
initPropertySources();

see org.springframework.web.context.support.WebApplicationContextUtils # initServletPropertySources
在springMVC中, 完成了属性环境变量的替换工作
思考, 怎么跳到initServletPropertySources方法的??????????????
```



1, 继承具体的类并扩展实现

```java
package com.mashibing.test;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyClassPathXmlApplicationContext extends ClassPathXmlApplicationContext {
    
    // 此处必须要调用父类的构造函数, 才能读取到configLocations
    public MyClassPathXmlApplicationContext(String... configLocations){
        super(configLocations);
    }

    @Override
    protected void initPropertySources() {
    		// 需要验证OS参数不为空
        getEnvironment().setRequiredProperties("OS");
    }
}
```



2, 编写测试类

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new MyClassPathXmlApplicationContext("test2.xml");
        User user = (User) context.getBean("testbean");
        System.out.println("username:" + user.getUserName() + "  " + "email:" + user.getEmail());
    }
}
```



## refresh2 : obtainFreshBeanFactory();

```java
创建容器对象并且完成配置文件的加载

// 创建容器对象：DefaultListableBeanFactory
ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

  obtainFreshBeanFactory -> AbstractRefreshableApplicationContext # refreshBeanFactory 方法源码:
  // 创建DefaultListableBeanFactory对象
  DefaultListableBeanFactory beanFactory = createBeanFactory();
  // 为了序列化指定id，可以从id反序列化到beanFactory对象
  beanFactory.setSerializationId(getId());
  // 定制beanFactory，设置相关属性，包括是否允许覆盖同名称的不同定义的对象以及循环依赖
  // 注意, allowBeanDefinitionOverriding 和 allowCircularReferences 默认 = null
  // 此处可以拓展实现, 重写 customizeBeanFactory
  // 如果子类实现没有调用super.customizeBeanFactory(beanFactory), 则父类方法不会被调用
  customizeBeanFactory(beanFactory); 
  // 初始化documentReader,并进行XML文件读取及解析,默认命名空间的解析，自定义标签的解析
  loadBeanDefinitions(beanFactory);
  this.beanFactory = beanFactory;
  
  obtainFreshBeanFactory -> AbstractRefreshableApplicationContext # refreshBeanFactory -> customizeBeanFactory 方法中:
  protected void customizeBeanFactory(DefaultListableBeanFactory beanFactory) {
      // 如果属性allowBeanDefinitionOverriding不为空，设置给beanFactory对象相应属性，是否允许覆盖同名称的不同定义的对象
      if (this.allowBeanDefinitionOverriding != null) {
         beanFactory.setAllowBeanDefinitionOverriding(this.allowBeanDefinitionOverriding);
      }
      // 如果属性allowCircularReferences不为空，设置给beanFactory对象相应属性，是否允许bean之间存在循环依赖
      if (this.allowCircularReferences != null) {
         beanFactory.setAllowCircularReferences(this.allowCircularReferences);
      }
  }
  
  注意:
  	createBeanFactory -> new DefaultListableBeanFactory, 
  	会执行父类 AbstractAutowireCapableBeanFactory 和 AbstractBeanFactory 的构造方法, 完成相关属性的赋值
```



### 拓展customizeBeanFactory()方法

```java
此方法是用来实现BeanFactory的属性设置，主要是设置两个属性：
   - allowBeanDefinitionOverriding：是否允许覆盖同名称的不同定义的对象
   - allowCircularReferences：是否允许bean之间的循环依赖

public class MyClassPathXmlApplicationContext extends ClassPathXmlApplicationContext {

    MyClassPathXmlApplicationContext(String... locations){
        super(locations);
    }

    @Override
    protected void customizeBeanFactory(DefaultListableBeanFactory beanFactory) {
        // 注意, beanFactory也有这两个set方法, super.set是ApplicationContext调用
        // 初始化ApplicationContext时这两个Boolean值为null, beanFactory中均为true
        super.setAllowBeanDefinitionOverriding(true);
        super.setAllowCircularReferences(true);
        super.customizeBeanFactory(beanFactory);
      	// 可以通过这种方式注入BFPP, 也可以通过配置文件注入Bean
        // new 的BFPP不归spring管理
      	super.addBeanFactoryPostProcessor(new MyBeanFactoryPostProcessor());
    }
}
```



### loadBeanDefinitions

```java
  loadBeanDefinitions 相关重载方法:
  protected void loadBeanDefinitions(DefaultListableBeanFactory beanFactory)
  protected void loadBeanDefinitions(XmlBeanDefinitionReader reader)
  public int loadBeanDefinitions(String... locations) 
  public int loadBeanDefinitions(String location)
  public int loadBeanDefinitions(String location, @Nullable Set<Resource> actualResources)
  public int loadBeanDefinitions(Resource... resources)
  public int loadBeanDefinitions(Resource resource)
  public int loadBeanDefinitions(EncodedResource encodedResource)
  
  BeanDefinitionReader 接口方法
  String[] -> String -> Resource[] -> Resource
  
  // 创建一个xml的beanDefinitionReader，并通过回调设置到beanFactory中 -> 设计模式: 适配器模式
  // DefaultListableBeanFactory 实现接口 BeanDefinitionRegistry, 定义对 BeanDefinition 的各种增删改操作
  // 构造方法: 将 beanFactory 设置到 BeanDefinitionReader 中的 BeanDefinitionRegistry 属性, 后面通过 getRegistry() 获得
  XmlBeanDefinitionReader beanDefinitionReader = new XmlBeanDefinitionReader(beanFactory);
  
---------------------------------------------------------------------------------------------------------------------

  beanDefinitionReader.setEntityResolver(new ResourceEntityResolver(this)); // 读取本地文件库
      EntityResolver : sax sample api for xml 解析xml工具; 还有dom4j
      xml引入的网络地址, 若当前无网络, 则会从spring.schemas文件查找
      spring.schemas文件包含网络地址对应的解析标签文件地址, 指向一堆dtd和xsd文件
      /Users/wangxinze/.m2/repository/org/springframework/spring-beans/5.0.6.RELEASE/spring-beans-5.0.6.RELEASE.jar!/META-INF/spring.schemas文件
        eg:
        http\://www.springframework.org/schema/beans/spring-beans-4.1.xsd=org/springframework/beans/factory/xml/spring-beans.xsd

        
      ResourceEntityResolver 的父类 DelegatingEntityResolver 构造函数
          public DelegatingEntityResolver(@Nullable ClassLoader classLoader) {
          this.dtdResolver = new BeansDtdResolver();
          // 当完成这行代码的调用之后，大家神奇的发现一件事情，schemaResolver对象的schemaMappings属性被完成了赋值操作，但是你遍历完成所有代码后依然没有看到显式调用
          // 其实此时的原理是非常简单的，我们在进行debug的时候，因为在程序运行期间需要显示当前类的所有信息，所以idea会帮助我们调用toString方法，只不过此过程我们识别不到而已
          // 使用idea去debug程序时需调用toString方法, 所以 new PluggableSchemaResolver 的时候
          // 会调用 toString 当中的 getSchemaMappings() 方法, 对schemaMappings属性被完成了赋值操作
          this.schemaResolver = new PluggableSchemaResolver(classLoader);
      }

      dtdResolver的解析路径: "spring-beans" ".dtd"
        		public class BeansDtdResolver implements EntityResolver {
              	private static final String DTD_EXTENSION = ".dtd";
              	private static final String DTD_NAME = "spring-beans";
            }
      schemaResolver的解析路径: "META-INF/spring.schemas"
  					public class PluggableSchemaResolver implements EntityResolver {
              	public static final String DEFAULT_SCHEMA_MAPPINGS_LOCATION = "META-INF/spring.schemas";
            }
 ---------------------------------------------------------------------------------------------------------------------
 
  
  // 注意!!!!
  public int loadBeanDefinitions(String location, @Nullable Set<Resource> actualResources) 方法中
   		// 这里本身是 ClassPathXmlApplicationContext
      // 调用 AbstractApplicationContext # getResource() 完成具体的Resource定位
			// 类图关系如下图
      Resource[] resources = ((ResourcePatternResolver) resourceLoader).getResources(location);
			->
      AbstractApplicationContext # getResources()
      @Override
      public Resource[] getResources(String locationPattern) throws IOException {
        // 创建一个资源模式解析器(其实就是用来解析xml配置文件)
        // return new PathMatchingResourcePatternResolver(this);
        return this.resourcePatternResolver.getResources(locationPattern);
      }

    public interface ResourcePatternResolver extends ResourceLoader {
      	String CLASSPATH_ALL_URL_PREFIX = "classpath*:";
      	Resource[] getResources(String locationPattern) throws IOException;
    }

    public interface ResourceLoader {
        // public static final String CLASSPATH_URL_PREFIX = "classpath:";
        String CLASSPATH_URL_PREFIX = ResourceUtils.CLASSPATH_URL_PREFIX;
        Resource getResource(String location);
        @Nullable
        ClassLoader getClassLoader();
    }

      包含此前缀xml示例:
      <context:property-placeholder location="classpath:db.properties" ></context:property-placeholder>

      执行完 loadBeanDefinitions后, beanFactory 中的 beanDefinitionMap 和 beanDefinitionNames 属性, 已经加载好bean
```

![image-20220706152050814](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0cml7zj21i60nfmzs.jpg)

### 干活方法 doLoadBeanDefinitions (这一步会注入internal BPP)

```java
	// 这个解析过程是由documentLoader完成的, 从String[] -> string -> Resource[] -> resource, 
  // 最终开始将resource读取成一个document文档，根据文档的节点信息封装成一个个的BeanDefinition对象
	// document对象里边包含很多node节点的信息, 包括type, name, value, parent, count...
  Document doc = doLoadDocument(inputSource, resource);
  int count = registerBeanDefinitions(doc, resource);
  
  // 解析标签元素且完成注册功能
  // 注意这个方法是 XmlBeanDefinitionReader 中的方法, 下面同名的方法调用的是 BeanDefinitionDocumentReader 对象
  public int registerBeanDefinitions(Document doc, Resource resource) throws BeanDefinitionStoreException {
      // 对xml的beanDefinition进行解析
      BeanDefinitionDocumentReader documentReader = createBeanDefinitionDocumentReader();
      // getRegistry() 是对象 DefaultListableBeanFactory !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
      // getBeanDefinitionCount 是 beanDefinitionMap 的容量
      int countBefore = getRegistry().getBeanDefinitionCount();
      // 完成具体的解析过程
      // 调用的是 BeanDefinitionDocumentReader 对象的 registerBeanDefinitions() 方法
      documentReader.registerBeanDefinitions(doc, createReaderContext(resource));
      return getRegistry().getBeanDefinitionCount() - countBefore;
	}
	
	doRegisterBeanDefinitions(doc.getDocumentElement()); 方法中:
  // 委托者模式, 创建一个解析器, 进行相关解析处理工作
  this.delegate = createDelegate(getReaderContext(), root, parent);
  ...
  // pre和post空方法, 留给拓展
  preProcessXml(root);
  parseBeanDefinitions(root, this.delegate);
  postProcessXml(root);
  
  parseBeanDefinitions 方法中:
  if (delegate.isDefaultNamespace(ele)) 是否是默认的命名空间
      1. 默认命名空间 parseDefaultElement
      2. 自定义命名空间/用户定义标签 parseCustomElement
  
  // 使用默认的方式进行解析 
  parseDefaultElement 方法中:
  1. parseDefaultElement() -> processBeanDefinition() -> parseBeanDefinitionElement() 
    	-> 重载方法 parseBeanDefinitionElement() -> createBeanDefinition(), 
  		此处 new GenericBeanDefinition(). 后续操作设置相关属性
  2. parseDefaultElement() -> processBeanDefinition() -> registerBeanDefinition()
      // 向ioc容器注册解析得到的beandefinition的地方. 操作beanDefinitionMap, beanDefinitionNames
      BeanDefinitionReaderUtils.registerBeanDefinition(bdHolder, getReaderContext().getRegistry());
        

  parseCustomElement() 源码:
	@Nullable
	public BeanDefinition parseCustomElement(Element ele, @Nullable BeanDefinition containingBd) {
      // 获取对应的命名空间
      String namespaceUri = getNamespaceURI(ele);
      if (namespaceUri == null) {
        return null;
      }
      // 根据命名空间找到对应的NamespaceHandlerspring
      NamespaceHandler handler = this.readerContext.getNamespaceHandlerResolver().resolve(namespaceUri);
      if (handler == null) {
        error("Unable to locate Spring NamespaceHandler for XML schema namespace [" + namespaceUri + "]", ele);
        return null;
      }
      // 调用自定义的NamespaceHandler进行解析
      return handler.parse(ele, new ParserContext(this.readerContext, this, containingBd));
	}

  // 其他命名空间的元素, 使用额外的方式进行解析
  parseCustomElement 方法中:
  // 根据当前标签的命名空间 url 字符串 找到对应的 NamespaceHandler handler 处理类. spring
  NamespaceHandler handler = this.readerContext.getNamespaceHandlerResolver().resolve(namespaceUri);

	------------------------------------------------------------------------------------------------
    
  注意, this.readerContext对象 是在
  doLoadBeanDefinitions() -> registerBeanDefinitions() -> 
  // 注意这里还有个 createReaderContext() 方法
  documentReader.registerBeanDefinitions(doc, createReaderContext(resource));
	this.readerContext = readerContext;
  创建的, 其中 createReaderContext -> getNamespaceHandlerResolver -> createDefaultNamespaceHandlerResolver 
    -> new DefaultNamespaceHandlerResolver(cl) 的构造函数会进行相关属性的赋值  
    
  DefaultNamespaceHandlerResolver 属性说明:
      public static final String DEFAULT_HANDLER_MAPPINGS_LOCATION = "META-INF/spring.handlers";
      private final String handlerMappingsLocation; // 构造时完成赋值
      // 属性标签 : Parser类 map
      private volatile Map<String, Object> handlerMappings; // 懒加载

  debug 时, toString() 方法会调用 getHandlerMappings() 方法(懒加载) 完成 handlerMappings 的赋值
  否则是在 
  parseCustomElement() # this.readerContext.getNamespaceHandlerResolver().resolve(namespaceUri); 
	的 resolve() 方法中显示调用 getHandlerMappings()方法, 完成handlerMappings的赋值
  
  ------------------------------------------------------------------------------------------------

  META-INF目录下 `Spring.handlers` 文件, 每一行是<命名空间=解析类>
	例如spring-context包下, 
  http\://www.springframework.org/schema/context=org.springframework.context.config.ContextNamespaceHandler
	
	每个<命名空间=解析类>会注册很多<标签> (下面每一行) 的解析器, 每一行是<命名空间:标签>对应<该标签所有属性的解析>
    public class ContextNamespaceHandler extends NamespaceHandlerSupport {
      @Override
      public void init() {
        registerBeanDefinitionParser("property-placeholder", new PropertyPlaceholderBeanDefinitionParser());
        registerBeanDefinitionParser("property-override", new PropertyOverrideBeanDefinitionParser());
        registerBeanDefinitionParser("annotation-config", new AnnotationConfigBeanDefinitionParser());
        registerBeanDefinitionParser("component-scan", new ComponentScanBeanDefinitionParser());
        registerBeanDefinitionParser("load-time-weaver", new LoadTimeWeaverBeanDefinitionParser());
        registerBeanDefinitionParser("spring-configured", new SpringConfiguredBeanDefinitionParser());
        registerBeanDefinitionParser("mbean-export", new MBeanExportBeanDefinitionParser());
        registerBeanDefinitionParser("mbean-server", new MBeanServerBeanDefinitionParser());
      }
    }
	
	// 调用自定义的NamespaceHandler进行解析
  // 通过不同的handler对象去解析不同的标签元素
	return handler.parse(ele, new ParserContext(this.readerContext, this, containingBd));
	
	此处会判断是否要完成某些内部bean的bd的加载: 有一堆inter的内部对象, 大部分跟注解有关
    ConfigurationClassPostProcessor
    AutowiredAnnotationBeanPostProcessor
    CommonAnnotationBeanPostProcessor
    EventListenerMethodProcessor
    AspectJAwareAdvisorAutoProxyCreator
    ...
    在 AnnotationConfigUtils 和 AopConfigUtils 中
    
    具体细节, 请跳转至
    @ComponentScan标签注入ConfigurationClassPostProcessor

	-----------------------------------------------------------------------------------------
  -----------------------------------------------------------------------------------------
  -----------------------------------------------------------------------------------------
      
			配置类 AnnotationConfigUtils 中:

	/**
	 * The bean name of the internally managed Configuration annotation processor.
	 */
	public static final String CONFIGURATION_ANNOTATION_PROCESSOR_BEAN_NAME =
			"org.springframework.context.annotation.internalConfigurationAnnotationProcessor";
	ConfigurationClassPostProcessor

	/**
	 * The bean name of the internally managed BeanNameGenerator for use when processing
	 * {@link Configuration} classes. Set by {@link AnnotationConfigApplicationContext}
	 * and {@code AnnotationConfigWebApplicationContext} during bootstrap in order to make
	 * any custom name generation strategy available to the underlying
	 * {@link ConfigurationClassPostProcessor}.
	 * @since 3.1.1
	 */
	public static final String CONFIGURATION_BEAN_NAME_GENERATOR =
			"org.springframework.context.annotation.internalConfigurationBeanNameGenerator";

	/**
	 * The bean name of the internally managed Autowired annotation processor.
	 */
	public static final String AUTOWIRED_ANNOTATION_PROCESSOR_BEAN_NAME =
			"org.springframework.context.annotation.internalAutowiredAnnotationProcessor";
	AutowiredAnnotationBeanPostProcessor

	/**
	 * The bean name of the internally managed Required annotation processor.
	 * @deprecated as of 5.1, since no Required processor is registered by default anymore
	 */
	@Deprecated
	public static final String REQUIRED_ANNOTATION_PROCESSOR_BEAN_NAME =
			"org.springframework.context.annotation.internalRequiredAnnotationProcessor";

	/**
	 * The bean name of the internally managed JSR-250 annotation processor.
	 */
	public static final String COMMON_ANNOTATION_PROCESSOR_BEAN_NAME =
			"org.springframework.context.annotation.internalCommonAnnotationProcessor";
	CommonAnnotationBeanPostProcessor

	/**
	 * The bean name of the internally managed JPA annotation processor.
	 */
	public static final String PERSISTENCE_ANNOTATION_PROCESSOR_BEAN_NAME =
			"org.springframework.context.annotation.internalPersistenceAnnotationProcessor";

	private static final String PERSISTENCE_ANNOTATION_PROCESSOR_CLASS_NAME =
			"org.springframework.orm.jpa.support.PersistenceAnnotationBeanPostProcessor";

	/**
	 * The bean name of the internally managed @EventListener annotation processor.
	 */
	public static final String EVENT_LISTENER_PROCESSOR_BEAN_NAME =
			"org.springframework.context.event.internalEventListenerProcessor";
	EventListenerMethodProcessor

	/**
	 * The bean name of the internally managed EventListenerFactory.
	 */
	public static final String EVENT_LISTENER_FACTORY_BEAN_NAME =
			"org.springframework.context.event.internalEventListenerFactory";
	DefaultEventListenerFactory

	-----------------------------------------------------------------------------------------
  -----------------------------------------------------------------------------------------
  -----------------------------------------------------------------------------------------
    
    配置类 AopConfigUtils 中:
        
      public static final String AUTO_PROXY_CREATOR_BEAN_NAME =
          "org.springframework.aop.config.internalAutoProxyCreator";
     
      static {
        // Set up the escalation list...
        APC_PRIORITY_LIST.add(InfrastructureAdvisorAutoProxyCreator.class);
        APC_PRIORITY_LIST.add(AspectJAwareAdvisorAutoProxyCreator.class);
        APC_PRIORITY_LIST.add(AnnotationAwareAspectJAutoProxyCreator.class);
      }

	-----------------------------------------------------------------------------------------
  -----------------------------------------------------------------------------------------
  -----------------------------------------------------------------------------------------

	parseCustomElement()
  return handler.parse(ele, new ParserContext(this.readerContext, this, containingBd));
  ->
  // 寻找<命名空间:标签>对应的解析器
	BeanDefinitionParser parser = findParserForElement(element, parserContext);
	// 不同的标签不同的处理类, 具体的处理逻辑可能不同, 但最终都会获取到完整的 beanDefinition 对象
	return (parser != null ? parser.parse(element, parserContext) : null);
  -> 
  AbstractBeanDefinitionParser # parse()
  -> 
  AbstractBeanDefinition definition = parseInternal(element, parserContext);
  AbstractSingleBeanDefinitionParser # parseInternal() 
  -> 
  // 调用子类重写的doParse方法进行解析
  doParse(element, parserContext, builder);
  PropertyPlaceholderBeanDefinitionParser # doParse(), 
	其中 super.doParse(element, parserContext, builder); 设置通用的属性值
  
	AbstractBeanDefinitionParser # parse() 方法中
  返回后续完成注册:
  // 将AbstractBeanDefinition转换为BeanDefinitionHolder并注册
  // 将解析完成的对象注册到容器 beanDefinition map 和 names 集合中
  BeanDefinitionHolder holder = new BeanDefinitionHolder(definition, id, aliases);
  registerBeanDefinition(holder, parserContext.getRegistry());
```



### 自定义标签

![image-20220313180551161](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0f1rywj20ro0gkmzc.jpg)

​	1、User.java

```java
package com.mashibing.selftag;

public class User {
  
    private String userName;
  	private String email;

    public String getUserName() {
        return userName;
    }
    public void setUserName(String userName) {
        this.userName = userName;
    }
    public String getEmail() {
        return email;
    }
    public void setEmail(String email) {
        this.email = email;
    }
}
```

​		2、UserBeanDefinitionParser.java

```java
package com.mashibing.selftag;

import org.springframework.beans.factory.support.BeanDefinitionBuilder;
import org.springframework.beans.factory.xml.AbstractSingleBeanDefinitionParser;
import org.springframework.util.StringUtils;
import org.w3c.dom.Element;

// 可以继承 AbstractSingleBeanDefinitionParser 和 AbstractPropertyLoadingBeanDefinitionParser
// 看是否需要 AbstractPropertyLoadingBeanDefinitionParser 中的标签处理方法
public class UserBeanDefinitionParser extends AbstractSingleBeanDefinitionParser {
  
  	@Override
    protected Class<?> getBeanClass(Element element) {
        return User.class;
    }

    protected void doParse(Element element, BeanDefinitionBuilder bean) {
        String userName = element.getAttribute("userName");
        String email = element.getAttribute("email");
        if (StringUtils.hasText(userName)) {
            bean.addPropertyValue("userName", userName);
        }
        if (StringUtils.hasText(email)){
            bean.addPropertyValue("email", email);
        }
    }
}
```

​		3、MyNamespaceHandler.java

```java
package com.mashibing.selftag;

import org.springframework.beans.factory.xml.NamespaceHandlerSupport;

public class MyNamespaceHandler extends NamespaceHandlerSupport {

    public void init() {
        registerBeanDefinitionParser("msb", new UserBeanDefinitionParser());
    }
}
```

​		4、在resource目录下创建META-INF目录下，并创建三个文件

Spring.handlers

```properties
http\://www.mashibing.com/schema/user=com.mashibing.selftag.MyNamespaceHandler
```

Spring.schemas

```properties
http\://www.mashibing.com/schema/user.xsd=META-INF/user.xsd
```

user.xsd

```xml
<?xml version="1.0" encoding="UTF-8"?>
<schema xmlns="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://www.mashibing.com/schema/user"
        xmlns:tns="http://www.mashibing.com/schema/user"
        elementFormDefault="qualified">
    <element name="msb">
        <complexType>
            <attribute name ="id" type = "string"/>
            <attribute name ="userName" type = "string"/>
            <attribute name ="email" type = "string"/>:
        </complexType>
    </element>
</schema>
```

​		5、创建配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aaa="http://www.mashibing.com/schema/user"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.mashibing.com/schema/user http://www.mashibing.com/schema/user.xsd">

    <aaa:msb id = "testbean" userName = "lee" email = "bbb"/>
</beans>
```

​		6、编写测试类

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new MyClassPathXmlApplicationContext("test2.xml");
        User user=(User)context.getBean("testbean");
        System.out.println("username:"+user.getUserName()+"  "+"email:"+user.getEmail());
    }
}
```

### 注解的入口

```java
parseCustomElement -> BeanDefinitionParser#parse() -> ComponentScanBeanDefinitionParser#parse()
```

![image-20220314200813762](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0k5vxdj217q07t0vp.jpg)

## refresh3 : prepareBeanFactory(beanFactory); 

```java
// beanFactory的准备工作，对各种属性进行填充
// 通过一系列 add, set, ignored, register开头的方法 设置属性值
prepareBeanFactory(beanFactory); 

// 设置beanfactory的表达式语言处理器
// SPEL表达式
beanFactory.setBeanExpressionResolver(new StandardBeanExpressionResolver(beanFactory.getBeanClassLoader()));
StandardBeanExpressionResolver 中属性表达式前缀 #{ 后缀 }.   #{}, ${}, 占位符

------------------------------------------------------------------------------------------------------
------------------------------------PropertyEditor------------------------------------------------------
------------------------------------------------------------------------------------------------------
  
// propertyEditor 属性编辑器, 可以自定义. 参考 -> 自定义属性编辑器 property editor
// 为beanFactory增加一个默认的propertyEditor，这个主要是对bean的属性等设置管理的一个工具类
beanFactory.addPropertyEditorRegistrar(new ResourceEditorRegistrar(this, getEnvironment()));
eg: 将 河北省_邯郸市_武安市 解析为 Address 属性

ResourceEditorRegistrar 实现 PropertyEditorRegistrar接口
PropertyEditorRegistrar接口 # registerCustomEditors()方法完成Editor类的注册

------------------------------------------------------------------------------------------------------
  
在 populateBean() 填充属性阶段显示调用 registerCustomEditors() 方法:

AbstractAutowireCapableBeanFactory # instantiateBean() 方法中
  // 父类构造方法会激活标志位 this.defaultEditorsActive = true;
  BeanWrapper bw = new BeanWrapperImpl(beanInstance);
	->
  super(object);
  ->
	AbstractNestablePropertyAccessor 的构造方法:
  protected AbstractNestablePropertyAccessor(Object object) {
		registerDefaultEditors();
		setWrappedInstance(object);
	}
  ->
  protected void registerDefaultEditors() {
		this.defaultEditorsActive = true; // 是否启动默认编辑器, 默认为false
	}

  initBeanWrapper(bw);
  protected void initBeanWrapper(BeanWrapper bw) {
    // 使用该工厂的 ConversionService 来作为 bw 的 ConversionService，用于转换属性值，以替换JavaBeans PropertyEditor
    // 之前是在 finishBeanFactoryInitialization 设置 conversionService 转换服务
    bw.setConversionService(getConversionService());
    // 将工厂中所有PropertyEditor注册到bw中
    // 在此处显示调用!!!!!!!!!!!
    registerCustomEditors(bw);
  }
------------------------------------------------------------------------------------------------------

重点!!!!!!!!!!!!!!!!!
在debug时候发现 invokeBeanFactoryPostProcessors() 触发 CustomEditorConfigurer 类
CustomEditorConfigurer 实现了 BFPP, 其 postProcessBeanFactory() 方法
将其两属性
	private PropertyEditorRegistrar[] propertyEditorRegistrars;
	private Map<Class<?>, Class<? extends PropertyEditor>> customEditors;
添加到beanFactory
  
------------------------------------------------------------------------------------------------------

AbstractAutowireCapableBeanFactory # applyPropertyValues() 方法中, 
	for (PropertyValue pv : original) {
		// 完成 河北省_邯郸市_武安市 到 Address属性的解析
    // /将resolvedValue转换为指定的目标属性对象
		convertedValue = convertForProperty(resolvedValue, propertyName, bw, converter);
	}

	convertForProperty() 
  -> 
  convertForProperty() 
  -> 
  convertForProperty() 
  -> 
  convertIfNecessary() 
  -> 
  TypeConverterDelegate # convertIfNecessary()
  
	// 为requiredType和propertyName找到一个自定义属性编辑器
	PropertyEditor editor = this.propertyEditorRegistry.findCustomEditor(requiredType, propertyName);
  // 使用editor将convertedValue转换为requiredType
  convertedValue = doConvertValue(oldValue, convertedValue, requiredType, editor);
  // 设置属性值
  doConvertValue -> doConvertTextValue -> editor.setAsText(newTextValue);

------------------------------------------------------------------------------------------------------
------------------------------------PropertyEditor------------------------------------------------------
------------------------------------------------------------------------------------------------------

// 添加beanPostProcessor, ApplicationContextAwareProcessor类用来完成某些Aware对象的注入
// 其 postProcessBeforeInitialization() 方法
beanFactory.addBeanPostProcessor(new ApplicationContextAwareProcessor(this));

// 实现 ① 六个aware
EnvironmentAware, EmbeddedValueResolverAware, ResourceLoaderAware, ApplicationEventPublisherAware
MessageSourceAware, ApplicationContextAware
这6个接口是通过ApplicationContextAwareProcessor implements BeanPostProcessor中的
postProcessBeforeInitialization() # invokeAwareInterfaces() 方法实现的

// 实现 ② 三个aware
BeanNameAware, BeanClassLoaderAware, BeanFactoryAware
是通过 invokeAwareMethods() 方法实现的

// 设置要忽略自动装配的接口 ① 六个aware
// 因为这些接口的实现是由容器通过set方法进行注入的，所以在使用autowire进行注入的时候需要将这些接口进行忽略
beanFactory.ignoreDependencyInterface(EnvironmentAware.class);
beanFactory.ignoreDependencyInterface(EmbeddedValueResolverAware.class);
beanFactory.ignoreDependencyInterface(ResourceLoaderAware.class);
beanFactory.ignoreDependencyInterface(ApplicationEventPublisherAware.class);
beanFactory.ignoreDependencyInterface(MessageSourceAware.class);
beanFactory.ignoreDependencyInterface(ApplicationContextAware.class);

// 设置要忽略自动装配的接口 ② 三个aware
在 obtainFreshBeanFactory() -> refreshBeanFactory() -> createBeanFactory() -> new DefaultListableBeanFactory()
  			AbstractAutowireCapableBeanFactory 的构造方法中
        // 忽略要依赖的接口 -- 在 invokeAwareMethod 方法中实现
        ignoreDependencyInterface(BeanNameAware.class);
        ignoreDependencyInterface(BeanFactoryAware.class);
        ignoreDependencyInterface(BeanClassLoaderAware.class);

注意两个增强器 beanFactoryPostProcessor 和 beanPostProcessor
注意beanFactory属性, ignoredDependencyType 和 ignoredDependencyInterfaces
  
  void ignoreDependencyType(Class<?> type);
  void ignoreDependencyInterface(Class<?> ifc);
	方便后续进行统一处理
  
  相关文章: 
	打开BeanFactory ignoreDependencyInterface方法的正确姿势
  https://www.jianshu.com/p/3c7e0608ff1f

------------------------------------------------------------------------------------------------------


思考 @Autowire 如何实现的? 需不需要对应的set方法?

反射在进行值的处理的时候有两种方式:
1.获取该属性对应的set方法进行赋值操作
2.获取到该属性对象Filed, 的set方法设置

在 AbstractAutowireCapableBeanFactory # populateBean() 方法中
  调用 autowireByName() 和 autowireByType()
// 根据autotowire的名称(如适用)添加属性值
if (resolvedAutowireMode == AUTOWIRE_BY_NAME) {
		//通过bw的PropertyDescriptor属性名，查找出对应的Bean对象，将其添加到newPvs中
		autowireByName(beanName, mbd, bw, newPvs);
}
// 根据自动装配的类型(如果适用)添加属性值
if (resolvedAutowireMode == AUTOWIRE_BY_TYPE) {
		//通过bw的PropertyDescriptor属性类型，查找出对应的Bean对象，将其添加到newPvs中
		autowireByType(beanName, mbd, bw, newPvs);
}

------------------------------------------------------------------------------------------------------

				// 类似于 @primary, 注入 bean 标签时 autowire 属性可以选择 byName, byType..
				// 如果按类型匹配到多个, 则 @primary 优先注入
        // 设置几个自动装配的特殊规则,当在进行ioc初始化的如果有多个实现，那么就使用指定的对象进行注入
        beanFactory.registerResolvableDependency(BeanFactory.class, beanFactory);
        beanFactory.registerResolvableDependency(ResourceLoader.class, this);
        beanFactory.registerResolvableDependency(ApplicationEventPublisher.class, this);
        beanFactory.registerResolvableDependency(ApplicationContext.class, this);

          
        // 存放着手动显示注册的依赖项类型-相应的自动装配值的缓存
        private final Map<Class<?>, Object> resolvableDependencies = new ConcurrentHashMap<>(16);
          

------------------------------------------------------------------------------------------------------

  ApplicationListenerDetector 用来检测 bean 是否实现了 ApplicationListener 接口，两个作用：
  	1、实例化完成之后，如果 bean 的单例的并且属于 ApplicationListener 接口，则加入到多播器中
  	2、bean 销毁之前, 如果 bean 是一个 applicationListener, 则从多播器中提前删除
  beanFactory.addBeanPostProcessor(new ApplicationListenerDetector(this));

------------------------------------------------------------------------------------------------------

        // 增加对AspectJ的支持，在java中织入分为三种方式，分为编译器织入，类加载器织入，运行期织入，
  			// 1. 编译器织入是指在java编译器，采用特殊的编译器，将切面织入到java类中，
        // 2. 类加载期织入则指通过特殊的类加载器，在类字节码加载到JVM时，织入切面，
        // 3. 运行期织入则是采用cglib和jdk进行切面的织入
        // aspectj提供了两种织入方式，
  			// 1. 第一种是通过特殊编译器，在编译器，将aspectj语言编写的切面类织入到java类中，
  			// 2. 第二种是类加载期织入，就是下面的load time weaving，此处后续讲
        if (beanFactory.containsBean(LOAD_TIME_WEAVER_BEAN_NAME)) {
            beanFactory.addBeanPostProcessor(new LoadTimeWeaverAwareProcessor(beanFactory));
            // Set a temporary ClassLoader for type matching.
            beanFactory.setTempClassLoader(new ContextTypeMatchClassLoader(beanFactory.getBeanClassLoader()));
        }

				注意类 AspectJWeavingEnabler


```





### 自定义属性编辑器 property editor

![image-20220308191431359](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0fh8g5j20u00wrq6b.jpg)

selfEditor2 采用注解的错误方式 <这是错误例子> ----------------------------------------------------------------------------------------

1. @PropertySource 加错地方

   ConfigurationClassParser # doProcessConfigurationClass()

   在解析 @property source 时的for循环, 

   ```java
   AnnotationConfigUtils.attributesForRepeatable(
           sourceClass.getMetadata(), PropertySources.class,
           org.springframework.context.annotation.PropertySource.class)
   ```

   扫描不到 customer 这个类

   加上这个注解没有意义

   扫描custom + @configuration注解 + @propertySource注解

   annotationConfigApplicationContext, 先register custom.class 再 refresh

2. 最后是set address对象而不是 set text.   ->      this.setValue(address);

selfEditor3 采用注解的正确方式 ----------------------------------------------------------------------------------------

在日常的工作中，我们经常遇到一些特殊的案例需要自定义属性的解析器来完成对应的属性解析工作，大家需要理解它的本质来进行随意的扩展工作,但是此处的扩展没有大家想象的那么简单，详细的流程讲课的时候我大概讲一下，但是要复杂很多。主要有两种方式：

第一种方式：

Address.java

```java
package com.mashibing.propertyEditor;

class Address {
    private String district;
    private String city;
    private String province;

    public String getDistrict() {
        return district;
    }

    public void setDistrict(String district) {
        this.district = district;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    public String getProvince() {
        return province;
    }

    public void setProvince(String province) {
        this.province = province;
    }

    public String toString() {
        return this.province + "省" + this.city + "市" + this.district + "区";
    }
}
```

Customer.java

```java
package com.mashibing.propertyEditor;

public class Customer {
    private String name;
    private Address address;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }
}
```

AddressPropertyEditor.java

```java
package com.mashibing.propertyEditor;

import java.beans.PropertyEditorSupport;

public class AddressPropertyEditor extends PropertyEditorSupport {
    @Override
    public void setAsText(String text) {
        try {
            String[] adds = text.split("-");
            Address address = new Address(); // 注意此处是new了一个Address对象
            address.setProvince(adds[0]);
            address.setCity(adds[1]);
            address.setDistrict(adds[2]);
            this.setValue(address);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

MyPropertyEditorRegistrar.java

```java
package com.mashibing.propertyEditor;

import org.springframework.beans.PropertyEditorRegistrar;
import org.springframework.beans.PropertyEditorRegistry;

public class MyPropertyEditorRegistrar implements PropertyEditorRegistrar {
    @Override
    public void registerCustomEditors(PropertyEditorRegistry registry) {
        registry.registerCustomEditor(Address.class, new AddressPropertyEditor());
    }
}

```

propertyEditor.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="customer" class="com.mashibing.propertyEditor.Customer">
        <property name="name" value="Jack" />
        <property name="address" value="浙江-杭州-西湖" />
    </bean>
    <!--第一种方式-->
    <bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
        <property name="propertyEditorRegistrars">
            <list>
                <bean class="com.mashibing.propertyEditor.MyPropertyEditorRegistrar"></bean>
            </list>
        </property>
    </bean>
    <!--第二种方式-->
    <bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
        <property name="customEditors">
            <map>
                <entry key="com.mashibing.propertyEditor.Address">
                    <value>com.mashibing.propertyEditor.AddressPropertyEditor</value>
                </entry>
            </map>
        </property>
    </bean>
</beans>
```

Test.java

```java
import com.mashibing.ignore.ListHolder;
import com.mashibing.propertyEditor.Customer;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test3 {

    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("propertyEditor.xml");
        Customer c = ac.getBean("customer", Customer.class);
        System.out.println(c.getAddress());
    }
}
```







## refresh4 : postProcessBeanFactory(beanFactory);

```java
// 子类覆盖方法做额外的处理，此处我们自己一般不做任何扩展工作，但是可以查看web中的代码，是有具体实现的
// 在纯 spring 中没有具体实现
postProcessBeanFactory(beanFactory);

与接口 BeanFactoryPostProcessor 接口的方法同名
void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException;
```

## refresh5 : invokeBeanFactoryPostProcessors(beanFactory);

```java
// <实例化>并且调用所有已经注册了的beanFactoryPostProcessor, 遵循指明的顺序
// Must be called before singleton instantiation. 
// 单例对象在初始化之前必须要被调用 --> beanDefinition的修改操作在实例化之前
// 配置文件中的 ${} 属性值对应的替换工作在此处完成
invokeBeanFactoryPostProcessors(beanFactory);

------------------------------------------------------------------------------------------------

question: 没有实例化怎么调用BFPP方法?
String[] postProcessorNames = beanFactory.getBeanNamesForType(BeanDefinitionRegistryPostProcessor.class, true, false);
getBeanNamesForType() 获取的是名字String数组
beanFactory.getBean() -> doGetBean() -> createBean() -> doCreateBean() 包含实例化过程

注意方法注释:
// 实例化并且调用所有已经注册了的beanFactoryPostProcessor, 遵循指明的顺序
refresh # invokeBeanFactoryPostProcessors()

------------------------------------------------------------------------------------------------

// 获取到当前应用程序上下文的 beanFactoryPostProcessors 变量的值，并且实例化调用执行所有已经注册的 beanFactoryPostProcessor
// 默认情况下，通过 getBeanFactoryPostProcessors() 来获取已经注册的BFPP，但是默认是空的
// 如果想拓展怎么处理?
// 拓展到 getBFPP() 方法中, addBFPP(new BFPP), 不归spring管理 
PostProcessorRegistrationDelegate.invokeBeanFactoryPostProcessors(beanFactory, getBeanFactoryPostProcessors());

// 注意, 此处是 DefaultListableBeanFactory, 实现接口 BeanDefinitionRegistry
if (beanFactory instanceof BeanDefinitionRegistry) 

1.用户手动加入的BFPP -> 以方法参数的形式注入 getBeanFactoryPostProcessors() 获得
	如果自定义实现了BFPP接口, 让spring识别可以: 1定义在配置文件中的bean 2调用具体的addBFPP方法
2.实现了BDRPP接口的类 -> 整个工厂中通过类型匹配找到 getBeanNamesForType() 方法
3.实现了BFPP接口的类 -> 整个工厂中通过类型匹配找到 getBeanNamesForType() 方法

注意区分 
1. 父接口 BeanFactoryPostProcessor
		void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException;
2. 子接口 BeanDefinitionRegistryPostProcessor extends BeanFactoryPostProcessor
		void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException;
		注意, 这里子接口继承了 BeanFactoryPostProcessor, 也有 postProcessBeanFactory() 方法
		void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException;
3. 参数接口 BeanDefinitionRegistry 
		对 BeanDefinition 进行修改操作

处理流程:
1. getBeanFactoryPostProcessors()外部集合
2. 子接口 BeanDefinitionRegistryPostProcessor
3. 父接口 BeanFactoryPostProcessor

1.实现了子接口 PriorityOrdered
2.实现了父接口 Ordered
3.没有实现排序接口

注意:
1.  postProcessBeanFactory() 方法当中 beanFactory参数,
		并没有add/register BeanFactoryPostProcessor的操作.
		所以在这个方法中不会新增BFPP, 后续不用循环操获取
    
    但是BDRPP可能会
    在 order 和 no-order 时候都需要重复执行
    postProcessorNames = beanFactory.getBeanNamesForType(BeanDefinitionRegistryPostProcessor.class, true, false);
    需要重复查找的原因在于上面的执行过程中可能会新增其他的BeanDefinitionRegistryPostProcessor
		只有BDRPP需要循环获取
		
2. 	如果一个类实现了 BeanDefinitionRegistryPostProcessor, 
		那么他的 postProcessBeanDefinitionRegistry()方法
		可以跟其他BeanFactoryPostProcessor的一起执行

3. 	addBFPP, new 出来的BFPP不由spring管理, 在入参执行, getBeanNamesForType() 找不到




------------------------------------------------------------------------------------------------



// 用于存放实现了PriorityOrdered接口的BeanFactoryPostProcessor
List<BeanFactoryPostProcessor> priorityOrderedPostProcessors = new ArrayList<>();
// 用于存放实现了Ordered接口的BeanFactoryPostProcessor的beanName
List<String> orderedPostProcessorNames = new ArrayList<>();
// 用于存放普通BeanFactoryPostProcessor的beanName
List<String> nonOrderedPostProcessorNames = new ArrayList<>();

注意 priorityOrderedPostProcessors 存的是BFPP, 其余存的是String, 为啥?
后面执行的时候通过 beanFactory.getBean(ppName, BeanFactoryPostProcessor.class) 获取实例
但是其实都可以

存name的时候size是默认10 (new ArrayList 默认值), 后面 new List 的时候是固定 size
List<BeanFactoryPostProcessor> orderedPostProcessors = new ArrayList<>(orderedPostProcessorNames.size());
List<BeanFactoryPostProcessor> nonOrderedPostProcessors = new ArrayList<>(nonOrderedPostProcessorNames.size());
```





### 执行BFPP的流程图

![执行BeanFactoryPostProcessor的流程图](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0kq4ucj20u30u0dix.jpg)



### 循环注入BDRPP示例代码--为什么要循环获取

```java
public class Teacher {

    private String name;

    public Teacher() {
        System.out.println("创建teacher对象");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Teacher{" +
                "name='" + name + '\'' +
                '}';
    }
}

// 这个是 myself
public class MySelfBeanDefinitionRegistryPostProcessor implements BeanDefinitionRegistryPostProcessor, PriorityOrdered {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException {
        System.out.println("调用执行postProcessBeanDefinitionRegistry--MySelfBeanDefinitionRegistryPostProcessor");
    }

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        System.out.println("调用执行postProcessBeanFactory--MySelfBeanDefinitionRegistryPostProcessor");
    }

    @Override
    public int getOrder() {
        return 0;
    }
}

// 需要在xml文件中注入bean
<bean class = "MyBeanDefinitionRegistryPostProcessor"></bean>

// 这个是my, bean注入的是my
public class MyBeanDefinitionRegistryPostProcessor implements BeanDefinitionRegistryPostProcessor, PriorityOrdered {

    @Override
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException {
        System.out.println("执行postProcessBeanDefinitionRegistry---MyBeanDefinitionRegistryPostProcessor");
      	// 通过 registerBeanDefinition() 在 BDRPP 中注入 BFPP
        BeanDefinitionBuilder builder = BeanDefinitionBuilder.rootBeanDefinition(MySelfBeanDefinitionRegistryPostProcessor.class);
        builder.addPropertyValue("name","zhangsan");
        registry.registerBeanDefinition("msb",builder.getBeanDefinition());
    }

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        System.out.println("执行postProcessBeanFactory---MyBeanDefinitionRegistryPostProcessor");
        BeanDefinition msb = beanFactory.getBeanDefinition("msb");
        msb.getPropertyValues().getPropertyValue("name").setConvertedValue("lisi");
        System.out.println("==============="); 
    }

    @Override
    public int getOrder() {
        return 0;
    }
}

执行顺序
1. PPBDR -- My
2. PPBDR -- MySelf 因为是在my中注入mySelf的, 先执行my
3. PPBF -- My
4. PPBF -- MySelf
```

### BFPP相关子类

```java
BFPP子类:
1. ConfigurationClassPostProcessor <重点必看!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!>
2. CustomAutowireConfigurer 自定义自动注入的配置类
3. CustomEditorConfigurer 自定义属性编辑器
4. CustomScopeConfigurer 自定义作用域
/*
此类用来对@EventListener提供支持
1、解析@EventListener,获取拦截方法
2、对拦截方法进行转换，编程ApplicationListener
3、将转换的ApplicationListener放到Spring容器中
*/
5. EventListenerMethodProcessor
// 对占位符进行解析处理
6. PropertySourcesPlaceholderConfigurer extends PlaceholderConfigurerSupport
7. PlaceholderConfigurerSupport extends PropertyResourceConfigurer 
8. PropertyResourceConfigurer implements BeanFactoryPostProcessor
```



### ConfigurationClassPostProcessor 解析图

![configurationClassPostProcessor解析](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0niofhj20yv0u0acw.jpg)



```java
在这里进行动态代理 (进行增强) 的目的是为了解决 @Bean 的单例问题
如果之前创建过单例对象, 就默认从容器中去取了
```



### @ComponentScan标签注入ConfigurationClassPostProcessor

```java
如果想使用注解开发, 必须开启 @ComponentScan 标签,
解析时会注入
  AnnotationConfigUtils 类中
  CONFIGURATION_ANNOTATION_PROCESSOR_BEAN_NAME =
  		"org.springframework.context.annotation.internalConfigurationAnnotationProcessor"
	对应的类 ConfigurationClassPostProcessor (真正注入的类, implements BeanDefinitionRegistryPostProcessor)
  
  
-----------------------------------------------------------------------------------------------------------

debug时, 如果配置了 @ComponentScan, 解析的时候会加载很多internal...到beanDefinitionMap
<context:component-scan base-package="com.mashibing"></context:component-scan>

(如果使用xml)
在解析的时候, loadBeanDefinitions() -> parseCustomElement() 
-> ComponentScanBeanDefinitionParser # parse() -> registerComponents()

// 组件注册（包括注册一些内部的注解后置处理器，触发注册事件）
// 将internal...放到beanDefinitionMap
registerComponents()

registerComponents源码: 
重点是AnnotationConfigUtils.registerAnnotationConfigProcessors!!!!!!!

boolean annotationConfig = true;
// 判断是否包含annotation-config标签, 没配置, 但是有这个属性值annotation-config, 默认为true
// <context:component-scan base-package="com.mashibing" annotation-config="false/true"></context:component-scan>
if (element.hasAttribute(ANNOTATION_CONFIG_ATTRIBUTE)) {
	// 获取component-scan标签的annotation-config属性值，默认为true
	annotationConfig = Boolean.parseBoolean(element.getAttribute(ANNOTATION_CONFIG_ATTRIBUTE));
}
if (annotationConfig) {
  // 如果annotation-config属性值为true，在给定的注册表中注册所有用于注解的bean后置处理器
  Set<BeanDefinitionHolder> processorDefinitions =
  	AnnotationConfigUtils.registerAnnotationConfigProcessors(readerContext.getRegistry(), source);
  for (BeanDefinitionHolder processorDefinition : processorDefinitions) {
  	// 将注册的注解后置处理器的BeanDefinition添加到compositeDef的nestedComponents属性中
  	compositeDef.addNestedComponent(new BeanComponentDefinition(processorDefinition));
	}
}

在AnnotationConfigUtils中包含很多
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalConfigurationBeanNameGenerator
org.springframework.context.annotation.internalAutowiredAnnotationProcessor

		ConfigurationClassPostProcessor类, 对应internalConfigurationAnnotationProcessor
		将ConfigurationClassPostProcessor注入进BeanFactory
		源码:
		// 注册内部管理的用于处理@configuration注解的后置处理器的bean
		// 	public static final String CONFIGURATION_ANNOTATION_PROCESSOR_BEAN_NAME = "org.springframework.context.annotation.internalConfigurationAnnotationProcessor";
		if (!registry.containsBeanDefinition(CONFIGURATION_ANNOTATION_PROCESSOR_BEAN_NAME)) {
			RootBeanDefinition def = new RootBeanDefinition(ConfigurationClassPostProcessor.class);
			def.setSource(source);
			// 注册BeanDefinition到注册表中
			beanDefs.add(registerPostProcessor(registry, def, CONFIGURATION_ANNOTATION_PROCESSOR_BEAN_NAME));
		}

-----------------------------------------------------------------------------------------------------------

public class ConfigurationClassPostProcessor implements BeanDefinitionRegistryPostProcessor, PriorityOrdered, ResourceLoaderAware, BeanClassLoaderAware, EnvironmentAware
  
ConfigurationClassPostProcessor类是一个后置处理器的类，主要功能是参与BeanFactory的建造，主要功能如下
1、解析 @Configuration 的配置类
2、解析 @ComponentScan 扫描的包
3、解析 @ComponentScans 扫描的包
4、解析 @Import 注解
```



### ConfigurationClassPostProcessor # postProcessBeanDefinitionRegistry() # processConfigBeanDefinitions()



```java
// 构建和验证一个类是否被@Configuration修饰，并做相关的解析工作
// 如果你对此方法了解清楚了，那么springboot的自动装配原理就清楚了
ConfigurationClassPostProcessor # postProcessBeanDefinitionRegistry() # processConfigBeanDefinitions()
包含常用注解的扫描:
@Configuration
@Bean
@Import
@Component
@ComponentScan
@ComponentScans

// 判断当前BeanDefinition是否是一个配置类，并为BeanDefinition设置属性为lite或者full，此处设置属性值是为了后续进行调用
// 如果Configuration配置proxyBeanMethods代理为true则为full
// 如果加了@Bean、@Component、@ComponentScan、@Import、@ImportResource注解，则设置为lite
// 如果配置类上被@Order注解标注，则设置BeanDefinition的order属性值
if (ConfigurationClassUtils.checkConfigurationClassCandidate(beanDef, this.metadataReaderFactory)) {
		// 添加到配置类or注解类的集合信息中
    // for循环, 过滤出 List<BeanDefinitionHolder> configCandidates
		configCandidates.add(new BeanDefinitionHolder(beanDef, beanName));
}

-----------------------------------------------------------------------------------------

checkConfigurationClassCandidate()源码讲解:

String className = beanDef.getBeanClassName();
  bean的名字都是全限定名(包名+类名)吗? 为什么有的名字后面有标识#1 #2 #3?
  BeanNameGenerator 接口用于生成名字, 三个实现子类 命名生成器

// AnnotatedBeanDefinition 所有被组件扫描到的bean
// 通过注解注入的bd都是AnnotatedGenericBeanDefinition，实现了AnnotatedBeanDefinition
public class AnnotatedGenericBeanDefinition extends GenericBeanDefinition implements AnnotatedBeanDefinition
// spring内部的bd是RootBeanDefinition，实现了AbstractBeanDefinition
public class RootBeanDefinition extends AbstractBeanDefinition
// 注意, 此处的bd是ScannedGenericBeanDefinition, 实现了AnnotatedBeanDefinition
public class ScannedGenericBeanDefinition extends GenericBeanDefinition implements AnnotatedBeanDefinition

// 最终都是为了获取 AnnotationMetadata metadata 注解元数据
AnnotationMetadata metadata;
if (beanDef instanceof AnnotatedBeanDefinition 
    && className.equals(((AnnotatedBeanDefinition) beanDef).getMetadata().getClassName())) {
  			metadata = ((AnnotatedBeanDefinition) beanDef).getMetadata();
// 判断是否是spring中默认的BeanDefinition
} else if (beanDef instanceof AbstractBeanDefinition 
    && ((AbstractBeanDefinition) beanDef).hasBeanClass()) {
  			metadata = AnnotationMetadata.introspect(beanClass);
} else {
  			metadata = metadataReader.getAnnotationMetadata();
}

// 获取bean定义的元数据被@Configuration注解标注的属性字典值
Map<String, Object> config = metadata.getAnnotationAttributes(Configuration.class.getName());
// 如果bean被@Configuration注解标注，且属性proxyBeanMethods为false(使用代理模式)，则将bean定义记为full
if (config != null && !Boolean.FALSE.equals(config.get("proxyBeanMethods"))) {
		beanDef.setAttribute(CONFIGURATION_CLASS_ATTRIBUTE, CONFIGURATION_CLASS_FULL);
}
// 如果bean被@configuration注解标注，且被注解@Component，@ComponentScan、@Import、@ImportResource或者@Bean标记的方法，则将bean定义标记为lite
else if (config != null || isConfigurationCandidate(metadata)) {
		beanDef.setAttribute(CONFIGURATION_CLASS_ATTRIBUTE, CONFIGURATION_CLASS_LITE);
} else {
		return false;
}

// 检查给定的元数据，以查找给定的候选配置类是否被指定的注解标注
// @Component，@ComponentScan、@Import、@ImportResource或者@Bean
重点是 isConfigurationCandidate(metadata) 判断

-----------------------------------------------------------------------------------------

继续ConfigurationClassPostProcessor # postProcessBeanDefinitionRegistry() # processConfigBeanDefinitions()

// 配置类的解析类
// 实例化ConfigurationClassParser类，并初始化相关的参数，完成配置类的解析工作
ConfigurationClassParser parser = new ConfigurationClassParser(
		this.metadataReaderFactory, this.problemReporter, this.environment,
    this.resourceLoader, this.componentScanBeanNameGenerator, registry);
->
ConfigurationClassPostProcessor # parse(Set<BeanDefinitionHolder> configCandidates)
parse() 方法三种判断逻辑, 与获取 AnnotationMetadata metadata; 的三种判断逻辑相同
    // 注解类型
    if (bd instanceof AnnotatedBeanDefinition) {
        parse(((AnnotatedBeanDefinition) bd).getMetadata(), holder.getBeanName());
    }
    // 有class对象的
    else if (bd instanceof AbstractBeanDefinition && ((AbstractBeanDefinition) bd).hasBeanClass()) {
        parse(((AbstractBeanDefinition) bd).getBeanClass(), holder.getBeanName());
    } else {
        parse(bd.getBeanClassName(), holder.getBeanName());
    }
->
最后都跳转到 ConfigurationClassParser # processConfigurationClass() 方法
```



### ConfigurationClassParser # processConfigurationClass()

```java
// 判断是否跳过解析
if (this.conditionEvaluator.shouldSkip(configClass.getMetadata(), ConfigurationPhase.PARSE_CONFIGURATION)) {
		return;
}

conditionEvaluator 条件计算器
eg: @Conditional({LinuxCondition.class}) 在linux环境下才加载这个Bean对象

public class WindowsCondition  implements Condition {
    /**
     * @param conditionContext:判断条件能使用的上下文环境
     * @param annotatedTypeMetadata:注解所在位置的注释信息
     * */
    @Override
    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        //获取ioc使用的beanFactory
        ConfigurableListableBeanFactory beanFactory = conditionContext.getBeanFactory();
        //获取类加载器
        ClassLoader classLoader = conditionContext.getClassLoader();
        //获取当前环境信息
        Environment environment = conditionContext.getEnvironment();
        //获取bean定义的注册类
        BeanDefinitionRegistry registry = conditionContext.getRegistry();
        //获得当前系统名
        String property = environment.getProperty("os.name");
        //包含Windows则说明是windows系统，返回true
        if (property.contains("Windows")){
            return true;
        }
        return false;
    }
}

--------------------------------------------------------------------------------------

shouldSkip() 方法源码

// Conditional.class 注解信息中包含的条件判断
// metadata为空或者配置类中不存在@Conditional标签
if (metadata == null || !metadata.isAnnotated(Conditional.class.getName())) {
  	// 不跳过
		return false;
}

// 采用递归的方式进行判断，第一次执行的时候phase为空，向下执行
if (phase == null) {
    // 下面的逻辑判断中，需要进入ConfigurationClassUtils.isConfigurationCandidate方法，主要的逻辑如下：
    // 1、metadata是AnnotationMetadata类的一个实例
    // 2、检查bean中是否使用@Configuration注解
    // 3、检查bean不是一个接口
    // 4、检查bean中是否包含@Component @ComponentScan @Import @ImportResource中任意一个
    // 5、检查bean中是否有@Bean注解
    // 只要满足其中1,2或者1,3或者1,4或者1,5就会继续递归
    if (metadata instanceof AnnotationMetadata &&
        		// ConfigurationClassUtils.isConfigurationCandidate () 方法,
        		// 在 ConfigurationClassPostProcessor # postProcessBeanDefinitionRegistry() # processConfigBeanDefinitions()
        		// ConfigurationClassUtils.checkConfigurationClassCandidate(beanDef, this.metadataReaderFactory) 中也调用过
    				ConfigurationClassUtils.isConfigurationCandidate((AnnotationMetadata) metadata)) {
        // shouldSkip() 基于@Conditional标签判断该对象是否要跳过
        return shouldSkip(metadata, ConfigurationPhase.PARSE_CONFIGURATION);
    }
    return shouldSkip(metadata, ConfigurationPhase.REGISTER_BEAN);
}
......

注意: 
public interface ConfigurationCondition extends Condition {
  // 返回判断Condition的ConfigurationCondition.ConfigurationPhase
	ConfigurationPhase getConfigurationPhase();
  // 判断Condition的不同配置阶段
	enum ConfigurationPhase {
    // @Configuration注解的类，解析的阶段判断Condition，如果Condition不匹配，则@Configuration注解的类不会加载
		PARSE_CONFIGURATION,
		// @Configuration注解的类，实例化为bean的阶段判断Condition，无论Condition是否匹配，@Configuration注解的类都会加载，且类加载优先于Condition的判断
		REGISTER_BEAN
	}
}
--------------------------------------------------------------------------------------

继续ConfigurationClassParser # processConfigurationClass()

// 处理imported的情况, 当前类是否被别的类import ~~~~~~~~~
// 第一次进入的时候，configurationClass的size为0，existingClass肯定为null，在此处处理configuration重复import
// 如果同一个配置类被处理两次，两次都属于被import的则合并导入类，返回，如果配置类不是被导入的，则移除旧的使用新的配置类
ConfigurationClass existingClass = this.configurationClasses.get(configClass);
......
// 如果要处理的配置类configclass在已经分析处理的配置类记录中已存在，合并两者的importBy属性
existingClass.mergeImportedBy(configClass);

// 处理配置类，由于配置类可能存在父类(若父类的全类名是以java开头的，则除外)，所有需要将configClass变成sourceClass去解析，然后返回sourceClass的父类。
// 如果此时父类为空，则不会进行while循环去解析，如果父类不为空，则会循环的去解析父类
// SourceClass的意义：简单的包装类，目的是为了以统一的方式去处理带有注解的类，不管这些类是如何加载的
// 如果无法理解，可以把它当做一个黑盒，不会影响看spring源码的主流程
SourceClass sourceClass = asSourceClass(configClass, filter);
do {
    // 解析各种注解
    sourceClass = doProcessConfigurationClass(configClass, sourceClass, filter);
}
while (sourceClass != null);        

// 将解析的配置类存储起来，这样回到parse方法时，能取到值
this.configurationClasses.put(configClass, configClass);
```



### ConfigurationClassParser # doProcessConfigurationClass()



```java
doProcessConfigurationClass() 方法解析很多注解, 包括
  @Component, @PropertySources, @PropertySource, @ComponentScans, @ComponentScan, @ImportResource, @Bean

-------------------------------------------------------------------------------------
解析@Component

// @Configuration继承了@Component
if (configClass.getMetadata().isAnnotated(Component.class.getName())) {
    // 递归处理内部类，因为内部类也是一个配置类，配置类上有@configuration注解，该注解继承@Component，
  	// if判断为true，调用processMemberClasses方法，递归解析配置类中的内部类
    // 内部类!!!!!!!
    processMemberClasses(configClass, sourceClass, filter);
}

processMemberClasses 方法中会继续调用 processConfigurationClass(), 跨方法的递归!!!!
// 将配置类入栈
this.importStack.push(configClass);
try {
    // 调用processConfigurationClass方法，因为内部类中还可能包含内部类，所以需要在做循环解析，实际工作中是不会有这中情况的
    processConfigurationClass(candidate.asConfigClass(configClass), filter);
} finally {
    // 解析完出栈
    this.importStack.pop();
}

-------------------------------------------------------------------------------------
解析@PropertySources, @PropertySource

用法, 读入参数值:
@PropertySource("classpath:db.properties") 标注在类上
@Value("#{jdbc.url}")
private String url;

通过 processPropertySource() -> addPropertySource()
最终将配置文件的属性加载进 beanFactory 的 MutablePropertySources

-------------------------------------------------------------------------------------
解析@ComponentScans, @ComponentScan

doProcessConfigurationClass()
->
// 解析@ComponentScan和@ComponentScans配置的扫描的包所包含的类
// 比如 basePackages = com.mashibing, 那么在这一步会扫描出这个包及子包下的class，然后将其解析成BeanDefinition
// (BeanDefinition 可以理解为等价于 BeanDefinitionHolder)
Set<BeanDefinitionHolder> scannedBeanDefinitions = this.componentScanParser.parse(componentScan, sourceClass.getMetadata().getClassName());
-> 
// 先获取各种属性
// ......
// 开始执行扫描，最终的扫描器是ClassPathBeanDefinitionScanner
return scanner.doScan(StringUtils.toStringArray(basePackages));
-> 
// 遍历 basePackages
// 扫描basePackage, 将符合要求的bean定义全部找出来
Set<BeanDefinition> candidates = findCandidateComponents(basePackage);
->
return scanCandidateComponents(basePackage);
->
isCandidateComponent(sbd)
/**
* isIndependent:确定底层类是否是独立的，即它是否是顶级类或者嵌套类，他可以独立于封闭类构造
* isConcrete：判断底层类是否表示具体类，即：既不是接口也不是抽象类
* isAbstract：是否被标记为抽象类
* hasAnnotatedMethods：确定基础类是否具有使用给定注解@Lookup类型进行数据的任何方法
*/
return (metadata.isIndependent() && (metadata.isConcrete() || (metadata.isAbstract() && metadata.hasAnnotatedMethods(Lookup.class.getName()))));
->
继续 doScan()
// 如果找到符合规则的类, 注册beanDefinition
registerBeanDefinition(definitionHolder, this.registry);
->
继续 doProcessConfigurationClass()
// 通过上一步扫描包com.mashibing，有可能扫描出来的bean中可能也添加了ComponentScan或者ComponentScans注解.
// 所以这里需要循环遍历一次，进行递归(parse)，继续解析，直到解析出的类上没有ComponentScan和ComponentScans
for (BeanDefinitionHolder holder : scannedBeanDefinitions) {
    BeanDefinition bdCand = holder.getBeanDefinition().getOriginatingBeanDefinition();
    if (bdCand == null) {
    		bdCand = holder.getBeanDefinition();
    }
    // 判断是否是一个配置类，并设置full或lite属性
    if (ConfigurationClassUtils.checkConfigurationClassCandidate(bdCand, this.metadataReaderFactory)) {
        // 通过递归方法进行解析 !!!!!!!!!! 递归again
        parse(bdCand.getBeanClassName(), holder.getBeanName());
    }
}

-------------------------------------------------------------------------------------


// 处理接口的默认方法实现，从jdk8开始，接口中的方法可以有自己的默认实现，因此如果这个接口的方法加了@Bean注解，也需要被解析
processInterfaces(configClass, sourceClass);

```



### @Import

```java
重点讲解解析@Import:

// 处理@Import注解注册的Bean, 这一步只会将import注册的bean变为configurationClass, 不会变成beanDefinition
// 而是在loadBeanDefinitions()方法中变成BeanDefinition, 再放入BeanDefinitionMap中
processImports(configClass, sourceClass, getImports(sourceClass), filter, true);
其中, 注意 getImports() -> collectImports (递归调用)

// 遍历每一个@Import注解的类
for (SourceClass candidate : importCandidates) {
    // 配置类Import引入的类 1.importSelector子类 2.importBeanDefinitionRegistry子类 3.普通类
    if (candidate.isAssignable(ImportSelector.class)) {}
    else if (candidate.isAssignable(ImportBeanDefinitionRegistrar.class)) {}
    else {}
}

/*
 * 此接口是spring中导入外部配置的核心接口，根据给定的条件（通常是一个或多个注解属性）判断要导入哪个配置类
 * 如果该接口的实现类同时实现了一些Aware接口，那么在调用 selectImports() 方法之前先调用上述接口中的回调方法
 * 如果需要在所有的@Configuration处理完再导入，可以实现DeferredImportSelector接口
 */
public interface ImportSelector 接口
// DeferredImportSelector是ImportSelect的子接口，该接口会在所有的@Configuration配置类处理完成后运行
// 当选择器和@Conditional条件注解一起使用时是特别有用的，此接口还可以和接口Ordered或者@Ordered一起使用，定义多个选择器的优先级
public interface DeferredImportSelector extends ImportSelector

// 判断引用选择器是否是DeferredImportSelector接口的实例
// 如果是则应用选择器将会在所有的配置类都加载完毕后加载
if (selector instanceof DeferredImportSelector) {
		// 将选择器添加到deferredImportSelectorHandler实例中，预留到所有的配置类加载完成后统一处理自动化配置类
		this.deferredImportSelectorHandler.handle(configClass, (DeferredImportSelector) selector);
} else {
		// 获取引入的类，然后使用递归方式将这些类中同样添加了@Import注解引用的类
		// selectImports() 对应很多子类, 比较麻烦
		String[] importClassNames = selector.selectImports(currentSourceClass.getMetadata());
		Collection<SourceClass> importSourceClasses = asSourceClasses(importClassNames, exclusionFilter);
		// 递归处理，被Import进来的类也有可能@Import注解
		processImports(configClass, currentSourceClass, importSourceClasses, exclusionFilter, false);
}

// 在此处进行延迟处理 [延迟处理] !!!!!!
回到 ConfigurationClassParser # parse(Set<BeanDefinitionHolder> configCandidates) 方法中, 

// 执行找到的DeferredImportSelector
// DeferredImportSelector 是 ImportSelector的一个子类
// ImportSelector 被设计成和 @Import 注解同样的效果，但是实现了 ImportSelector 的类可以条件性的决定导入某些配置
// DeferredImportSelector 的设计魔都是在所有其他的配置类被处理后才进行处理
this.deferredImportSelectorHandler.process();

public void parse(Set<BeanDefinitionHolder> configCandidates) --- spring boot 中
processDeferredImportSelectors(); --- springboot中 ????????????
->
// 核心处理类，此处完成自动配置功能
handler.processGroupImports();
->
// getImports方法是读取自动化配置类的核心入口
getImports()
->
// 核心方法，在springboot中此方法会去SpringFactoriesLoader类中加载自动化配置类
this.group.process(deferredImport.getConfigurationClass().getMetadata(), deferredImport.getImportSelector());
->
getAutoConfigurationEntry() --- springboot中
->
显示调用 getCandidateConfigurations() --- springboot中
->
getImports().forEach()

----------------------------------------------------------------------------------------------

@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration 

@Import(AutoConfigurationPackages.Registrar.class)
public @interface AutoConfigurationPackage
  
springboot @Import 了两个类:
org.springframework.boot.autoconfigure.AutoConfigurationPackages.Registrar
org.springframework.boot.autoconfigure.AutoConfigurationImportSelector


springboot中才有
AutoConfigurationImportSelector # getCandidateConfigurations()

protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
    List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
    Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
    return configurations;
}
其中,
protected Class<?> getSpringFactoriesLoaderFactoryClass() {
		return EnableAutoConfiguration.class;
}
->
loadFactoryNames()
->
Map<String, List<String>> loadSpringFactories()
->
Enumeration<URL> urls = (classLoader != null ?
		classLoader.getResources(FACTORIES_RESOURCE_LOCATION) :
    ClassLoader.getSystemResources(FACTORIES_RESOURCE_LOCATION));
其中,
public static final String FACTORIES_RESOURCE_LOCATION = "META-INF/spring.factories";

找到
/Users/wangxinze/.m2/repository/org/springframework/boot/spring-boot-autoconfigure/1.5.7.RELEASE/spring-boot-autoconfigure-1.5.7.RELEASE.jar!/META-INF/spring.factories
加入一系列配置类
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
...

```



### spring boot 怎么完成自动装配的?

```java
1. 执行BFPP, 核心处理类 ConfigurationClassPostProcessor, 处理相关注解的解析
		@Component, @PropertySources, @PropertySource, @ComponentScans, @ComponentScan, @Import, @ImportResource, @Bean
2. 在解析@Import的时候, 会从启动类挨个识别, 识别到 AutoConfigurationImportSelector 类
3. 在这个类进行解析的时候, 会有一个延迟加载的属性. 
    在延迟加载的时候, 通过getImports()方法获取AutoConfigurationEnrty对象,
    获取enrty对象时候, 会显示调用getCandidateConfigurations()方法,
4. 可以把配置文件对应的属性值加载进spring, 完成自动装配环节
  
解决循坏依赖:
实例化/初始化分开
把完成实例化, 但未完成初始化的对象提前暴露
```



## refresh6 : registerBeanPostProcessors(beanFactory);

```java
// 注意这里是BPP, 注册的是Bean对象
// 实例化并且注册所有的BPP实例, 这里只是注册功能，真正调用的是getBean()方法
registerBeanPostProcessors(beanFactory);

在 prepareRefresh() 中也可以 beanFactory.addBPP()

spring中的两种bean:
1.spring自带
2.使用定义的

/**
* 定义这个bean的应用
* application：用户
* support:某些复杂配置的一部分
* infrastructure：完全内部使用，与用户无关
*/
private int role = BeanDefinition.ROLE_APPLICATION;
----> 查看 BeanDefinition 定义的其他角色
  
BeanDefinition 中常量定义:
// 单例字符串singleton
String SCOPE_SINGLETON = ConfigurableBeanFactory.SCOPE_SINGLETON;
// 原型字符串prototype
String SCOPE_PROTOTYPE = ConfigurableBeanFactory.SCOPE_PROTOTYPE;
// 用户自定义bean的角色类型
int ROLE_APPLICATION = 0;
// 某些复杂的配置类
int ROLE_SUPPORT = 1;
// spring内部使用的bean角色
int ROLE_INFRASTRUCTURE = 2;

----------------------------------------------------------------------------------------------------------

// 添加BeanPostProcessorChecker(主要用于记录信息)到beanFactory中
beanFactory.addBeanPostProcessor(new BeanPostProcessorChecker(beanFactory, beanProcessorTargetCount));
// 后置处理器的after方法，用来判断哪些是不需要检测的bean
BeanPostProcessorChecker # postProcessAfterInitialization()

registerBeanPostProcessors 中会
->
//将 postProcessor 添加到beanFactory,它将应用于该工厂创建的Bean。在工厂配置期间调用
beanFactory.addBeanPostProcessor(postProcessor);
->
对应后面的 getBeanPostProcessor() // 没找到

为什么要重新add 一下? 要放到最后
// 注册ApplicationListenerDetector到beanFactory中
beanFactory.addBeanPostProcessor(new ApplicationListenerDetector(applicationContext));
注意: 在 prepareBeanFactory() 中也注册过一次: 
beanFactory.addBeanPostProcessor(new ApplicationListenerDetector(this));

```



![registerBeanPostProcessor的解析过程](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0h73bhj20u00xxgp4.jpg)



## refresh7 : initMessageSource();

```java
// 为上下文初始化message源，即不同语言的消息体，国际化处理,在springmvc的时候通过国际化的代码重点讲
initMessageSource();


// 首先判断是否已有xml文件定义了id为messageSource的bean对象
// 主要是设置 MESSAGE_SOURCE_BEAN_NAME = "messageSource"
if (beanFactory.containsLocalBean(MESSAGE_SOURCE_BEAN_NAME)) {

} else {
    // 如果没有xml文件定义信息源对象，新建DelegatingMessageSource类作为messageSource的bean
    DelegatingMessageSource dms = new DelegatingMessageSource();
    // 给这个DelegatingMessageSource添加父类消息源
    dms.setParentMessageSource(getInternalParentMessageSource());
    this.messageSource = dms;
    // 将这个messageSource实例注册到bean工厂中
    beanFactory.registerSingleton(MESSAGE_SOURCE_BEAN_NAME, this.messageSource);
}
          

// 此接口提供了处理消息的策略，包含了信息的国际化和包含参数的信息的替换
public interface MessageSource {
  /*
   * 解析code对应的信息进行返回，如果对应的code不能被解析，则返回默认信息defaultMessage
	 * code:需要进行解析的code，对应资源文件中的一个属性名
	 * args:需要用来替换code对应的信息中包含参数的内容
	 * defaultMessage: 当对应code的信息不存在时需要返回的默认值
	 * locale:对应的Locale对象
	 */
  String getMessage(String code, @Nullable Object[] args, @Nullable String defaultMessage, Locale locale);
	/*
	 * 解析code对应的信息进行返回，如果对应的code不能被解析则抛出异常NoSuchMessageException
	 * code:需要进行解析的code，对应资源文件中的一个属性名
	 * args:需要用来替换code对应的信息中包含参数的内容
	 * locale:对应的Locale对象
	 */
  String getMessage(String code, @Nullable Object[] args, Locale locale) throws NoSuchMessageException;
	// 通过传递的MessageSourceResolvable对应来解析对应的信息
  String getMessage(MessageSourceResolvable resolvable, Locale locale) throws NoSuchMessageException;
}
```



## refresh8 : initApplicationEventMulticaster(); 

### 观察者模式

![观察者模式的 UML 图](https://www.runoob.com/wp-content/uploads/2014/08/observer_pattern_uml_diagram.jpg)



菜鸟教程:

https://www.runoob.com/design-pattern/observer-pattern.html

```java
import java.util.Observable; 观察者类, util包实现

事件驱动

传统  
被观察者  观察者

spring
事件: 被观察者具体要执行的动作  
  			public abstract class ApplicationEvent extends EventObject
监听器: 观察者, 可能存在多个, 接收不同的事件来做不同的处理工作   
        @FunctionalInterface
        public interface ApplicationListener<E extends ApplicationEvent> extends EventListener {
        		void onApplicationEvent(E event);
        }
多播器: 把被观察者 <遍历观察者通知消息> 的操作拿出来委托给一个多播器, 来进行消息的通知, 或者说通过观察者进行不同的操作
事件源: 谁来调用或者执行发布具体的事件

逻辑执行过程:
1.事件源发布不同的事件
2.当发布事件之后, 会调用多播器的方法来进行事件的广播操作, 由多播器去触发具体的监听器去执行操作
3.监听器接收到具体的事件之后, 可以验证匹配是否能处理当前的事件
	可以, 直接处理
	不可以, 不做任何操作

实际代码处理过程:
1.提前准备好n多个事件源
2.初始化多播器(创建多播器对象, 应该包含一个监听器集合)
3.准备好一系列监听器
4.向多播器中注册已经有的监听器
5.准备事件发布, 来通知多播器, 循环调用监听器, 来进行相关的逻辑处理工作
```



```java
// 判断容器中是否存在bdName为applicationEventMulticaster的bd，也就是说自定义的事件监听多路广播器，必须实现ApplicationEventMulticaster接口
// public static final String APPLICATION_EVENT_MULTICASTER_BEAN_NAME = "applicationEventMulticaster";
if (beanFactory.containsLocalBean(APPLICATION_EVENT_MULTICASTER_BEAN_NAME)) {
} else {
  	// 如果没有，则默认采用SimpleApplicationEventMulticaster
		this.applicationEventMulticaster = new SimpleApplicationEventMulticaster(beanFactory);
}

public class SimpleApplicationEventMulticaster extends AbstractApplicationEventMulticaster

其父类
public abstract class AbstractApplicationEventMulticaster
		implements ApplicationEventMulticaster, BeanClassLoaderAware, BeanFactoryAware
		成员变量:
        // 创建监听器助手类，用于存放应用程序的监听器集合，参数是否是预过滤监听器为false
        private final ListenerRetriever defaultRetriever = new ListenerRetriever(false);
		成员方法:
        // 添加应用程序监听器类
        public void addApplicationListener(ApplicationListener<?> listener)

// 封装特定目标监听器的 Hellper 类，允许高效地检索预过滤的监听器
private class ListenerRetriever {
    // ApplicationListener 对象集合
    public final Set<ApplicationListener<?>> applicationListeners = new LinkedHashSet<>();
    // BeanFactory中的applicationListener类型Bean名集合
    public final Set<String> applicationListenerBeans = new LinkedHashSet<>();
}

其父接口
// ApplicationEventMulticaster 接口的实现类可以管理多个ApplicationListener监听器对象，并且发布事件到监听器
public interface ApplicationEventMulticaster {
  // 添加监听器以接受所有的事件通知
	void addApplicationListener(ApplicationListener<?> listener);
  // 从通知列表中删除监听器
	void addApplicationListenerBean(String listenerBeanName);
  // 从通知列表中删除监听器
	void removeApplicationListener(ApplicationListener<?> listener);
  // 删除注册到代理上的所有监听器，在remove调用之后，代理不会对事件通知执行任何操作直到有新的监听器注册
	void removeApplicationListenerBean(String listenerBeanName);
  // 删除注册到代理上的所有监听器，在remove调用之后，代理不会对事件通知执行任何操作直到有新的监听器注册
	void removeAllListeners();
  // 将应用程序事件广播到对应的监听器上
	void multicastEvent(ApplicationEvent event);
  // 将应用程序事件广播到对应的监听器上
	void multicastEvent(ApplicationEvent event, @Nullable ResolvableType eventType);
}

---------------------------------------------------------------------------------

	源码示例:
	SimpleApplicationEventMulticaster # multicastEvent()
	@Override
	public void multicastEvent(final ApplicationEvent event, @Nullable ResolvableType eventType) {
		// 如果eventType不为null就引用eventType; 否则将event转换为ResolvableType对象再引用
		ResolvableType type = (eventType != null ? eventType : resolveDefaultEventType(event));
		// 获取此多播器的当前任务线程池
		Executor executor = getTaskExecutor();
		// getApplicationListeners方法是返回与给定事件类型匹配的应用监听器集合
		// 遍历获取所有支持event的监听器
		for (ApplicationListener<?> listener : getApplicationListeners(event, type)) {
			// 如果executor不为null
			if (executor != null) {
				// 使用executor回调listener的onApplicationEvent方法，传入event
				executor.execute(() -> invokeListener(listener, event));
			}
			else {
				// 回调listener的onApplicationEvent方法，传入event
				invokeListener(listener, event);
			}
		}
	}

---------------------------------------------------------------------------------

// 完成刷新过程，通知生命周期处理器lifecycleProcessor刷新过程，同时发出ContextRefreshEvent通知别人
refresh12: finishRefresh()
->
// 发布最终事件
// 新建ContextRefreshedEvent事件对象，将其发布到所有监听器。
publishEvent(new ContextRefreshedEvent(this));
->
// 多播applicationEvent到适当的监听器
getApplicationEventMulticaster().multicastEvent(applicationEvent, eventType);
->
invokeListener(listener, event);
->
doInvokeListener(listener, event);
->
// 回调listener的onApplicationEvent方法，传入event
listener.onApplicationEvent(event);


springboot 用的监听器是 initialMulticaster() 换汤不换药
这个对象的初始化, 成员变量的初始化, 在类的初始化的时候创建完成, 后面可以直接用
```



## refresh9: onRefresh();

## refresh10: registerListeners();

## refresh11: finishBeanFactoryInitialization(beanFactory)

### 之前讲的

```java
// 一级缓存, 用于保存BeanName和创建bean实例之间的关系
public final Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256);

// 三级缓存, 用于保存BeanName和创建bean的工厂之间的关系
public final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<>(16);

// 二级缓存, 保存BeanName和创建bean实例之间的关系
// 与singletonFactories的不同之处在于:
// 当一个单例bean被放到这里之后，那么当bean还在创建过程中, 就可以通过getBean方法获取到，可以方便进行循环依赖的检测
public final Map<String, Object> earlySingletonObjects = new ConcurrentHashMap<>(16);

// 初始化剩下的单实例（非懒加载的）
refresh11: finishBeanFactoryInitialization(beanFactory);

getBean() --> doGetBean() --> createBean() --> doCreateBean() 
  --> createBeanInstance() --> instantiateBean() --> instantiate() --> instantiateClass()
					1. doGetBean当中:
					sharedInstance = getSingleton(beanName, () -> {
						try {
							// 为给定的合并后BeanDefinition(和参数)创建一个bean实例
							return createBean(beanName, mbd, args);
						}
						catch (BeansException ex) {
							// 显示地从单例缓存中删除实例：它可能是由创建过程急切地放在那里，以允许循环引用解析。还要删除
							// 接收到该Bean临时引用的任何Bean
							// 销毁给定的bean。如果找到相应的一次性Bean实例，则委托给destoryBean
							destroySingleton(beanName);
							// 重新抛出ex
							throw ex;
						}
					});
          // 从beanInstance中获取公开的Bean对象，主要处理beanInstance是FactoryBean对象的情况，如果不是
					// FactoryBean会直接返回beanInstance实例
					bean = getObjectForBeanInstance(sharedInstance, name, beanName, mbd);
					
					getObjectForBeanInstance 方法, sharedInstance 已经是完整对象了
					sharedInstance有可能是FactoryBean, 包含getObject的调用操作
					如果一个接口实现了FactoryBean, 会创建两个对象, 当调用getObject的时候才会返回自定义的创建对象
					getObjectFromFactoryBean
					doGetObjectFromFactoryBean
					object = factory.getObject();
					
					2. getSingleton当中(判断一级二级缓存里面有没有该对象):
          // 创建单例之前的回调,默认实现将单例注册为当前正在创建中
          beforeSingletonCreation(beanName);
          //从单例工厂中获取对象
          singletonObject = singletonFactory.getObject();
          // 创建单例后的回调,默认实现将单例标记为不在创建中
          afterSingletonCreation(beanName);

					其中 singletonFactory 就是传过来的lambda表达式
					会跳转到 AbstractAutowireCapableBeanFactory 的 createBean
					
					3.doCreateBean当中
					doCreateBean -- createBeanInstance 创建bean方法
					doCreateBean -- addSingletonFactory
            addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
            addSingletonFactory 解决循环依赖 -- 提前暴露(只完成实例化, 未完成初始化)
            循环依赖: 1. 构造器的循环依赖(无法解决) 2. set的循环依赖, 实例化和初始化分开
            暴露的是 只完成实例化 未完成初始化的操作 (默认为空的那些对象)
            同时注意getEarlyBeanReference方法
					
					doCreateBean -- populateBean属性注入, 执行完后属性都被赋值
					doCreateBean -- initializeBean - invokeAwareMethods
					doCreateBean -- initializeBean - applyBeanPostProcessorsBeforeInitialization - postProcessBeforeInitialization
					doCreateBean -- initializeBean - invokeInitMethods
					doCreateBean -- initializeBean - applyBeanPostProcessorsAfterInitialization - postProcessAfterInitialization
					
					// 注册bean对象，方便后续在容器销毁的时候销毁对象
					doCreateBean -- registerDisposableBeanIfNecessary
					
					4. instantiateBean当中
					instantiateBean: 无参构造方法
					autowireConstructor自动注入的构造方法
					
          // 获取实例化策略并且进行实例化操作, bean的生成策略，默认是cglib 
					beanInstance = getInstantiationStrategy().instantiate(mbd, beanName, this);
					
					5. instantiate当中
          // 获取默认的无参构造器
          constructorToUse = clazz.getDeclaredConstructor();
          return BeanUtils.instantiateClass(constructorToUse);
          
          6. instantiateClass当中
          return ctor.newInstance(argsWithDefaultValues);

```



### finishBeanFactoryInitialization()

```java
// 初始化剩下的单实例（非懒加载的）
finishBeanFactoryInitialization(beanFactory);

ConversionService 转换服务
整型值 123 -> Integer类型
与之前的 refresh3: prepareBeanFactory() 中 自定义属性编辑器 property editor 类似

三个convert父接口:
1. Converter接口 将S类型转换成T类型
    @FunctionalInterface
    public interface Converter<S, T> {
        @Nullable
        T convert(S source);
    }
2. GenericConverter接口 用于两种或者更多种类型之间转换的通用转换器接口
		接口内部类 final class ConvertiblePair 组建一个源到目的的组合  
		// 根据source和target来做条件判断，从而判断哪个转换器生效哪个转换器不生效
  	public interface ConditionalConverter {
				boolean matches(TypeDescriptor sourceType, TypeDescriptor targetType);
		}
		// 同时继承 GenericConverter 和 ConditionalConverter, 加了一个 matches() 的条件判断
		public interface ConditionalGenericConverter extends GenericConverter, ConditionalConverter
3. ConverterFactory<S, R> 
    // converter转换器的工厂类，用来获取对应的转换器
    public interface ConverterFactory<S, R> {
        // 获取转换器
        <T extends R> Converter<S, T> getConverter(Class<T> targetType);
    }

// 自己添加一个新的转化服务, 源码中并没有add, register 之类的方法
在 ConversionServiceFactoryBean 中的 converters 属性
configuration a conversionService:
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
		<property name = "converters">
    		<set>
    				<bean class="example.MyCustomerConvertor">
    		</set>
    </property>
</bean>

-----------------------------------------------------------------------------

// 如果beanFactory之前没有注册嵌入值解析器，则注册默认的嵌入值解析器，主要用于注解属性值的解析
if (!beanFactory.hasEmbeddedValueResolver()) {
		beanFactory.addEmbeddedValueResolver(strVal -> getEnvironment().resolvePlaceholders(strVal));
}

// spring提供的一个工具类，用于解析bean定义中属性值里面的占位符，此类不能被直接实例化使用
public abstract class PlaceholderConfigurerSupport extends PropertyResourceConfigurer
		implements BeanNameAware, BeanFactoryAware
其父类实现了 BFPP 接口
public abstract class PropertyResourceConfigurer extends PropertiesLoaderSupport
		implements BeanFactoryPostProcessor, PriorityOrdered
其子类
public class PropertySourcesPlaceholderConfigurer extends PlaceholderConfigurerSupport implements EnvironmentAware
另一子类
解析${}, 已经被废弃
@Deprecated
public class PropertyPlaceholderConfigurer extends PlaceholderConfigurerSupport

完成赋值的过程:
PropertySourcesPlaceholderConfigurer # postProcessBeanFactory()
->
processProperties(beanFactory, new PropertySourcesPropertyResolver(this.propertySources));
->
doProcessProperties(beanFactoryToProcess, valueResolver);
跳转到 PlaceholderConfigurerSupport # doProcessProperties()
->
最后一行
// 设置占位符处理器为内置的值处理器
beanFactoryToProcess.addEmbeddedValueResolver(valueResolver);
  
-----------------------------------------------------------------------------

// 尽早初始化loadTimeWeaverAware bean,以便尽早注册它们的转换器
String[] weaverAwareNames = beanFactory.getBeanNamesForType(LoadTimeWeaverAware.class, false, false);
for (String weaverAwareName : weaverAwareNames) {
		getBean(weaverAwareName);
}

在 prepareRefresh() 中完成赋值 ??? 没找到啊

```



### preInstantiateSingletons() // getMergedLocalBeanDefinition()

```java
// 实例化剩下的单例对象 (非懒加载的)
beanFactory.preInstantiateSingletons();

// 合并父类BeanDefinition, 把父类和子类的关于bean的描述信息做一个整合
RootBeanDefinition bd = getMergedLocalBeanDefinition(beanName);

注意: RootBeanDefinition 和 GenericBeanDefinition 是同级
  RootBeanDefinition 包含了父类相关信息的对象定义
  GenericBeanDefinition 通用的对象定义

-----------------------------------------------------------------------------
之前也调用过 getMergedLocalBeanDefinition() 方法
  
注意 AbstractBeanFactory 中属性:
	// 从bean名称映射到合并的RootBeanDefinition
	private final Map<String, RootBeanDefinition> mergedBeanDefinitions = new ConcurrentHashMap<>(256);

invokeBeanFactoryPostProcessors()
String[] postProcessorNames = beanFactory.getBeanNamesForType(BeanDefinitionRegistryPostProcessor.class, true, false);
->
getBeanNamesForType()
// 配置还未被冻结或者类型为null或者不允许早期初始化
if (!isConfigurationFrozen() || type == null || !allowEagerInit) {
		return doGetBeanNamesForType(ResolvableType.forRawClass(type), includeNonSingletons, allowEagerInit);
}
or
// 如果缓存中没有获取到，那么只能重新获取，获取到之后就存入缓存
resolvedBeanNames = doGetBeanNamesForType(ResolvableType.forRawClass(type), includeNonSingletons, true);
->
doGetBeanNamesForType()
// 获取合并的BeanDefinition，合并的BeanDefinition指的是整合了父BeanDefinition的属性，然后属性值会转换为RootBeanDefinition
RootBeanDefinition mbd = getMergedLocalBeanDefinition(beanName);
->
getMergedLocalBeanDefinition()
// 因为在后面 put 了, 所以可以先在 mergedBeanDefinitions 中 get
RootBeanDefinition mbd = this.mergedBeanDefinitions.get(beanName);
->
getMergedBeanDefinition()
// 将beanName和mbd的关系添加到 从bean名称映射到合并的RootBeanDefinition集合中
this.mergedBeanDefinitions.put(beanName, mbd);

总之:
getBeanNamesForType() -> doGetBeanNamesForType() -> getMergedLocalBeanDefinition() -> getMergedBeanDefinition()

getMergedLocalBeanDefinition()说明: 
在实例化之前, 要把所有基础的 beanDefinition 对象转换成 rootBeanDefinition 对象进行缓存, 
后续在需要马上要实例化的时候直接获取定义信息, 
而定义信息中如果包含了父类, 
那么必须要<先创建父类才能有子类>
```

### BeanFactory 和 FactoryBean

```java

都是对象工厂, 用来创建对象
 - 如果使用BeanFactor的接口, 必须严格遵守spring bean的生命周期, 从实例化, 到初始化, 
 			到invokeAwareMethod, invokeInitMethod, before, after, 此流程非常复杂且麻烦
 - 更简单的的方式创建: FactoryBean, 不需要遵循此创建顺序

public interface FactoryBean<T> {
   String OBJECT_TYPE_ATTRIBUTE = "factoryBeanObjectType";
   // 直接返回对象
   @Nullable
   T getObject() throws Exception;
   // 返回类型
   @Nullable
   Class<?> getObjectType();
   // 判断是否单例
   default boolean isSingleton() {
   		return true;
   }
}

如何注入容器?
<bean class = "com.wxz.MyFactoryBean"></bean>

String FACTORY_BEAN_PREFIX = "&";
在spring中获取factoryBean对象: 用取地址符
MyFactoryBean myFactoryBean = applicationContext.getBean("&myFactoryBean"); 能取到
MyFactoryBean myFactoryBean = applicationContext.getBean(MyFactoryBean.class); 能取到
MyFactoryBean myFactoryBean = applicationContext.getBean("&myFactoryBean", MyFactoryBean.class); 取不到

当使用FactoryBean一共创建了两个对象, FactoryBean对象和getObject()创建的对象, 
两个对象都交由spring管理, 但注意其存放对象的缓存集合不同
1. 实现了FactoryBean接口的对象先创建, 放在一级缓存
		注意, 在整个 refresh11: finishBeanFactoryInitialization() 方法执行完后, 一级缓存中没有getObject()创建的对象
2. 但是调用了getObject()方法获取的<单例>对象, 存储在 factoryBeanObjectCache
3. 如果不是单例对象的话, isSingleton()方法返回false, 那么需要每次调用的时候重新创建, 缓存中不会保存当前对象

      
// 根据&+beanName来获取具体的对象
Object bean = getBean(FACTORY_BEAN_PREFIX + beanName);
or
执行
MyFactoryBean myFactoryBean = applicationContext.getBean("&myFactoryBean");
的时候, 会去掉前面的取地址符:
->
doGetBean()
->
// 提取对应的beanName，在此处直接使用即可，为什么还要进行转换呢?
// 原因在于当bean对象实现FactoryBean接口之后就会变成&beanName，同时如果存在别名，也需要把别名进行转换
String beanName = transformedBeanName(name);


获取到user对象的流程 !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
User user = (User) applicationContext.getBean("myFactoryBean"); 
->
AbstractBeanFactory # doGetBean()
// 返回对象的实例，当你实现了FactoryBean接口的对象，需要获取具体的对象的时候就需要此方法来进行获取了
bean = getObjectForBeanInstance(sharedInstance, name, beanName, null);
->
getObjectForBeanInstance()
// 从BeanFactory对象中获取管理的对象. 如果不是synthetic会对其对象进行该工厂的后置处理
object = getObjectFromFactoryBean(factory, beanName, !synthetic);
->
getObjectFromFactoryBean()

注意:
// 由FactoryBeans创建的单例对象的缓存:FactoryBean名称到对象
private final Map<String, Object> factoryBeanObjectCache = new ConcurrentHashMap<>(16);
  
// 获取beanName的Bean对象
Object object = this.factoryBeanObjectCache.get(beanName);

// 获取factory管理的对象实例并赋值给object
object = doGetObjectFromFactoryBean(factory, beanName);

// 仅在上面的getObject()调用期间进行后处理和存储(如果尚未放置)
// (例如,由于自定义getBean调用触发的循环引用处理)
// 重新从factoryBeanObjectCache中获取beanName对应bean对象
Object alreadyThere = this.factoryBeanObjectCache.get(beanName);

// 创建单例之前的回调
beforeSingletonCreation(beanName);
// 对从FactoryBean获得的给定对象进行后处理.
object = postProcessObjectFromFactoryBean(object, beanName);
// 创建单例后的回调
afterSingletonCreation(beanName);

// 将beanName以及object添加到factoryBeanObjectCache中
this.factoryBeanObjectCache.put(beanName, object);
->
doGetObjectFromFactoryBean()
// 显示调用getObject()方法 !!! FactoryBean 接口方法
// 获取factory管理的对象实例赋值给object
object = factory.getObject();


```



### doGetBean()

```java
getBean() -> doGetBean() -> createBean() -> doCreateBean() 
除了在 refresh11: finishBeanFactoryInitialization 中调用了 getBean() 方法, 
在 refresh5: invokeBeanFactoryPostProcessors() 方法中也会调用 getBean() 方法

// 为指定的Bean标记为已经创建（或将要创建）
// 加标识和放入集合的区别: 加标识判断也需要遍历集合
markBeanAsCreated()

// 如果存在依赖的bean的话，那么则优先实例化依赖的bean
// depend-on, 被依赖的bean需要优先创建
String[] dependsOn = mbd.getDependsOn();

源码:
// 创建bean的实例对象
if (mbd.isSingleton()) {
  // 返回以beanName的(原始)单例对象，如果尚未注册，则使用singletonFactory创建并注册一个对象:
  // getSingleton 从一级缓存中获取, 注意之前的重载方法
  sharedInstance = getSingleton(beanName, () -> {
    try {
      // 为给定的合并后BeanDefinition(和参数)创建一个bean实例
      return createBean(beanName, mbd, args);
    }
    catch (BeansException ex) {
      // 显示地从单例缓存中删除实例：它可能是由创建过程急切地放在那里，以允许循环引用解析。还要删除
      // 接收到该Bean临时引用的任何Bean
      // 销毁给定的bean。如果找到相应的一次性Bean实例，则委托给destoryBean
      destroySingleton(beanName);
      // 重新抛出ex
      throw ex;
    }
  });
  // 从beanInstance中获取公开的Bean对象，主要处理beanInstance是FactoryBean对象的情况，如果不是
  // FactoryBean会直接返回beanInstance实例
  bean = getObjectForBeanInstance(sharedInstance, name, beanName, mbd);
}

// 返回以给定名称注册的(原始)单例对象，如果尚未注册，则创建并注册一个对象
// 注意第二个参数传入的是一个 lambda 函数式接口
public Object getSingleton(String beanName, ObjectFactory<?> singletonFactory) {
  
    // 创建单例之前的回调,默认实现将单例注册为当前正在创建中
  	beforeSingletonCreation(beanName);
  
  	// 从单例工厂中获取对象
  	// 注意这里的getObject()方法中, 跳转到匿名内部类的方法, return createBean(beanName, mbd, args);
  	singletonObject = singletonFactory.getObject();
  
  	// 创建单例后的回调,默认实现将单例标记为不在创建中
  	afterSingletonCreation(beanName);
}

@FunctionalInterface
public interface ObjectFactory<T> {
	T getObject() throws BeansException;
}
ObjectFactory是一个函数式接口, 当调用其中的getObject()方法的时候, 才会将实际传递的匿名内部类中的实现逻辑来进行执行
所以, 执行顺序: getSingleton() -> getObject() -> 传入的匿名内部类

// 正在创建过程中的beanName集合
private final Set<String> singletonsCurrentlyInCreation = Collections.newSetFromMap(new ConcurrentHashMap<>(16));  
// beforeSingletonCreation() 和 afterSingletonCreation() 操作 singletonsCurrentlyInCreation 记录正在创建的状态

新生状态 -> 开始创建 -> 创建过程中 -> 创建结束 -> 完整对象 -> context.getBean();
创建过程中:
实例化 -> 填充属性 -> 执行aware接口方法 -> 执行init方法 -> 执行后置处理器方法
在创建过程中, 需要判断是不是正在创建过程中的状态
```



### createBean()

#### 1. Class<?> resolvedClass = resolveBeanClass(mbd, beanName);

```java
// 锁定class，根据设置的class属性或者根据className来解析class
Class<?> resolvedClass = resolveBeanClass(mbd, beanName);
->
// 判断是否有安全管理器
// SecurityManager 权限检查类, 一堆check, 防止程序在运行过程中出现异常情况
// doPrivileged 返回值是 native, ...
if (System.getSecurityManager() != null) {
		return AccessController.doPrivileged((PrivilegedExceptionAction<Class<?>>)
					() -> doResolveBeanClass(mbd, typesToMatch), getAccessControlContext());
}
else {
    // 进行详细的处理解析过程
    return doResolveBeanClass(mbd, typesToMatch);
}

// 获取mbd配置的bean类名，将bean类名解析为Class对象,并将解析后的Class对象缓存在mdb中以备将来使用
doResolveBeanClass()
->
// 定期解析，将结果缓存在BeanDefinition中...
// 使用classLoader加载当前BeanDefinitiond对象所配置的Bean类名的Class对象（每次调用都会重新加载,可通过
// AbstractBeanDefinition#getBeanClass 获取缓存）
return mbd.resolveBeanClass(beanClassLoader);
->
// 获取当前bean对应的Class对象
Class<?> resolvedClass = ClassUtils.forName(className, classLoader);
->
// 传入elementClassName递归本方法获取其Class
Class<?> elementClass = forName(elementClassName, classLoader);
```



#### 2. mbdToUse.prepareMethodOverrides(); // 处理lookup-method & replace-method

```java
关于 lookup-method 和 replace-method:
https://blog.51cto.com/u_15127569/4193894
https://www.cnblogs.com/ViviChan/p/4981619.html
https://www.jianshu.com/p/4b3731465ea4

lookup-method:
单例模式的bean A需要引用另外一个非单例模式的bean B，为了在我们每次引用的时候都能拿到最新的bean B，
		1. 我们可以让bean A通过实现 ApplicationContextAware 来感知 applicationContext（即可以获得容器上下文），
  			从而能在运行时通过 ApplicationContext.getBean(String beanName) 的方法来获取最新的bean B。
				但是如果用 ApplicationContextAware 接口，就让我们与Spring代码耦合了，违背了反转控制原则
  			（IoC，即bean完全由Spring容器管理，我们自己的代码只需要用bean就可以了）。
		2. 通过<lookup-method/>标签注入
eg:
<bean id="apple" class="cn.com.willchen.test.di.Apple" scope="prototype"/>
<bean id="bananer" class="cn.com.willchen.test.di.Bananer " scope="prototype"/>
<bean id="fruitPlate1" class="cn.com.willchen.test.di.FruitPlate">
    <lookup-method name="getFruit" bean="apple"/>
</bean>
<bean id="fruitPlate2" class="cn.com.willchen.test.di.FruitPlate">
    <lookup-method name="getFruit" bean="bananer"/>
</bean>

replace-method:
replace-method是Spring动态借助CGLIB改变bean中的方法，通过改变方法逻辑注入对象，该方法的使用需要依赖Spring提供的MethodReplacer接口实现。
eg:
<bean id="userService" class="com.codegeek.ioc.day2.replacemethod.UserServiceImpl">
    <replaced-method name="findUserNameById" replacer="replaceMethod">
    		<arg-type>java.lang.String</arg-type>
    </replaced-method>
</bean>
<!-- replace-method属性注入 -->
<bean id="replaceMethod" class="com.codegeek.ioc.day2.replacemethod.UserReplaceMethod"/>


继续 createBean()
// 验证及准备覆盖的方法, lookup-method, replace-method
// 当需要创建的bean对象中包含了lookup-method和replace-method标签的时候，会产生覆盖操作
mbdToUse.prepareMethodOverrides();
注意属性 allowBeanDefinitionOverriding. 拓展 customizeFactoryBean() 时可以进行修改

lookup-method示例: methodOverride.xml
    spring中默认的对象都是单例的，spring会在一级缓存中持有该对象，方便下次直接获取，
    那么如果是原型作用域的话，会创建一个新的对象
    如果想在一个单例模式的bean下引用一个原型模式的bean,怎么办？
    在此时就需要引用lookup-method标签来解决此问题
    通过拦截器的方式每次需要的时候都去创建最新的对象，而不会把原型对象缓存起来，
    
mbdToUse.prepareMethodOverrides();
->
创建FruitPlate时候会进入
getMethodOverrides().getOverrides().forEach(this::prepareMethodOverride);  
// 标记methodOverride暂未被覆盖，避免参数类型检查的开销
methodOverride.setOverloaded(false);
->
在实例化时
doCreateBean() -> createBeanInstance() -> instantiateBean() ->
beanInstance = getInstantiationStrategy().instantiate(mbd, beanName, this);
->
先 getInstantiationStrategy() 方法

// bean的生成策略，默认是cglib
private InstantiationStrategy instantiationStrategy = new CglibSubclassingInstantiationStrategy();

public class CglibSubclassingInstantiationStrategy extends SimpleInstantiationStrategy {
    // 如果没有override方法覆盖的话，那么索引位置为0
    private static final int PASSTHROUGH = 0;
    // 如果有lookup-method的覆盖，那么索引位置为1
    private static final int LOOKUP_OVERRIDE = 1;
    // 如果有replace-method的覆盖，那么索引位置为2
    private static final int METHOD_REPLACER = 2;
  	
  	这些下标索引决定了内部类 CglibSubclassCreator 中用哪个拦截器最后进行实例化操作
  	
  	private static class CglibSubclassCreator {
    		// 此处定义了一个回调类型的数组，后面MethodOverrideCallbackFilter通过指定下标获取对应的拦截
				private static final Class<?>[] CALLBACK_TYPES = new Class<?>[]
						{NoOp.class, LookupOverrideMethodInterceptor.class, ReplaceOverrideMethodInterceptor.class};
    }
}
->
后 instantiate(mbd, beanName, this) 方法
instantiate()方法的第一步就是判断有没有overrides
// bd对象定义中，是否包含MethodOverride列表，spring中有两个标签参数会产生MethodOverrides,分别是lookup-method,replaced-method
// 没有MethodOverrides对象，可以直接实例化
if (!bd.hasMethodOverrides()) {
  	// 查看bd对象里使用否含有构造方法
		constructorToUse = (Constructor<?>) bd.resolvedConstructorOrFactoryMethod;
  	// 获取默认的无参构造器
  	constructorToUse = clazz.getDeclaredConstructor();
  	// 获取到构造器之后将构造器赋值给bd中的属性
  	bd.resolvedConstructorOrFactoryMethod = constructorToUse;
  	// 通过反射生成具体的实例化对象
  	return BeanUtils.instantiateClass(constructorToUse);
} else {
    // 必须生成cglib子类
    // SimpleInstantiationStrategy # instantiateWithMethodInjection () 
  	// 此方法没有做任何的实现，直接抛出异常，可以由子类重写覆盖
    return instantiateWithMethodInjection(bd, beanName, owner);
}
->
// 子类重写SimpleInstantiationStrategy 中的 instantiateWithMethodInjection方法
CglibSubclassingInstantiationStrategy # instantiateWithMethodInjection()
->
// 必须生成一个cglib的子类
return new CglibSubclassCreator(bd, owner).instantiate(ctor, args);
->
CglibSubclassingInstantiationStrategy 类中 CglibSubclassCreator 内部类 instantiate() 方法
  
源码: 参考cglib实现动态代理
  	public Object instantiate(@Nullable Constructor<?> ctor, Object... args) {
			// 根据beanDefinition来创建一个cglib的子类
  		// enhance 增强, 使用的是cglib动态代理
			Class<?> subclass = createEnhancedSubclass(this.beanDefinition);
			Object instance;
			// 如果构造器等于空，那么直接通过反射来实例化对象
			if (ctor == null) {
				instance = BeanUtils.instantiateClass(subclass);
			}
			else {
				try {
					// 通过cglib对象来根据参数类型获取对应的构造器
					Constructor<?> enhancedSubclassConstructor = subclass.getConstructor(ctor.getParameterTypes());
					// 通过构造器来获取对象
					instance = enhancedSubclassConstructor.newInstance(args);
				}
				catch (Exception ex) {
					throw new BeanInstantiationException(this.beanDefinition.getBeanClass(),
							"Failed to invoke constructor for CGLIB enhanced subclass [" + subclass.getName() + "]", ex);
				}
			}
			// SPR-10785: set callbacks directly on the instance instead of in the
			// enhanced class (via the Enhancer) in order to avoid memory leaks.
			// https://jira.spring.io/browse/SPR-10785
			Factory factory = (Factory) instance;
			factory.setCallbacks(new Callback[] {NoOp.INSTANCE,
					new LookupOverrideMethodInterceptor(this.beanDefinition, this.owner),
					new ReplaceOverrideMethodInterceptor(this.beanDefinition, this.owner)});
			return instance;
		}
      
      其中与apple相关的属性在
      debug时注意查看methodOverrides属性

-------------------------------------------------------------------------------------------------------------
        
    <bean id="apple" class="com.mashibing.methodOverrides.lookup.Apple">
        <property name="banana" ref="banana"></property>
    </bean>
    <bean id="banana" class="com.mashibing.methodOverrides.lookup.Banana">
    </bean>
    <bean id="fruitplate1" class="com.mashibing.methodOverrides.lookup.FruitPlate">
        <lookup-method name="getFruit" bean="apple"></lookup-method>
    </bean>
    <bean id="fruitplate2" class="com.mashibing.methodOverrides.lookup.FruitPlate">
        <lookup-method name="getFruit" bean="banana"></lookup-method>
    </bean>

// apple 和 banana 单例, 获取时从缓存拿
在FruitPlate fruitplate1 = (FruitPlate) ac.getBean("fruitplate1");时,
fruitplate1, fruitplate2, apple, banana 已经加载到一级缓存 singletonObjects
其中fruitplate1, fruitplate2是动态代理对象

调用fruitplate1.getFruit();时,
会跳到 LookupOverrideMethodInterceptor # intercept() 方法

// apple 和 banana 多例, 加上scope="prototype", 获取时走创建流程
只有 fruitplate1, fruitplate2 被加载进入一级缓存
在 doGetBean() 时, 会走
// 原型模式的bean对象创建
else if (mbd.isPrototype()) {}

-------------------------------------------------------------------------------------------------------------

<bean id="apple" class="com.mashibing.methodOverrides.lookup.Apple">
		<property name="banana" ref="banana"></property>
</bean>
<bean id="banana" class="com.mashibing.methodOverrides.lookup.Banana" scope="prototype">
</bean>

ApplicationContext ac = new ClassPathXmlApplicationContext("methodOverride.xml");
Apple bean = ac.getBean(Apple.class);
System.out.println(bean.getBanana());
System.out.println(bean.getBanana());
两个banana是一样的, 因为apple是单例
```



#### 3. Object bean = resolveBeforeInstantiation(beanName, mbdToUse); // aop阶段生成advisor对象



```java
如果包含aop的相关处理, 那么会在此处生成advisor对象, 方便后续进行调用


// 给BeanPostProcessors一个机会来返回代理来替代真正的实例，应用实例化前的前置处理器,用户自定义动态代理的方式，针对于当前的被代理类需要经过标准的代理流程来创建对象
Object bean = resolveBeforeInstantiation(beanName, mbdToUse);

    // 调用预实例化的postprocessor，处理是否有预实例化的快捷方式对于特殊的bean
    @Nullable
    protected Object resolveBeforeInstantiation(String beanName, RootBeanDefinition mbd) {
        Object bean = null;
        // 如果beforeInstantiationResolved值为null或者true，那么表示尚未被处理，进行后续的处理
        if (!Boolean.FALSE.equals(mbd.beforeInstantiationResolved)) {
            // 确认beanclass确实在此处进行处理
            // 判断当前mbd是否是合成的，只有在实现aop的时候synthetic的值才为true，并且是否实现了InstantiationAwareBeanPostProcessor接口
            if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) {
                // 获取类型
                Class<?> targetType = determineTargetType(beanName, mbd);
                if (targetType != null) {
                    bean = applyBeanPostProcessorsBeforeInstantiation(targetType, beanName);
                    if (bean != null) {
                        bean = applyBeanPostProcessorsAfterInitialization(bean, beanName);
                    }
                }
            }
            // 是否解析了
            mbd.beforeInstantiationResolved = (bean != null);
        }
        return bean;
    }

		hasInstantiationAwareBeanPostProcessors()
    public class MyInstantiationAwareBeanPostProcessor implements InstantiationAwareBeanPostProcessor {
		}
		在registerBPP中完成注册 (xml文件中<bean>标签)
		
    其中 applyBeanPostProcessorsBeforeInstantiation() <上面源码> 
      会调用 MyInstantiationAwareBeanPostProcessor # postProcessBeforeInstantiation() 方法
			由cglib动态代理生成对象
		
		如果是 new 的一个对象, 会交给spring管理, 但是不能拦截方法
		enhancer.setCallback(new MyMethodInterceptor());

		通过自定义创建出来的bean可以直返回, 不用往下走执行doCreateBean
    ?????? 已经忘了什么意思了
    
		hasInstantiationAwareBeanPostProcessors() 方法说明:
      关联 AbstractBeanFactory # hasInstantiationAwareBeanPostProcessors 属性
		abstractBeanFactory 
    	属性:
        // 指示是否已经注册了任何 InstantiationAwareBeanPostProcessors 对象
        private volatile boolean hasInstantiationAwareBeanPostProcessors;
			方法:
        @Override
        public void addBeanPostProcessor(BeanPostProcessor beanPostProcessor) {
          Assert.notNull(beanPostProcessor, "BeanPostProcessor must not be null");
          // 后添加的BeanPostProcessor会覆盖之前的，先删除，然后在添加
          // 从老的位置移除此beanPostProcessor
          this.beanPostProcessors.remove(beanPostProcessor);
          // 此处是为了设置某些状态变量，这些状态变量会影响后续的执行流程，只需要判断是否是指定的类型，然后设置标志位即可
          if (beanPostProcessor instanceof InstantiationAwareBeanPostProcessor) {
            // 该变量表示beanfactory是否已注册过InstantiationAwareBeanPostProcessor
            this.hasInstantiationAwareBeanPostProcessors = true;
          }
          if (beanPostProcessor instanceof DestructionAwareBeanPostProcessor) {
            // 该变量表示beanfactory是否已注册过DestructionAwareBeanPostProcessor
            this.hasDestructionAwareBeanPostProcessors = true;
          }
          // 将beanPostProcessor添加到beanPostProcessors缓存中
          this.beanPostProcessors.add(beanPostProcessor);
        }
		
		InstantiationAwareBeanPostProcessor 接口说明:
		// 继承自BeanPostProcessor，添加了实例化前，实例化后，属性注入后的处理方法
		public interface InstantiationAwareBeanPostProcessor extends BeanPostProcessor {
      // 在bean实例化之前调用
      @Nullable
      default Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) throws BeansException {
        return null;
      }
      // 在bean实例化之后调用
      default boolean postProcessAfterInstantiation(Object bean, String beanName) throws BeansException {
        return true;
      }
      // 当使用注解的时候，通过这个方法来完成属性的注入
      @Nullable
      default PropertyValues postProcessProperties(PropertyValues pvs, Object bean, String beanName)
        throws BeansException {
        return null;
      }
      // 属性注入后执行的方法，在5.1版本被废弃
      @Deprecated
      @Nullable
      default PropertyValues postProcessPropertyValues(
        PropertyValues pvs, PropertyDescriptor[] pds, Object bean, String beanName) throws BeansException {
        return pvs;
      }
    }
```



### doCreateBean() # createBeanInstance()

```java
创建 bean 对象的方式
1. 自定义BPP, 生成代理对象, 如上 InstantiationAwareBeanPostProcessor
2. 通过反射创建对象
3. 通过 factoryMethod 创建对象
4. factoryBean 创建对象. 
		抽象出一个接口规范, 所有的对象必须通过getObject()方法获得
		接口规范实现
5. supplier 创建对象. 与 factoryBean 有相似之处. 
		随便定义创建对象的方法, 不止局限于getObject()
		只是beanDefinition的一个属性值
6. 直接 new 一个对象

------------------------------------------------------------------------------------------------------------------
  通过 supplier 创建对象

// 实际创建bean的调用
Object beanInstance = doCreateBean(beanName, mbdToUse, args);
->
// 根据执行bean使用对应的策略创建新的实例，如，工厂方法，构造函数主动注入、简单初始化
instanceWrapper = createBeanInstance(beanName, mbd, args);
->
// 判断当前beanDefinition中是否包含实例供应器，此处相当于一个回调方法，利用回调方法来创建bean
Supplier<?> instanceSupplier = mbd.getInstanceSupplier();
if (instanceSupplier != null) {
		return obtainFromSupplier(instanceSupplier, beanName);
}
        
@FunctionalInterface
public interface Supplier<T> {
    T get();
}

    <bean id="user" class="com.mashibing.supplier.User"></bean>
    <bean class="com.mashibing.supplier.SupplierBeanFactoryPostProcessor"></bean>
    
      public class SupplierBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
          @Override
          public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
              BeanDefinition user = beanFactory.getBeanDefinition("user");
              // setInstanceSupplier() 在 AbstractBeanDefinition 抽象类中的方法, 
              // beanDefinition 最早是 GenericBeanDefinition, 且 GenericBeanDefinition 继承了 AbstractBeanDefinition
              GenericBeanDefinition genericBeanDefinition = (GenericBeanDefinition) user;
              genericBeanDefinition.setInstanceSupplier(CreateSupplier::createUser);
              genericBeanDefinition.setBeanClass(User.class);
          }
      }

->
obtainFromSupplier()
->
// 调用supplier的方法
instance = instanceSupplier.get();

------------------------------------------------------------------------------------------------------------------
	通过 factoryBean 和 factoryMethod 创建对象
  
createBean() -> doCreateBean() -> 继续 createBeanInstance()
// 如果工厂方法不为空则使用工厂方法初始化策略
if (mbd.getFactoryMethodName() != null) {
		return instantiateUsingFactoryMethod(beanName, mbd, args);
}
->
// 创建构造器处理器并使用factorymethod进行实例化操作
return new ConstructorResolver(this).instantiateUsingFactoryMethod(beanName, mbd, explicitArgs);

	<bean>标签中有属性, factory-bean, factory-method 两种不同的工厂, 实例工厂/静态工厂, 通过static修饰区分

    // class引用对象是静态工厂类
    <bean id="person" class="com.mashibing.factoryMethod.PersonStaticFactory" factory-method="getPerson">
        <!--constructor-arg：可以为方法指定参数-->
        <constructor-arg value="123"></constructor-arg>
    </bean>
    
    // 实例工厂
    <bean id="personInstanceFactory" class="com.mashibing.factoryMethod.PersonInstanceFactory"></bean>
    
    // class引用对象是person对象, 且需要注入实例工厂
    <bean id="person2" class="com.mashibing.factoryMethod.Person" factory-bean="personInstanceFactory" factory-method="getPerson">
        <constructor-arg value="wangwu"></constructor-arg>
    </bean>
->
ConstructorResolver # instantiateUsingFactoryMethod()
->

// 获取工厂类的所有候选工厂方法
factoryClass = ClassUtils.getUserClass(factoryClass);

// 如果mbd所配置工厂方法时唯一
if (mbd.isFactoryMethodUnique) {}

// 根据mbd的是否允许访问非公共构造函数和方法标记【RootBeanDefinition.isNonPublicAccessAllowed】来获取factoryClass的所有候选方法
Method[] rawCandidates = getCandidateMethods(factoryClass, mbd);

// 根据名称和数据类型创建参数持有者
argsHolder = createArgumentArray(beanName, mbd, resolvedValues, bw, paramTypes, paramNames,
		getUserDeclaredConstructor(candidate), autowiring, candidates.length == 1);

// 使用factoryBean生成与beanName对应的Bean对象,并将该Bean对象保存到bw中
// 里面有个 instantiate() 方法
bw.setBeanInstance(instantiate(beanName, mbd, factoryBean, factoryMethodToUse, argsToUse));
->
ConstructorResolver # instantiate()
// 在beanFactory中返回beanName的Bean实例，并通过factoryMethod创建它
return strategy.instantiate(mbd, beanName, this.beanFactory, constructorToUse, argsToUse);
->
SimpleInstantiationStrategy # instantiate()

public Object instantiate(RootBeanDefinition bd, @Nullable String beanName, BeanFactory owner,
		@Nullable Object factoryBean, final Method factoryMethod, Object... args) {
        // 使用factoryMethod实例化对象
        // 这里实际调用getPerson()方法 !!!!!
        Object result = factoryMethod.invoke(factoryBean, args);
}


------------------------------------------------------------------------------------------------------------------


继续 createBeanInstance(), supplier 和 factorymethod 方式都不满足

createBeanInstance() 源码: --------------------------------------------
// 一个类可能有多个构造器，所以Spring得根据参数个数、类型确定需要调用的构造器
// 在使用构造器创建实例后，Spring会将解析过后确定下来的构造器或工厂方法保存在缓存中，避免再次创建相同bean时再次解析

// 标记下，防止重复创建同一个bean
boolean resolved = false;
// 是否需要自动装配
boolean autowireNecessary = false;
// 如果没有参数
if (args == null) {
  synchronized (mbd.constructorArgumentLock) {
    // 因为一个类可能由多个构造函数，所以需要根据配置文件中配置的参数或传入的参数来确定最终调用的构造函数。
    // 因为判断过程会比较，所以spring会将解析、确定好的构造函数缓存到BeanDefinition中的resolvedConstructorOrFactoryMethod字段中。
    // 在下次创建相同时直接从RootBeanDefinition中的属性resolvedConstructorOrFactoryMethod缓存的值获取，避免再次解析
    if (mbd.resolvedConstructorOrFactoryMethod != null) {
      resolved = true;
      autowireNecessary = mbd.constructorArgumentsResolved;
    }
  }
}
        
// 有构造参数的或者工厂方法
if (resolved) {
  // 构造器有参数
  if (autowireNecessary) {
    // 构造函数自动注入
    return autowireConstructor(beanName, mbd, null, null);
  } else {
    // 使用默认构造函数构造
    return instantiateBean(beanName, mbd);
  }
}
        
// 从bean后置处理器中为自动装配寻找构造方法, 有且仅有一个有参构造或者有且仅有@Autowired注解构造
Constructor<?>[] ctors = determineConstructorsFromBeanPostProcessors(beanClass, beanName);
        
// 注意自动装配的几种方式
// 以下情况符合其一即可进入
// 1、存在可选构造方法
// 2、自动装配模型为构造函数自动装配
// 3、给BeanDefinition中设置了构造参数值
// 4、有参与构造函数参数列表的参数
if (ctors != null || mbd.getResolvedAutowireMode() == AUTOWIRE_CONSTRUCTOR ||
    mbd.hasConstructorArgumentValues() || !ObjectUtils.isEmpty(args)) {
  return autowireConstructor(beanName, mbd, ctors, args);
}
        
// 找出最合适的默认构造方法
ctors = mbd.getPreferredConstructors();
if (ctors != null) {
  // 构造函数自动注入
  return autowireConstructor(beanName, mbd, ctors, null);
}

// 使用默认无参构造函数创建对象，如果没有无参构造且存在多个有参构造且没有@AutoWired注解构造，会报错
return instantiateBean(beanName, mbd);

createBeanInstance() 源码: ------------------------------------

使用默认构造函数构造 instantiateBean():
    
// 第一次不走上面的缓存
// 使用默认无参构造函数创建对象，如果没有无参构造且存在多个有参构造且没有@AutoWired注解构造，会报错
return instantiateBean(beanName, mbd);
->
// 获取实例化策略并且进行实例化操作
beanInstance = getInstantiationStrategy().instantiate(mbd, beanName, this);
->
SimpleInstantiationStrategy # instantiate()
// 如果发现是有方法重载的，就需要用cglib来动态代理，如果没有就直接获取默认构造方法实例化
public Object instantiate(RootBeanDefinition bd, @Nullable String beanName, BeanFactory owner) {}
->
// 如果走缓存的话, 在 SimpleInstantiationStrategy # instantiate() 可以直接获取构造器
// 查看bd对象里使用否含有构造方法
constructorToUse = (Constructor<?>) bd.resolvedConstructorOrFactoryMethod;
// 获取默认的无参构造器
constructorToUse = clazz.getDeclaredConstructor();
// 通过反射生成具体的实例化对象
return BeanUtils.instantiateClass(constructorToUse);

------------------------------------------------------------------------------------------------------------------

使用构造器注入 autowireConstructor():
        <bean id="person" class="com.mashibing.Person" >
            <constructor-arg name="id" value="1"></constructor-arg>
            <constructor-arg name="name" value="lisi"></constructor-arg>
        </bean>
    
        <bean>标签中autowire属性 有五种, byName, byType, constructor, default, no
        public interface AutowireCapableBeanFactory extends BeanFactory {
            int AUTOWIRE_NO = 0; // 没有自动装配
            int AUTOWIRE_BY_NAME = 1; // 按照名字自动装配
            int AUTOWIRE_BY_TYPE = 2; // 按照类型自动装配
            int AUTOWIRE_CONSTRUCTOR = 3; // 按照构造器自动装配
            int AUTOWIRE_AUTODETECT = 4; // 自动装配
        }
        
// 从bean后置处理器中为自动装配寻找构造方法, 有且仅有一个有参构造或者有且仅有@Autowired注解构造
Constructor<?>[] ctors = determineConstructorsFromBeanPostProcessors(beanClass, beanName);
// 以下情况符合其一即可进入
// 1、存在可选构造方法
// 2、自动装配模型为构造函数自动装配
// 3、给BeanDefinition中设置了构造参数值
// 4、有参与构造函数参数列表的参数
if (ctors != null || mbd.getResolvedAutowireMode() == AUTOWIRE_CONSTRUCTOR ||
    mbd.hasConstructorArgumentValues() || !ObjectUtils.isEmpty(args)) {
    return autowireConstructor(beanName, mbd, ctors, args);
}
->
return new ConstructorResolver(this).autowireConstructor(beanName, mbd, ctors, explicitArgs);
注意: 此处是有参构造器构造
->
// 使用public的构造器或者所有构造器
candidates = (mbd.isNonPublicAccessAllowed() ? beanClass.getDeclaredConstructors() : beanClass.getConstructors());

// 对候选的构造函数进行排序，先是访问权限后是参数个数
// public权限参数数量由多到少
AutowireUtils.sortConstructors(candidates);

/**
  * 没有传入参与构造函数参数列表的参数时，对 构造函数 缓存到BeanDefinition中
  * 	1、缓存BeanDefinition进行实例化时使用的构造函数
  * 	2、缓存BeanDefinition代表的Bean的构造函数已解析完标识
  * 	3、缓存参与构造函数参数列表值的参数列表
  */
if (explicitArgs == null && argsHolderToUse != null) {
    // 将解析的构造函数加入缓存
    argsHolderToUse.storeCache(mbd, constructorToUse);
}

------------------------------------------------------------------------------------------------------------------


       
/*
    获取构造器集合
    如果有多个Autowired，required(autowire注解的属性值)为true，不管有没有默认构造方法，会报异常
    如果只有一个Autowired，required为false，没有默认构造方法，会报警告
    如果没有Autowired注解，定义了两个及以上有参数的构造方法，没有无参构造方法，就会报错 ???
        且不是使用xml配置文件进行加载, 使用@Component进行加载, 就会报错: 没有无参构造方法
    其他情况都可以，但是以有Autowired的构造方法优先，然后才是默认构造方法
*/
// 筛选对应的构造方法
// 从bean后置处理器中为自动装配寻找构造方法, 有且仅有一个有参构造或者有且仅有@Autowired注解构造
Constructor<?>[] ctors = determineConstructorsFromBeanPostProcessors(beanClass, beanName);
->
Constructor<?>[] ctors = ibp.determineCandidateConstructors(beanClass, beanName);
->
AutowiredAnnotationBeanPostProcessor # determineCandidateConstructors()
         
public class AutowiredAnnotationBeanPostProcessor extends InstantiationAwareBeanPostProcessorAdapter
	implements MergedBeanDefinitionPostProcessor, PriorityOrdered, BeanFactoryAware {}
        
		
		查看BPP类图, 依次继承
        public interface BeanPostProcessor
        public interface InstantiationAwareBeanPostProcessor extends BeanPostProcessor
        public interface SmartInstantiationAwareBeanPostProcessor extends InstantiationAwareBeanPostProcessor {
            // 预测bean的类型，主要是在bean还没有创建前我们需要获取bean的类型
            @Nullable
            default Class<?> predictBeanType(Class<?> beanClass, String beanName) throws BeansException {
                    return null;
            }
            // 完成对构造函数的解析和推断
            @Nullable
            default Constructor<?>[] determineCandidateConstructors(Class<?> beanClass, String beanName)
                            throws BeansException {
                    return null;
            }
            // 解决循环依赖问题，通过此方法提前暴露一个合格的对象
            default Object getEarlyBeanReference(Object bean, String beanName) throws BeansException {
                    return bean;
            }
        }
------------------------------------------------------------------------------------------------------------------
        
    	createBeanInstance()
    	->
        // 找出最合适的默认构造方法
        ctors = mbd.getPreferredConstructors();
        继承子类 GenericApplicationContext # getPreferredConstructors()
        
```

```

```

### doCreateBean() # createBeanInstance() # instantiateUsingFactoryMethod 流程图



 ![instantiateUsingFactoryMethod大致流程](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0d44suj21bj0u0dix.jpg)





### doCreateBean() # createBeanInstance() # instantiateBean()

```java

// 使用默认构造函数构造
// 使用默认无参构造函数创建对象，如果没有无参构造且存在多个有参构造且没有@AutoWired注解构造，会报错
AbstractAutowireCapableBeanFactory # instantiateBean()
->
// 获取实例化策略并且进行实例化操作
// bean的生成策略，默认是cglib
beanInstance = getInstantiationStrategy().instantiate(mbd, beanName, this);

接口 InstantiationStrategy, 三个重载方法
    // 实例化策略接口，子类被用来根据rootBeanDefinition来创建实例对象
    public interface InstantiationStrategy {
        // 使用默认构造方法进行实例化
        Object instantiate(RootBeanDefinition bd, @Nullable String beanName, BeanFactory owner) 
                           throws BeansException;
        // 通过指定构造器来进行实例化
        Object instantiate(RootBeanDefinition bd, @Nullable String beanName, BeanFactory owner,
                           Constructor<?> ctor, Object... args) throws BeansException;
        // 通过指定工厂方法来进行实例化
        Object instantiate(RootBeanDefinition bd, @Nullable String beanName, BeanFactory owner, 
                           @Nullable Object factoryBean, Method factoryMethod, Object... args) throws BeansException;
		}
	
    // 简单的对象实例化策略
    public class SimpleInstantiationStrategy implements InstantiationStrategy {
        实现了 InstantiationStrategy 接口的 三个 instantiate() 方法
        // 此方法没有做任何的实现，直接抛出异常，可以由子类重写覆盖
        protected Object instantiateWithMethodInjection(RootBeanDefinition bd, @Nullable String beanName, BeanFactory owner) {
            throw new UnsupportedOperationException("Method Injection not supported in SimpleInstantiationStrategy");
        }
        // 此方法没有做任何的实现，直接抛出异常，可以由子类重写覆盖
        protected Object instantiateWithMethodInjection(RootBeanDefinition bd, @Nullable String beanName,
                                                        BeanFactory owner, @Nullable Constructor<?> ctor, Object... args) {
            throw new UnsupportedOperationException("Method Injection not supported in SimpleInstantiationStrategy");
        }
    }
	
    // beanFactory中的对象默认实例化策略
    public class CglibSubclassingInstantiationStrategy extends SimpleInstantiationStrategy {
        // 子类重写SimpleInstantiationStrategy中的instantiateWithMethodInjection方法
        @Override
        protected Object instantiateWithMethodInjection(RootBeanDefinition bd, @Nullable String beanName, BeanFactory owner) {
          	return instantiateWithMethodInjection(bd, beanName, owner, null);
        }
        @Override
        protected Object instantiateWithMethodInjection(RootBeanDefinition bd, @Nullable String beanName, BeanFactory owner,
                                                        @Nullable Constructor<?> ctor, Object... args) {
            // 必须生成一个cglib的子类
            return new CglibSubclassCreator(bd, owner).instantiate(ctor, args);
        }
      }

SimpleInstantiationStrategy 的子类 CglibSubclassingInstantiationStrategy 实现其两个 instantiateWithMethodInjection()方法, 
动态代理的子类创建对象, 包括有参无参.
所以一共有<五种>实例化方法:
simple实例化策略:
	1.无参构造实例化
  2.有参构造实例化
  3.工厂方法实例化
cglib实例化策略:
动态代理对象
  4.无参构造
  5.有参构造
  ->
  CglibSubclassCreator # instantiate()
  
  
继续 instantiateBean()
// spring中的核心接口，是spring中的一个包装类，具有单独或者批量获取和设置属性值，获取属性描述符一级查询属性可读可写的能力，还可以完成类型的转换
public interface BeanWrapper extends ConfigurablePropertyAccessor {}
public interface ConfigurablePropertyAccessor extends PropertyAccessor, PropertyEditorRegistry, TypeConverter 
1. 自定义属性编辑器 PropertyEditorRegistry 接口
		selfEditor 包下 setAsText
2. TypeConverter 接口
		selfConverter 包下 convert, 一对一 / 一对多 / 多对多

instantiateBean() 源码:
// 包装成BeanWrapper
BeanWrapper bw = new BeanWrapperImpl(beanInstance);
initBeanWrapper(bw);
->
// 使用该工厂的ConversionService来作为bw的ConversionService，用于转换属性值，以替换JavaBeans PropertyEditor
bw.setConversionService(getConversionService());
// 将工厂中所有PropertyEditor注册到bw中
registerCustomEditors(bw);
```





### doCreateBean() # applyMergedBeanDefinitionPostProcessors() // 处理@PostConstruct, @PreDestroy, @Resource



```java
// 允许beanPostProcessor 去修改合并的beanDefinition
applyMergedBeanDefinitionPostProcessors()
完成注解的扫描和实现工作, 后面就进行初始化了(属性值的注入etc..) 

// 处理 @PostConstruct
// 允许beanPostProcessor去修改合并的beanDefinition
synchronized (mbd.postProcessingLock) {
  if (!mbd.postProcessed) {
    try {
      // MergedBeanDefinitionPostProcessor后置处理器修改合并bean的定义
      // 对应前面的 beanFactory.freezeConfiguration(), 构造完之后修改BeanDefinition
      // 应用MergedBeanDefinitionPostProcessors类型的beanPostProcessor到指定的beanDefinition中，
      // 执行postProcessMergedBeanDefinition方法
      applyMergedBeanDefinitionPostProcessors(mbd, beanType, beanName);
    } catch (Throwable ex) {
      throw new BeanCreationException(mbd.getResourceDescription(), beanName,
                                      "Post-processing of merged bean definition failed", ex);
    }
    mbd.postProcessed = true;
  }
}
				        
查看BPP类图 MergedBeanDefinitionPostProcessor 接口
public interface MergedBeanDefinitionPostProcessor extends BeanPostProcessor {
  // spring通过此方法找出所有需要注入的字段，同时做缓存
  // 其实现子类大多都带有annotationPostProcessor, 对注解进行解析处理, 其接口方法:
  void postProcessMergedBeanDefinition(RootBeanDefinition beanDefinition, Class<?> beanType, 
  																		 String beanName);
  // 用于在BeanDefinition被修改后，清除容器的缓存
  default void resetBeanDefinition(String beanName) 
}

注意这个接口中 postProcessMergedBeanDefinition() 的实现子类方法 !!!!!!!!

其中, AutowiredAnnotationBeanPostProcessor 处理 @Autowired, @Value
CommonAnnotationBeanPostProcessor 处理 @PostConstruct @PreDestroy @Resource
  
另一个:
查看实现子类
CommonAnnotationBeanPostProcessor # postProcessMergedBeanDefinition() {
  // 处理@PostConstruct和@PreDestroy注解
  super.postProcessMergedBeanDefinition(beanDefinition, beanType, beanName);
  //找出beanType所有被@Resource标记的字段和方法封装到InjectionMetadata中
  InjectionMetadata metadata = findResourceMetadata(beanName, beanType, null);
  metadata.checkConfigMembers(beanDefinition);
}
				
第一行跳转到其父类, 处理@PostConstruct @PreDestroy
InitDestroyAnnotationBeanPostProcessor # postProcessMergedBeanDefinition()
@Override
public void postProcessMergedBeanDefinition(RootBeanDefinition beanDefinition, 
                                            Class<?> beanType, String beanName) {
  // 调用方法获取生命周期元数据并保存
  LifecycleMetadata metadata = findLifecycleMetadata(beanType);
  // 验证相关方法
  metadata.checkConfigMembers(beanDefinition);
}
        

-------------------------------------------------------------------------------------------------------------
        
// 调用方法获取生命周期元数据并保存
LifecycleMetadata metadata = findLifecycleMetadata(beanType);
InitDestroyAnnotationBeanPostProcessor # postProcessMergedBeanDefinition() # findLifecycleMetadata()
->
buildLifecycleMetadata()
          
// 负责解析@Resource、@WebServiceRef、@EJB三个注解，这三个注解都是定义再javax.*包下的注解，属于java中的注解
@Resource, JDK提供, 和 @Autowired类似. 建议使用@Autowired
注意: @PostConstructor 和 @PreDestroy 也是归属于jdk的
public class CommonAnnotationBeanPostProcessor extends InitDestroyAnnotationBeanPostProcessor
  								implements InstantiationAwareBeanPostProcessor, BeanFactoryAware, Serializable {
  
  	private static final Set<Class<? extends Annotation>> resourceAnnotationTypes = new LinkedHashSet<>(4);
		
    static {
        webServiceRefClass = loadAnnotationType("javax.xml.ws.WebServiceRef");
        ejbClass = loadAnnotationType("javax.ejb.EJB");

        // 添加@Resource注解
        resourceAnnotationTypes.add(Resource.class);
        if (webServiceRefClass != null) {
        	  // 添加@WebServiceRef注解
      	    resourceAnnotationTypes.add(webServiceRefClass);
        }
        if (ejbClass != null) {
       	   // 添加@EJB注解
       	   resourceAnnotationTypes.add(ejbClass);
        }
    }
  		
    // CommonAnnotationBeanPostProcessor在初始化时向里面放置了@PostConstruct注解
    @Nullable
    private Class<? extends Annotation> initAnnotationType;

    // CommonAnnotationBeanPostProcessor在初始化时向里面放置了@PreDestory注解
    @Nullable
    private Class<? extends Annotation> destroyAnnotationType;

  	// 构造方法，设置@PostConstruct和@PreDestory注解
    public CommonAnnotationBeanPostProcessor() {
        setOrder(Ordered.LOWEST_PRECEDENCE - 3);
        setInitAnnotationType(PostConstruct.class);
        setDestroyAnnotationType(PreDestroy.class);
        ignoreResourceType("javax.xml.ws.WebServiceContext");
    }

    public void setInitAnnotationType(Class<? extends Annotation> initAnnotationType) {
        this.initAnnotationType = initAnnotationType;
    }

    public void setDestroyAnnotationType(Class<? extends Annotation> destroyAnnotationType) {
        this.destroyAnnotationType = destroyAnnotationType;
    }
  
    @Override
    public void postProcessMergedBeanDefinition(RootBeanDefinition beanDefinition, Class<?> beanType, String beanName) {
        // 处理@PostConstruct和@PreDestroy注解
        super.postProcessMergedBeanDefinition(beanDefinition, beanType, beanName);
        //找出beanType所有被@Resource标记的字段和方法封装到InjectionMetadata中
        InjectionMetadata metadata = findResourceMetadata(beanName, beanType, null);
        metadata.checkConfigMembers(beanDefinition);
    }
}

       
@PostConstruct 和 @PreDestroy 的调用执行:
AbstractAutowireCapableBeanFactory # initializeBean() {
  
	invokeAwareMethods(beanName, bean);
  
  wrappedBean = applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);
  
  invokeInitMethods(beanName, wrappedBean, mbd);
  
  wrappedBean = applyBeanPostProcessorsAfterInitialization(wrappedBean, beanName);
}
->
// 将BeanPostProcessors应用到给定的现有Bean实例，调用它们的postProcessBeforeInitialization初始化方法。
// 返回的Bean实例可能是原始Bean包装器
wrappedBean = applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);
->
// postProcessBeforeInitialization：在任何Bean初始化回调之前(如初始化Bean的afterPropertiesSet或自定义的init方法)
// 将此BeanPostProcessor 应用到给定的新Bean实例。Bean已经填充了属性值。返回的Bean实例可能时原始Bean的包装器。
// 默认实现按原样返回给定的 Bean
Object current = processor.postProcessBeforeInitialization(result, beanName);
->
InitDestroyAnnotationBeanPostProcessor # postProcessBeforeInitialization()
->
// init destroy 方法的调用执行
// 最后在 InitDestroyAnnotationBeanPostProcessor 的内部类 LifecycleMetadata # invokeInitMethods() 调用执行
metadata.invokeInitMethods(bean, beanName);

-------------------------------------------------------------------------------------------------------------
        
// 验证相关方法. 这一步是将修改的BPP设置回去
metadata.checkConfigMembers(beanDefinition);
InitDestroyAnnotationBeanPostProcessor # postProcessMergedBeanDefinition() # checkConfigMembers()
->
// 注册初始化调用方法
beanDefinition.registerExternallyManagedInitMethod(methodIdentifier);
// 注册销毁调用方法
beanDefinition.registerExternallyManagedDestroyMethod(methodIdentifier);
        
-------------------------------------------------------------------------------------------------------------

加载注解的相关解析配置工作
1. CommonAnnotationBeanPostProcessor, 在静态块 static 中添加 @Resource 注解
		其父类 InitDestroyAnnotationBeanPostProcessor @postconstructor, @predestroy
2. AutowiredAnnotationBeanPostProcessor, 在构造方法中注入@Autowire注解, @Value注解. 处理 @Autowirde, @Value

在 applyMergedBeanDefinitionPostProcessors, 遍历到了执行 postProcessMergedBeanDefinition() 方法
先find, 再build

注意 AutowiredAnnotationBeanPostProcessor 内部类
AutowiredFieldElement 类 自动装配属性元素
AutowiredMethodElement 类 自动装配方法元素
其 inject() 方法完成对应属性和方法的注入
  
    // 获取依赖的value值的工作  最终还是委托给beanFactory.resolveDependency()去完成的
    // 这个接口方法由AutowireCapableBeanFactory提供，它提供了从bean工厂里获取依赖值的能力
    value = beanFactory.resolveDependency(desc, beanName, autowiredBeanNames, typeConverter);
    ->
    DefaultListableBeanFactory # doResolveDependency()
      
    // 通过反射，给属性赋值
    ReflectionUtils.makeAccessible(field);
    field.set(bean, value);
    
```









### 从这里开始初始化

### doCreateBean() # 循环依赖

```java
// 往三级缓存中放入lambda表达式, 方便后续调用

doCreateBean() 源码:
// 判断当前bean是否需要提前曝光：单例 & 允许循环依赖 & 当前bean正在创建中，检测循环依赖
boolean earlySingletonExposure = (mbd.isSingleton() && this.allowCircularReferences &&
                                  isSingletonCurrentlyInCreation(beanName));
if (earlySingletonExposure) {
  if (logger.isTraceEnabled()) {
    logger.trace("Eagerly caching bean '" + beanName +
                 "' to allow for resolving potential circular references");
  }
  // 为避免后期循环依赖，可以在bean初始化完成前将创建实例的ObjectFactory加入工厂
  addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));

  // 只保留二级缓存，不向三级缓存中存放对象
  // earlySingletonObjects.put(beanName,bean);
  // registeredSingletons.add(beanName);
}
    
初始化和循环依赖:
  当实例化完成之后, 要开始进行初始化赋值操作了
  但是赋值的时候, 值的类型有可能是引用类型, 需要从 spring 容器中获取具体的某个对象来完成赋值操作
	如果该对象没有创建, 在整个过程中就会设计到对象的创建过程, 而内部对象创建过程中又会有其他的依赖
	其他的依赖有可能包含当前的对象, 而此时当前的对象还没有创建完成, 所以产生了循环依赖问题
        
一级缓存 成品缓存map: beanName 2 Object 普通不需要代理的对象 + 代理对象
二级缓存 半成品缓存map: beanName 2 Object 完成实例化但未完成初始化的对象
三级缓存 生成代理对象的缓存map: beanName 2 lambda表达式用来生成具体的代理对象(一个回调机制)

问题: 
初始化环节的填充属性阶段有没有可能改变当前对象的地址空间? 不可能

spring 在使用的时候会配置aop, 需要生成具体的代理对象
1. 生成代理对象之前要不要生成普通对象? 要
2. 什么时候调用具体的对象? 无论是普通对象还是代理对象? 不知道什么时间调用
3. 有没有可能, 在spring中同时存在同一个beanName的普通对象和代理对象?
		如果有的话, 调用的时候是根据beanName获取具体的对象, 此时两个对象应该用哪个?
		
		那么可以创建两个缓存, 分别存储普通对象和代理对象
		这两个缓存, 优先获取哪个对象?
		优先获取代理对象, 然后才是普通对象 --> 错误结论
		代理对象中应该包含普通对象的所有功能
		如果一个对象被代理了, 那么此时普通对象就可以不存在, 只需要代理对象即可
		也就是说, 在后续创建代理对象之后, 就可以使用代理对象来覆盖普通对象, 普通对象可以不存在
		
		回到第二个问题, 什么时候调用对象
		在调用对象的时候, 如果检测到当前对象需要被代理, 直接创建代理对象覆盖即可
		
		我怎么能够在随时需要的时候创建代理对象呢? -> 类似于观察者, 但是不是
		传递进去一个生成代理对象的匿名内部类, 当需要调用的时候, 直接调用匿名内部类(lambda)来生成代理对象
		就是类似于回调机制
		
4. 生成aop代理对象的时候, 赋值完成了吗? 没有

4. 换一个思路: 生成代理对象的步骤一定在赋值操作完成之后

5. 初始化包含
		a.填充属性
		b.执行aware接口所对应的方法
		c.执行beanPostProcessor中的before方法
		d.执行init-method
		e.执行beanPostProcessor中的after方法
		
		上述步骤执行完成后, 是为了获得一个成品对象, 但是在初始化前能确定哪一个对象需要生成代理对象吗
		而且三级缓存只是一个回调机制, 能否把所有的bean所需要创建代理对象的lambda表达式都放到三级缓存中?
		把所有的bean所需要创建代理对象的lambda表达式都放到三级缓存中, 后续如果我需要调用, 直接从
		三级缓存中调用执行即可, 如果不需要, 在生成完整对象之后可以把三级缓存中lambda()表达式给清除掉
		(往一级缓存中放成品对象的时候就可以清楚)
		
		在什么时候要生成具体的代理对象呢?(代码实操)
		a. 在进行属性注入的时候, 调用该对象生成的时候检测是否需要被代理, 如果需要, 直接创建代理对象
		b. 在整个过程中, 没有其他的对象有当前对象的依赖, 那么在生成最终的完整对象之前生成代理对象即可
		(BeanPostProcessor中的after方法)
		
		什么是提前暴露对象? 二级缓存, 实例化但未被初始化的对象
		三级缓存, 在进行查找的时候, 顺序是什么样的?  1 -> 2 -> 3
		有没有可能直接从三级缓存到一级缓存? 有
		如果单纯为了解决循环依赖问题, 使用二级缓存足够解决问题, 
		三级缓存存在的意义是为了避免代理. 如果没有代理对象, 二级缓存足以解决问题
		
		如何解决循环依赖问题? 
		实例化和初始化是分开处理的, 当完成实例化之后就可以让其他对象引用当前对象, 只不过当前对象不是一个完整对象而已. 后续需要完成此对象的剩余步骤
		
		直接获取半成品对象的引用地址, 保证对象能够被找到. 而半成品对象在堆空间是否有设置的属性值, 无所谓
		
		
		B实例化后赋值给A对象的b属性, 那B生成代理对象后, A.b 如何处理?
		B实例化后赋值给A对象的b属性, 在给A对象的b属性赋值的时候, 已经确定好了 B对象是一个普通对象还是一个代理对象
		
		生成代理的对象的时候调用了 
		getEarlyBeanReference
		
		
		代理不是在 createBean # resolveBeforeInstantiation 的时候就创建了吗?
    // 给BeanPostProcessors一个机会来返回代理来替代真正的实例，应用实例化前的前置处理器,用户自定义动态代理的方式，针对于当前的被代理类需要经过标准的代理流程来创建对象
    Object bean = resolveBeforeInstantiation(beanName, mbdToUse);
    必须要实现一个自定义的BPP resolveBeforeInstantiation 包下 MyInstantiationAwareBeanPostProcessor
    与后面的循环依赖没关系

        
```



### doCreateBean() # populateBean()

```java

在填充属性的过程中, 会涉及到循环依赖的问题
  
propertyValue 对象, pv 对象
debug 时查看 beanFactory -> beanDefinitionMap -> dataSource -> value -> propertyValues
  包含 propertyValueList

属性:
1. 基本类型. 直接完成赋值操作 (4类8种) byte, short, int, long, float, double, boolean, char
2. 引用类型. 从容器中获取具体的对象值, 有就赋值, 没有就创建
注意: Spring 生命周期 所有的对象都必须是单例的
3. 数组, list, set, map, properties 等集合中既包含基本类型, 又包含引用类型
4. 涉及到类型转换问题

<引用类型> 注入的方式:
1. 不注入
2. 按照类型完成注入
3. 按照名称完成注入
4. 按照构造器完成注入
<bean> 标签 autowire 注解, byName, byType, constructor, default, no

// 对bean的属性进行填充，将各个属性值注入，其中，可能存在依赖于其他bean的属性，则会递归初始化依赖的bean
populateBean(beanName, mbd, instanceWrapper);

        // 如果beanWrapper为空
        if (bw == null) {
            // 如果mbd有需要设置的属性
          	// !!! 注意, 定义bean的时候必须要有property标签来完成解析工作
            if (mbd.hasPropertyValues()) {
                // 抛出bean创建异常
                throw new BeanCreationException(
                        mbd.getResourceDescription(), beanName, "Cannot apply property values to null instance");
            } else {
                // 没有可填充的属性，直接跳过
                return;
            }
        }
  
// InstantiationAwareBeanPostProcessor
if (hasInstantiationAwareBeanPostProcessors) {
  	// postProcessAfterInstantiation：一般用于设置属性, 设置bean
    if (!ibp.postProcessAfterInstantiation(bw.getWrappedInstance(), beanName)) {
        return;
    }
}

postProcessAfterInstantiation() 实现子类:
1.
// ProxyProcessorSupport的重要子类。SpringAOP中的核心类。
// 实现了SmartInstantiationAwareBeanPostProcessor、BeanFactoryAware接口。
// 自动创建代理对象的类。我们在使用AOP的时候基本上都是用的这个类来进程Bean的拦截，创建代理对象。
public abstract class AbstractAutoProxyCreator extends ProxyProcessorSupport
		implements SmartInstantiationAwareBeanPostProcessor, BeanFactoryAware 

2.
// 负责解析@Resource、@WebServiceRef、@EJB三个注解，这三个注解都是定义再javax.*包下的注解，属于java中的注解
CommonAnnotationBeanPostProcessor
  
可以通过在这种方式, 在<<<实例化之后>>>进行属性值的设置工作

--------------------------------------------------------------------------------------------------------------

  			源码:
        // 包装一个对象所有的PropertyValue,封装了一些基本操作
        public interface PropertyValues extends Iterable<PropertyValue> {
        }

        // PropertyValues：包含以一个或多个PropertyValue对象的容器，通常包括针对特定目标Bean的一次更新
        // 如果mdb有PropertyValues就获取其PropertyValues
        // 如果Bean标签中有properties标签, 则会解析
        PropertyValues pvs = (mbd.hasPropertyValues() ? mbd.getPropertyValues() : null);

        //MutablePropertyValues：PropertyValues接口的默认实现。允许对属性进行简单操作，并提供构造函数来支持从映射 进行深度复制和构造
        MutablePropertyValues newPvs = new MutablePropertyValues(pvs);
        // 根据autotowire的名称(如适用)添加属性值
        if (resolvedAutowireMode == AUTOWIRE_BY_NAME) {
          //通过bw的PropertyDescriptor属性名，查找出对应的Bean对象，将其添加到newPvs中
          autowireByName(beanName, mbd, bw, newPvs);
        }
        // 根据自动装配的类型(如果适用)添加属性值
        if (resolvedAutowireMode == AUTOWIRE_BY_TYPE) {
          //通过bw的PropertyDescriptor属性类型，查找出对应的Bean对象，将其添加到newPvs中
          autowireByType(beanName, mbd, bw, newPvs);
        }
        //让pvs重新引用newPvs,newPvs此时已经包含了pvs的属性值以及通过AUTOWIRE_BY_NAME，AUTOWIRE_BY_TYPE自动装配所得到的属性值
        pvs = newPvs;


怎么知道哪些属性值是要自动装配的?

// 通过bw的PropertyDescriptor属性名，查找出对应的Bean对象，将其添加到pvs中
autowireByName()---------------------------
->
// 获取bw中有setter方法 && 非简单类型属性 && mbd的PropertyValues中没有该pd的属性名的 PropertyDescriptor 属性名数组
String[] propertyNames = unsatisfiedNonSimpleProperties(mbd, bw);
// 遍历属性名
for (String propertyName : propertyNames) {
  // 如果该bean工厂有propertyName的beanDefinition或外部注册的singleton实例
  // 区分哪些在此时注入. 用 byName 还是 byType
  if (containsBean(propertyName)) {
    //获取该工厂中propertyName的bean对象
    Object bean = getBean(propertyName);
    //将propertyName,bean添加到pvs中
    pvs.add(propertyName, bean);
    //注册propertyName与beanName的依赖关系
    registerDependentBean(propertyName, beanName);
  }
}

// 定义 "按类型自动装配" (按类型bean属性)行为的抽象方法
autowireByType()---------------------------
->
// 使用bw作为类型转换器
// 之前说明过 BeanWrapper 继承了TypeConverter
converter = bw;

// 获取bw中有setter方法 && 非简单类型属性 && mbd的PropertyValues中没有该pd的属性名的 PropertyDescriptor 属性名数组
String[] propertyNames = unsatisfiedNonSimpleProperties(mbd, bw);
过滤出五个
address, books, maps, properties, set

// 根据据desc的依赖类型解析出与descriptor所包装的对象匹配的候选Bean对象
Object autowiredArgument = resolveDependency(desc, beanName, autowiredBeanNames, converter);
->
// 解析出与descriptor所包装的对象匹配的候选Bean对象
doResolveDependency()
此时 address 对象已经被创建
->
// 尝试针对desciptor所包装的对象类型是[stream,数组,Collection类型且对象类型是接口,Map]的情况，进行解析与依赖类型匹配的候选Bean对象
// 针对desciptor所包装的对象类型是[stream,数组,Collection类型且对象类型是接口,Map]的情况，进行解析与依赖类型匹配的 候选Bean对象，
// 并将其封装成相应的依赖类型对象
Object multipleBeans = resolveMultipleBeans(descriptor, beanName, autowiredBeanNames, typeConverter);
//尝试与type匹配的唯一候选bean对象
//查找与type匹配的候选bean对象,构建成Map，key=bean名,val=Bean对象
Map<String, Object> matchingBeans = findAutowireCandidates(beanName, type, descriptor);
->
// 添加一个条目在result中:一个bean实例(如果可用)或仅一个已解析的类型
addCandidateEntry(result, candidate, descriptor, requiredType);


--------------------------------------------------------------------------------------------------------------

继续 populateBean()

//工厂是否拥有InstiationAwareBeanPostProcessor
boolean hasInstAwareBpps = hasInstantiationAwareBeanPostProcessors();

//如果工厂拥有InstiationAwareBeanPostProcessor,那么处理对应的流程，主要是对几个注解的赋值工作包含的两个关键子类是
// CommonAnnoationBeanPostProcessor,
// AutowiredAnnotationBeanPostProcessor
if (hasInstAwareBpps) {
  
  	InstantiationAwareBeanPostProcessor ibp = (InstantiationAwareBeanPostProcessor) bp;
  
    //postProcessProperties:在工厂将给定的属性值应用到给定Bean之前，对它们进行后处理，不需要任何属性扫描符。该回调方法在未来的版本会被删掉。
    // -- 取而代之的是 postProcessPropertyValues 回调方法。
    // 让ibp对pvs增加对bw的Bean对象的propertyValue，或编辑pvs的proertyValue
    PropertyValues pvsToUse = ibp.postProcessProperties(pvs, bw.getWrappedInstance(), beanName);
  
    //postProcessPropertyValues:一般进行检查是否所有依赖项都满足，例如基于"Require"注释在 bean属性 setter，
    // 	-- 替换要应用的属性值，通常是通过基于原始的PropertyValues创建一个新的MutablePropertyValue实例， 添加或删除特定的值
    // 	-- 返回的PropertyValues 将应用于bw包装的bean实例 的实际属性值（添加PropertyValues实例到pvs 或者 设置为null以跳过属性填充）
    //回到ipd的postProcessPropertyValues方法
    pvsToUse = ibp.postProcessPropertyValues(pvs, filteredPds, bw.getWrappedInstance(), beanName);
}


调用接口 InstantiationAwareBeanPostProcessor 中的方法 postProcessProperties()  来进行属性值的设置工作
PropertyValues pvsToUse = ibp.postProcessProperties(pvs, bw.getWrappedInstance(), beanName);
->
// 完成bean中@Autowired，@Inject，@Value注解的解析并注入的功能
AutowiredAnnotationBeanPostProcessor # postProcessProperties()
// 处理注入注解元数据
CommonAnnotationBeanPostProcessor # postProcessProperties() 解析 @PreConstruct 和 @PostDestroy
跳到 AutowiredAnnotationBeanPostProcessor 里
		子类对象 AutowiredFieldElement 和 AutowiredMethodElement 的 inject() 方法 ?????? 怎么跳过去呀????
->
// 获取依赖的value值的工作  最终还是委托给beanFactory.resolveDependency()去完成的
// 这个接口方法由AutowireCapableBeanFactory提供，它提供了从bean工厂里获取依赖值的能力
value = beanFactory.resolveDependency(desc, beanName, autowiredBeanNames, typeConverter);
->
// 解析出与descriptor所包装的对象匹配的候选Bean对象
result = doResolveDependency(descriptor, requestingBeanName, autowiredBeanNames, typeConverter);
->
//尝试与type匹配的唯一候选bean对象
//查找与type匹配的候选bean对象,构建成Map，key=bean名,val=Bean对象
Map<String, Object> matchingBeans = findAutowireCandidates(beanName, type, descriptor);
->
// 添加一个条目在result中:一个bean实例(如果可用)或仅一个已解析的类型
addCandidateEntry(result, candidate, descriptor, requiredType);
->
// 将candidateName和其对应的Class对象绑定到candidates中
candidates.put(candidateName, getType(candidateName));
getType()
->
// 检查手动注册的单例,获取beanName注册的单例对象，但不会创建早期引用
Object beanInstance = getSingleton(beanName, false);


继续 doResolveDependency
			//如果instanceCandidate是Class实例
			if (instanceCandidate instanceof Class) {
          //让instanceCandidate引用 descriptor对autowiredBeanName解析为该工厂的Bean实例
          instanceCandidate = descriptor.resolveCandidate(autowiredBeanName, type, this);
			}
			// 断点在这里停
      // 定义一个result变量，用于存储最佳候选Bean对象
			Object result = instanceCandidate;
->
return beanFactory.getBean(beanName);
走getBean() -> doGetBean() -> createBean() -> doCreateBean() 创建流程, 能识别 @Autowired 注解了
  
  
继续 AutowiredAnnotationBeanPostProcessor # inject()
    // 通过反射，给属性赋值
    ReflectionUtils.makeAccessible(field);
    field.set(bean, value);
                
--------------------------------------------------------------------------------------------------------------

作业
自定义注解, 完成跟@Autowired一样的功能
self autowired包下, 照抄就可以

--------------------------------------------------------------------------------------------------------------
  
populateBean() # applyPropertyValues() 具体的属性值设置  <<<核心逻辑 !!!!!>>> 前面的方法一般都进不去
  
  流程:
	对propertyValues进行一系列的验证工作
	->
  循环遍历集合中存在的propertyValue
  ->
  分别获取name和value对象值
    // 获取属性的名字
    String propertyName = pv.getName();
    // 获取未经类型转换的值
    Object originalValue = pv.getValue();
  ->
  //交由valueResolver根据pv解析出originalValue所封装的对象
  调用此方法来完成value值的解析工作
  Object resolvedValue = valueResolver.resolveValueIfNecessary(pv, originalValue);
  对应很多类型, 分别调用不同resolve方法完成属性值的解析工作, 下面示例:
		RuntimeBeanReference
  	RuntimeBeanNameReference
  	BeanDefinitionHolder
    BeanDefinition
  	DependencyDescriptor
    ManagedArray
  	ManagedList
    ManagedSet
    ManagedMap
    ManagedProperties
    TypedStringValue
    NullBean
  --->
  if RuntimeBeanReference
  //解析出对应ref所封装的Bean元信息(即Bean名,Bean类型)的Bean对象:
	return resolveReference(argName, ref);
	->
  //获取resolvedName的Bean对象
	bean = this.beanFactory.getBean(resolvedName);
	!!!!!! populatedBean() 中有可能会重新调用 getBean() 方法来创建不同的对象
  --->
  if BeanDefinitionHolder
  //根据bdHolder所封装的Bean名和BeanDefinition对象解析出内部Bean对象
	return resolveInnerBean(argName, bdHolder.getBeanName(), bdHolder.getBeanDefinition());
      
自定义注解的<赋值功能>在这里实现
  //如果pvs不为null
  if (pvs != null) {
    //应用给定的属性值，解决任何在这个bean工厂运行时其他bean的引用。必须使用深拷贝，所以我们 不会永久地修改这个属性
    applyPropertyValues(beanName, mbd, bw, pvs);
  }
->
// 获取用户自定义类型转换器
TypeConverter converter = getCustomTypeConverter();
// 如果转换器为空，则直接把包装类赋值给converter
if (converter == null) {
  converter = bw;   没有 typeConverter 就用 beanWrapper对象
}  
  
Object resolvedValue = valueResolver.resolveValueIfNecessary(pv, originalValue);
对于具体值的解析, 逻辑很麻烦


//应用给定的属性值，解决任何在这个bean工厂运行时其他bean的引用。必须使用深拷贝，所以我们 不会永久地修改这个属性
// 可以是基本数据类型, 也可以是引用类型 list set map 包含很多细节处理工作
// pvs, bean 标签中的 Property 属性值
applyPropertyValues(beanName, mbd, bw, pvs);

值处理器根据具体的类型 map set list 调用其他不同的方法
对类型做判断: BeanDefinitionValueResolver # resolveValueIfNecessary() 方法
->
evaluate()
->
//如有必要(value可解析成表达式的情况下)，将value封装的value评估为表达式并解析出表达式的值
doEvaluate()
->
// 评估value,如果value是可解析表达式，会对其进行解析，否则直接返回value
return this.beanFactory.evaluateBeanDefinitionString(value, this.beanDefinition);
->
// 评估value作为表达式（如果适用）；否则按原样返回值
return this.beanExpressionResolver.evaluate(value, new BeanExpressionContext(this, scope));

SPEL表达式解析器
beanExpressionResolver 在 prepareBeanFactory 赋值

判断完成之后, 放到 deepCopy 结果集, 最后做整体处理

address对象: RuntimeBeanNameReference

构造器 解决不了循环依赖
要用set 将实例化和初始化分开

核心, 往 deepCopy 中设置属性值

具体的赋值操作
//按原样使用deepCopy构造一个新的MutablePropertyValues对象然后设置到bw中以对bw的属性值更新
bw.setPropertyValues(new MutablePropertyValues(deepCopy));
->
setPropertyValues()
setPropertyValue()
->
// 获取属性的访问权限
getPropertyAccessorForPropertyPath()
->
setPropertyValue()
->
processLocalProperty()
->
BeanWrapperImpl # setValue()
获取set方法
Method writeMethod = (this.pd instanceof GenericTypeAwarePropertyDescriptor ?
((GenericTypeAwarePropertyDescriptor) this.pd).getWriteMethodForActualAccess() :
this.pd.getWriteMethod());
完成赋值操作
writeMethod.invoke(getWrappedInstance(), value);


--------------------------------------------------------------------------------------------------------------

populateBean() 方法改造
1. implements InstantiationAwareBeanPostProcessor. 可以直接终止后续的值处理, 也可以被后续值处理覆盖
重写 postProcessAfterInstantiation 实例化之后
返回true or false 在这里控制 populateBean, 如果返回false, 后面不再处理, 不会被覆盖; 返回true会被覆盖
if (!ibp.postProcessAfterInstantiation(bw.getWrappedInstance(), beanName)) {
		return;
}
2. 根据配置文件的autowired属性 来决定使用名称注入还是类型注入
		autowireByName, autowireByType
3. 将对象中定义的@autowired注解进行解析, 并完成对象或者属性的注入
		postProcessProperties(), autowiredAnnotationBeanPostProcessor 类
		metadata.inject(bean, beanName, pvs);
4. 根据property标签定义的属性值, 完成各种属性值的解析和赋值工作
		applyPropertyValues(beanName, mbd, bw, pvs);

```



###  doCreateBean() # initializeBean()

```java
1. 调用执行aware接口的方法
		BeanNameAware, BeanClassLoaderAware, BeanFactoryAware
2. 执行before的初始化方法
  
  //如果mdb不为null || mbd不是"synthetic"。一般是指只有AOP相关的prointCut配置或者Advice配置才会将 synthetic设置为true
  if (mbd == null || !mbd.isSynthetic()) {
    // 将BeanPostProcessors应用到给定的现有Bean实例，调用它们的postProcessBeforeInitialization初始化方法。
    // 返回的Bean实例可能是原始Bean包装器
    wrappedBean = applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);
  }
  
		class ApplicationContextAwareProcessor implements BeanPostProcessor
      // 完成其他aware接口子类的值设置工作, 以下六个 aware 接口
      if (!(bean instanceof EnvironmentAware || bean instanceof EmbeddedValueResolverAware ||
            bean instanceof ResourceLoaderAware || bean instanceof ApplicationEventPublisherAware ||
            bean instanceof MessageSourceAware || bean instanceof ApplicationContextAware)) {
        return bean;
      }
      	
      
		CommonAnnotationBeanPostProcessor extends InitDestroyAnnotationBeanPostProcessor
		InitDestroyAnnotationBeanPostProcessor 中处理 @PreDestry @PostConstructor
		@PreDestry @PostConstructor 解析完成之后有一个register 用于标记属性
		InitDestroyAnnotationBeanPostProcessor # postProcessMergedBeanDefinition()
		->
		checkConfigMembers()
		->
    beanDefinition.registerExternallyManagedInitMethod(methodIdentifier);
    ->
    externallyManagedInitMethods
    
    AbstractAutowireCapableBeanFactory # initializeBean()
    ->
    invokeInitMethods()
    if (StringUtils.hasLength(initMethodName) &&
        !(isInitializingBean && "afterPropertiesSet".equals(initMethodName)) &&
        !mbd.isExternallyManagedInitMethod(initMethodName)) {
        // 在bean上调用指定的自定义init方法
        invokeCustomInitMethod(beanName, bean, mbd);
    }
    isExternallyManagedInitMethod 用于判断

		
3. 调用执行 init-method 执行初始化方法
		3.1 实现了 InitializeBean 接口之后 调用 afterPropertiesSet 方法 
				AbstractAutowireCapableBeanFactory # invokeInitMethods() 中
				isInitializingBean
				调用 afterPropertiesSet() 方法来设置属性
		3.2 调用执行用户自定义初始化方法 init-method
      	没有实现 InitializeBean 接口
     		调用这个方法
      	// 在bean上调用指定的自定义init方法
      	invokeCustomInitMethod(beanName, bean, mbd);
4. 执行after的初始化方法 <<<最关键核心是aop>>>
  	会生成具体对象的代理对象
		调的 AbstractAutoProxyCreator, 对应实现aop功能 
		生成的完整对象进行返回操作
		
		如果 @PostConstruct 的时候, 和 3.2 的时候, 都需要执行 自定义的init方法( 假设的前提 ), 怎么保证只调用一次?
		标记类
		
		


-------------------------------------------

initializeBean()

invokeAwareMethods()

		在前面构造方法忽略了
    public AbstractAutowireCapableBeanFactory() {
        super();
        // 忽略要依赖的接口
        ignoreDependencyInterface(BeanNameAware.class);
        ignoreDependencyInterface(BeanFactoryAware.class);
        ignoreDependencyInterface(BeanClassLoaderAware.class);
    }
    
    在 prepareBeanFactory 中也设置了一堆 6个, 在 ApplicationContextAwareProcessor 中进行处理
    
    
    可能的原因:
    接口 BeanFactory 大体可以分为两个分支
    1. AbstractBeanFactory
				ListableBeanFactory
				ConfigurationBeanFactory
				这个分支, 忽略三个的aware
    2. ApplicationContext
    		ClassPathXmlApplicationContext
    		AnnotationConfigApplicationContext
    		FileSystemXmlApplicationContext
    		这个分支, 忽略六个的aware
    		
    		
  修炼! 注意执行 before 和 after 当中每个类是具体干什么工作的
  
  afterPropertySet 最后修改bean的机会, 允许对赋值进行验证
  

```





### doCreateBean() # registerDisposableBeanIfNecessary()

```java

// 注册bean对象，方便后续在容器销毁的时候销毁对象
 registerDisposableBeanIfNecessary(beanName, bean, mbd);

回调, 设置需要销毁的对象
  
  // 注册一个一次性Bean实现来执行给定Bean的销毁工作：DestructionAwareBeanPostProcessors 一次性Bean接口，自定义销毁方法。
  // DisposableBeanAdapter：实际一次性Bean和可运行接口适配器，对给定Bean实例执行各种销毁步骤
  // 构建Bean对应的DisposableBeanAdapter对象，与beanName绑定到 注册中心的一次性Bean列表中
  registerDisposableBean(beanName,
                         new DisposableBeanAdapter(bean, beanName, mbd, getBeanPostProcessors(), acc));


```



### doCreateBean # 得到 exposedObject



```java



// 判断循环依赖, 注意此处抛出的异常
// 因为bean创建后所依赖的bean一定是已经创建的
// actualDependentBeans不为空则表示当前bean创建后其依赖的bean却没有全部创建完，也就是说存在循环依赖
if (!actualDependentBeans.isEmpty()) 


// BeanFactory 有bean的创建和销毁流程
// 注册bean对象，方便后续在容器销毁的时候销毁对象
registerDisposableBeanIfNecessary(beanName, bean, mbd);
->
// 注册一个一次性Bean实现来执行给定Bean的销毁工作：DestructionAwareBeanPostProcessors 一次性Bean接口，自定义销毁方法。
// DisposableBeanAdapter：实际一次性Bean和可运行接口适配器，对给定Bean实例执行各种销毁步骤
// 构建Bean对应的DisposableBeanAdapter对象，与beanName绑定到 注册中心的一次性Bean列表中
registerDisposableBean(beanName, new DisposableBeanAdapter(bean, beanName, mbd, getBeanPostProcessors(), acc));



return 到 getSingleton() 当中
// 创建单例之前的回调,默认实现将单例注册为当前正在创建中
beforeSingletonCreation(beanName);
// 创建单例后的回调,默认实现将单例标记为不在创建中
afterSingletonCreation(beanName);

				// 生成了新的单例对象, 往缓存中加对象
				if (newSingleton) {
					// 将beanName和singletonObject的映射关系添加到该工厂的单例缓存中:
          // 将生成的完整对象设置到一级缓存中, 方便后续去进行获取
					addSingleton(beanName, singletonObject);
				}

retrun 到 doGetBean 当中
// 从beanInstance中获取公开的Bean对象，主要处理beanInstance是FactoryBean对象的情况，如果不是
// FactoryBean会直接返回beanInstance实例
bean = getObjectForBeanInstance(sharedInstance, name, beanName, mbd);

之后主要是作用域scope的一些设置

-----------------------------------------------------------

bean的生命周期流程图:
定义好xml文件, java类之后
创建容器对象obtainFreshBeanFactory
		创建容器defaultListableBeanFactory
		设置一些属性
		加载配置文件loadBeanDefinitions
				document -> element -> parseDefaultElement(): 处理 bean 标签
				parseCustomElement(): context / aop / 自定义标签
				得到BeanDefinition
				GenericBeanDefinition -> 合并 -> RootBeanDefinition (后续调用才会触发)
prepareBeanFactory
		给容器工厂设置某些属性值
invokeBeanFactoryPostProcessor
		调用执行BFPP, 可以修改或者引入其他的BeanDefinition. (可以用来修改或者添加GenericBeanDefinition)
		但是需要注意, BFPP针对的操作对象是BeanFactory
		关键类: ConfigurationClassPostProcessor 用来完成对相关注解的解析工作
		@Import @resource @component-scan @bean
registerBeanPostProcessor -- 实例化BPP
		完成BeanPostProcessor的注册工作, 方便后续再实例化完成之后调用 before 和 after 方法
finishBeanFactoryInitialization
		完成对对象的创建工作
		先从容器中找, 找不到再创建
		getBean -> doGetBean
		将需要创建的bean对象放到数组中, 挨个进行创建
		取出BD, 转换成RootBeanDefinition, 进行具体的实例化操作
		createBean -> doCreateBean -> createBeanInstance
		创建bean的方式:
				supplier
				factoryMethod
				通过反射的方式创建
				BPP代理  (前面步骤完成)
		applyMergedBeanDefinitionPostProcessors
				注册生命周期接口
				@PostConstruct, @PreDestroy, @Resource, @Autowired, @Value
		populateBean 
				填充属性
				创建依赖的bean对象, getBean, doGetBean
		initializeBean
				进行初始化工作
						执行 invokeAwareMethod (BeanNameAware, BeanClassLoaderAware, BeanFactoryAware)
						执行 BPP 的 before 方法
								ApplicationContextAwareProcessor 继续实现某些aware接口的set方法 
								CommonAnnotationBeanPostProcessor 执行@PostConstruct, @PreDestroy
						invokeInitMethods
								实现了InitializingBean ? 调用afterPropertiesSet(最后一次修改属性值)
								执行用户自定义的init-method
						执行BPP的after方法
								AOP
		获取对象来进行相关操作
		销毁流程 beanFactory 接口
				DestructionAwareBeanPostProcessor # postProcessBeforeDestruction()
				DisposableBean
				自定义的custom destroy-method
		
		ac.close 关闭 application context   
		
```





### 循环依赖问题

```java

一级缓存
BeanName : bean实例
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256);

二级缓存
BeanName : bean实例
与singletonFactories的不同之处在于，
当一个单例bean被放到这里之后，那么当bean还在创建过程中, 就可以通过getBean方法获取到，可以方便进行循环依赖的检测
private final Map<String, Object> earlySingletonObjects = new ConcurrentHashMap<>(16);

三级缓存
用于保存 BeanName 和 创建bean的工厂 之间的关系
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<>(16);

根据 DefaultSingletonBeanRegistry # getSingleton() 方法中, 先后顺序区分一级/二级/三级缓存
为什么必须要三级缓存?
如果发生循环依赖的对象, 不需要代理的话, 只需要二级缓存足以解决所有问题
但是当存在代理对象之后就无法解决了, 必须要使用三级缓存来解决

如果A类包含b属性, B类包含a属性
A的实例化, 初始化, 填充属性b
从容器中获取b对象 ? 直接赋值 : B的实例化, 初始化, 填充属性a
从容器中获取a对象 ? 直接赋值 : `创建A`

必须保证第二次不走 `创建A` 这个步骤, 即可以从容器中获取a对象
提前暴露对象:
对象的创建分为 实例化 和 初始化, 实例化好但是未完成初始化的对象是可以直接给其他对象引用的, 所以此时可以做一件事,
把完成实例化但是未完成初始化的对象提前暴露出去, 让其他对象能够进行引用, 就完成了这个闭环的解环操作

debug: ----------------------------------------------------------------------------
创建出a后 A@1536
添加三级缓存的地方 doCreateBean # addSingletonFactory()
addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
key: beanName
value: () -> getEarlyBeanReference(beanName, mbd, bean)
->
populateBean() 给对象A填充属性
->
applyPropertyValues()
-> 
pvs, 要填充的属性值, 对于每一个pv
Object resolvedValue = valueResolver.resolveValueIfNecessary(pv, originalValue);
->
RuntimeBeanReference
resolveReference()
->
这里知道需要处理的对象是b
					//让resolvedName引用ref所包装的Bean名
					resolvedName = String.valueOf(doEvaluate(ref.getBeanName()));
					//获取resolvedName的Bean对象
					bean = this.beanFactory.getBean(resolvedName);
->
doGetBean()
->
getSingleton() 从三级缓存中取
isSingletonCurrentlyInCreation() 用来标记当前属性是否在创建过程中 
这个属性中是在 beforeSingletonCreation() 中进行标记的
处理b的时候进不去getSingleton, 缓存中没有
->
sharedInstance = getSingleton(beanName, () -> {
    try {
        // 为给定的合并后BeanDefinition(和参数)创建一个bean实例
        return createBean(beanName, mbd, args);
    }
}
->
doCreateBean()

createBeanInstance()
此时, B对象已经创建, B@1937, A@1536, 两个对象都是半成品
但是, a和b两个对象都只是完成实例化, 没有完成初始化

// 往三级缓存加东西. 此时一二级缓存应该没有东西
// 为避免后期循环依赖，可以在bean初始化完成前将创建实例的ObjectFactory加入工厂
addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));

加入
key: b; value: lambda表达式

->
populateBean()
->
applyPropertyValues()
Object originalValue = pv.getValue(); B的初始化, 填充a属性
此时还是 RuntimeBeanReference 类型, 运行时的bean引用
Object resolvedValue = valueResolver.resolveValueIfNecessary(pv, originalValue);
->
resolveReference()

// 获取B对象的属性值a
//让resolvedName引用ref所包装的Bean名
resolvedName = String.valueOf(doEvaluate(ref.getBeanName()));
//获取resolvedName的Bean对象
bean = this.beanFactory.getBean(resolvedName);

此时一级二级缓存中没有 A, B对象, 只有三级缓存中有创建 A, B 对象的lambda表达式
->
getBean -> doGetBean -> getSingleton -----------------------------------------------------
创建出未初始化的A, 未初始化的B, 一级二级缓存都没有, 但是三级缓存里有创建A, B的lambda表达式

从 一级缓存 和 二级缓存都取不到, 
注意判断条件 isSingletonCurrentlyInCreation 为 true, allowEarlyReference 默认为 true

// 当某些方法需要提前初始化的时候则会调用addSingletonFactory方法将对应的ObjectFactory初始化策略存储在singletonFactories
ObjectFactory<?> singletonFactory = this.singletonFactories.get(beanName);
if (singletonFactory != null) {
    // 如果存在单例对象工厂，则通过工厂创建一个单例对象
    singletonObject = singletonFactory.getObject();
    // 记录在缓存中，二级缓存和三级缓存的对象不能同时存在
    this.earlySingletonObjects.put(beanName, singletonObject);
    // 从三级缓存中移除
    this.singletonFactories.remove(beanName);
}

其中, singletonFactory.getObject(); 跳转到
AbstractAutowireCapableBeanFactory # doCreateBean() # addSingletonFactory() # getEarlyBeanReference()
// 为避免后期循环依赖，可以在bean初始化完成前将创建实例的ObjectFactory加入工厂
addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
这里走 getEarlyBeanReference(), 
此时 bean 为 A, 属性 b 为 null. A{b=null}
if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) 这里进不去
最后return exposedObject; A{b=null}, 返回的是A@1536
return 后注意 singletonFactory.getObject() 的后续方法, 移除三级缓存加入二级
->
然后 populateBean 完成 B对象属性a的赋值, 
此时 B 是一个完整的对象 B@1937, 属性  a = A{b=null}
-> 返回到
继续 getBean -> doGetBean -> getSingleton() 方法
// 生成了新的单例对象
if (newSingleton) {
    // 将beanName和singletonObject的映射关系添加到该工厂的单例缓存中:
    addSingleton(beanName, singletonObject);
}
此处将B对象放入一级缓存 !!!!!!!

然后又返回, 此处三级缓存被清空, A对象在二级缓存, B对象在一级缓存, B对象已经完整, A对象不完整, 给A对象的b属性赋值
完成 populateBean 后, A有B, B有A, 可以点开很多个
最后在 addSingleton 中将 A 放入一级缓存, 二级三级缓存移除


---------------------------------------------------------------------------


三级缓存中并不是严格的从三级到二级到一级 这样一个对象转化顺序, 
有可能在三级和一级中有对象, 也有可能在一二三级中都有对象

如果没有代理的话, 二级缓存也能完成这个操作
如何改变源码中的三级缓存引用关系, 将三级缓存变成二级缓存? 改哪些代码?
1. 在 doCreateBean 中有添加lambda表达式到三级缓存的代码, 要改成直接放到二级缓存中
2. 直接修改 getSingleton 方法, 将三级缓存操作去掉

如果去掉三级缓存, 并使用aop, 报错:
doCreateBean() 中源码:
if (!actualDependentBeans.isEmpty()) {
throw new BeanCurrentlyInCreationException(beanName,
    "Bean with name '" + beanName + "' has been injected into other beans [" +
    StringUtils.collectionToCommaDelimitedString(actualDependentBeans) +
    "] in its raw version as part of a circular reference, but has eventually been " +
    "wrapped. This means that said other beans do not use the final version of the " +
    "bean. This is often the result of over-eager type matching - consider using " +
    "'getBeanNamesForType' with the 'allowEagerInit' flag turned off, for example.");
}

思考: 为什么有代理的时候, 使用三级缓存能解决问题? spring是怎么解决的?

之前三级缓存中, B对象的lambda表达式并没有被调用过, 但是A对象的singletonFactory.getObject();方法有被调用, 注意链路
// 为避免后期循环依赖，可以在bean初始化完成前将创建实例的ObjectFactory加入工厂
addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
->
exposedObject = ibp.getEarlyBeanReference(exposedObject, beanName);
实现类 AbstractAutoProxyCreator # getEarlyBeanReference() 应该是代理对象有问题


------------------------------------------------------------------------------------------

将代码修改为只有两级缓存 !!!!!!!!!!!! 放开代理, 追踪链路:

doCreateBean() 当中
instanceWrapper = createBeanInstance(beanName, mbd, args); 创建出A对象
        if (earlySingletonExposure) {
            // 为避免后期循环依赖，可以在bean初始化完成前将创建实例的ObjectFactory加入工厂
            addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
            
            // or
            //只保留二级缓存，不向三级缓存中存放对象
            //			earlySingletonObjects.put(beanName,bean);
            //			registeredSingletons.add(beanName);
        }
        
populateBean() # 
        // 否是"synthetic"。一般是指只有AOP相关的pointCut配置或者Advice配置才会将 synthetic设置为true
        // 如果mdb是不是'syntheic'且工厂拥有InstantiationAwareBeanPostProcessor
        if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) 
        if (!ibp.postProcessAfterInstantiation(bw.getWrappedInstance(), beanName)) 
        加上aop配置之后, 这里会包含一些BeanPostProcessor 
        
applyPropertyValues(beanName, mbd, bw, pvs); 到这里还是 A{b=null}, 进去开始赋值
Object resolvedValue = valueResolver.resolveValueIfNecessary(pv, originalValue);
RuntimeBeanReference
return resolveReference(argName, ref);

//让resolvedName引用ref所包装的Bean名
resolvedName = String.valueOf(doEvaluate(ref.getBeanName()));
//获取resolvedName的Bean对象
bean = this.beanFactory.getBean(resolvedName);

此时从容器中拿对象, 只有两级缓存, 拿不到B对象, 开始创建B对象
getBean() -> doGetBean() -> createBean() -> doCreateBean() -> createBeanInstance()
同样, 在earlySingletonExposure判断中把B对象放入二级缓存

B对象填充a属性
populateBean -> applyPropertyValues -> resolveValueIfNecessary -> resolveReference -> getBean
-> doGetBean -> getSingleton
可以从二级缓存中拿到A对象

完成给B对象赋值后, B对象里面有a, A{b=null}, 此时A, B都在二级缓存中
在 doCreateBean 中 earlySingletonExposure 第二个判断中
Object earlySingletonReference = getSingleton(beanName, false);
此时 earlySingletonReference != null, 能进去

返回到 getSingleton 当中 执行addSingleton时, 把代理的B对象放入一级缓存, 清除二级缓存的B对象

然后返回到给A对象赋值b属性, 其中b属性是代理的B对象

此时 A对象包含的是代理b, 代理B当中的a是空
A@1947 -> cglibB@3279
cglibB@3279 -> null
而 B@2469 -> A@1947

返回到doCreateBean # 第二个earlySingletonExposure判断 # getDependentBeans
需要往当前的依赖集合actualDependentBeans加一个值, 所以不为空, 报错

在整个容器的生命周期中, 可能存在几个同名的B对象, 就是有错的
A@1947
cglibA@3620
B@2469
cglibB@3279
代理对象什么时候生成的? BPP. after 方法

怎么解决这个问题: 三级缓存, 为什么?
在获取具体的对象的时候, 直接生成其代理对象, 而不要原来的对象, 通过lambda表达式动态生成
在整个三级缓存中, 对象仅能存在一份

AbstractAutowireCapableBeanFactory # getEarlyBeanReference()
要么返回原始bean, 要么返回代理生成的bean, 只可能返回一个
保证全局对象有且只有一个

不使用三级缓存, 直接额外方法获取生成代理对象也可以








```







## refresh12: finishRefresh();

## refresh exception finally

```java

						try {

            } catch (BeansException ex) {

                // 为防止bean资源占用，在异常处理中，销毁已经在前面过程中生成的单件bean
                destroyBeans();

                // 重置active标志
                cancelRefresh(ex);

                throw ex;
            } finally {
                // Reset common introspection caches in Spring's core, since we
                // might not ever need metadata for singleton beans anymore...
                resetCommonCaches();
            }

```







# AOP 面向切面编程



## 核心对象的创建

```java
动态代理: jkd / cglib. 查看文档: 07动态代理.md

可能会有某些bean对象, 并不是原始的bean对象.

IOC

AOP概念: 
aspect
advise
pointcut

需要定义三个东西:
额外添加的业务逻辑
位置来进行额外逻辑的执行
是怎么筛选这个具体的位置

我们怎么做?
1. 先编辑额外的逻辑类 -> 切面
2. 具体的某些方法要被执行处理 -> 切点: 之前, 之后;  adviser(通知器)  advise(通知)
3. 被应用在哪些方法上 -> 连接点

1. 哪些类需要相关的切入 expression
2. 额外逻辑处理中, 有几个通知消息, 或者说有哪些逻辑可以被执行
		before after afterThrowing afterReturning around: advisor -> (advise)
3. 额外处理逻辑的类, 也就是哪个切面 aspect

      
method对象
beanFactory对象, 根据beanName 取出对应的bean
动态代理 -> 字节码文件 -> 需要自动手动操作(反编译) javac javap 或者工具类
调用 AspectJAroundAdvice 的时候, 一定要准备好 method对象 / beanFactory对象. 其构造函数:
public AspectJAroundAdvice(Method aspectJAroundAdviceMethod, AspectJExpressionPointcut pointcut, AspectInstanceFactory aif) {
	super(aspectJAroundAdviceMethod, pointcut, aif);
}

-------------------------------------------------------------------------------------------------------

使用方式: xml / 注解 ConfigurationClassPostProcessor

1. 查看配置文件的解析工作, 在处理之后的beanDefinition中包含什么信息
		beandefinition的解析工作 (loadBeanDefinitions, default 和 custom)
		涉及到beaDefinition的封装过程
2. 对aop相关的beanDefinition进行实例化操作
3. 之前实例化: beanNames.for 挨个创建
	 			创建这些对象之前需要准备工作? 如果对象需要动态代理, 要把method对象, factory对象提前准备好
	 			在进行第一个对象创建之前, 就必须把aop相关的对象提前准备好, 因为无法预估哪些对象需要动态代理
	 			
	 在哪个步骤中可以提前实例化并且生成对应的对象????
	 BFPP是用来对beanfactory进行修改操作的, 不会影响后面的实例化过程
	 但是BPP可以(befor, after), 其子接口
	 
	 之前哪个步骤可以提前通过动态代理的方式创建出对象????
	 createBean # resolveBeforeInstantiation !!!!!!!
   // 给BeanPostProcessors一个机会来返回代理来替代真正的实例，应用实例化前的前置处理器,用户自定义动态代理的方式，针对于当前的被代理类需要经过标准的代理流程来创建对象
   Object bean = resolveBeforeInstantiation(beanName, mbdToUse);
   ->
   applyBeanPostProcessorsBeforeInstantiation
   ->
   postProcessBeforeInstantiation
   实现子类 AbstractAutoProxyCreator # postProcessBeforeInstantiation 包含 createProxy() 方法, 创建代理
   
   
-------------------------------------------------------------------------------------------------------
  
  解析aop标签, <aop:config>
  parseBeanDefinitions() 
  -> 
  parseCustomElement() 
  根据命名空间的url找到对应的handler的处理器
  -> 
  ConfigBeanDefinitionParser # parse()
    
      类似于 AnnotationConfigUtils / AopConfigUtils 注入 internalAutoProxyCreator
    AopConfigUtils # registerAspectJAutoProxyCreatorIfNecessary() 注册了 			
    		AspectJAwareAdvisorAutoProxyCreator.class
    registerAspectJAnnotationAutoProxyCreatorIfNecessary() 注册了 
        AnnotationAwareAspectJAutoProxyCreator.class // 是 AspectJAwareAdvisorAutoProxyCreator 的子类
  
    		ConfigBeanDefinitionParser # parse() 源码:
        public BeanDefinition parse(Element element, ParserContext parserContext) {
        	CompositeComponentDefinition compositeDef =
            new CompositeComponentDefinition(element.getTagName(), parserContext.extractSource(element));
          parserContext.pushContainingComponent(compositeDef);
          // 注册自动代理模式创建器,AspectjAwareAdvisorAutoProxyCreator
          configureAutoProxyCreator(parserContext, element);
          // 解析aop:config子节点下的 aop:pointcut / aop:advice / aop:aspect
          List<Element> childElts = DomUtils.getChildElements(element);
          for (Element elt: childElts) {
            String localName = parserContext.getDelegate().getLocalName(elt);
            if (POINTCUT.equals(localName)) {
              parsePointcut(elt, parserContext);
            }
            else if (ADVISOR.equals(localName)) {
              parseAdvisor(elt, parserContext);
            }
            else if (ASPECT.equals(localName)) {
              parseAspect(elt, parserContext);
            }
          }
          parserContext.popAndRegisterContainingComponent();
          return null;
        }
  ->
  parseAspect() // 解析<aop:aspect>标签
    
  // 解析其下的advice节点
	NodeList nodeList = aspectElement.getChildNodes();
	// 循环解析advice节点
  nodeList.for {
    // 是否是 advice 接口的五个标签
  	// 是否为advice:before/advice:after/advice:after-returning/advice:after-throwing/advice:around节点
  	if (isAdviceNode(node, parserContext)) {
        // 解析advice节点并注册到bean工厂中
      	// parseAdvice() 完成 <aop:around> 标签的解析, 一定会调到 AspectJAroundAdvice ???
  			AbstractBeanDefinition advisorDefinition = parseAdvice(aspectName, i, aspectElement, 
                            (Element) node, parserContext, beanDefinitions, beanReferences);
				beanDefinitions.add(advisorDefinition);
    }
  }
  ->
  AspectJPointcutAdvisor #1 #2 #3 #4 #5, 分为五个, 都是继承aspect的接口. 其中一个
  AspectJAroundAdvice: 注意下列对象的成员变量
  0 MethodLocatingFactoryBean 代表一个method对象
  1 表达式 AspectJExpressionPointcut
  2 SimpleBeanFactoryAwareAspectInstanceFactory, 实现BeanFactoryAware接口, 获取beanFactory 代表工厂
  ->
  parseAdvice() 
    // 涉及point-cut属性的解析，并结合上述的两个bean最终包装为AbstractAspectJAdvice通知对象
    AbstractBeanDefinition adviceDef = createAdviceDefinition(adviceElement, parserContext, 
            aspectName, order, methodDefinition, aspectFactoryDef, beanDefinitions, beanReferences);
    
  -> 
  进入 createAdviceDefinition()
  // getAdviceClass() 根据标签元素获取具体的类
	// 首先根据adviceElement节点分析出是什么类型的Advice
	RootBeanDefinition adviceDefinition = new RootBeanDefinition(getAdviceClass(adviceElement, parserContext));
	...
  // 解析point-cut属性
	Object pointcut = parsePointcutProperty(adviceElement, parserContext);
  ->
  继续 parseAdvice()
  // 在 advice 对象外面包装 advisor
  // AspectJPointcutAdvisor
  RootBeanDefinition advisorDefinition = new RootBeanDefinition(AspectJPointcutAdvisor.class);
  ->
  继续 parseAspect()
  // 将解析完成的标签放到一个统一的对象
  AspectComponentDefinition aspectComponentDefinition = createAspectComponentDefinition();
  
  // 解析aop:point-cut节点并注册到bean工厂
  List<Element> pointcuts = DomUtils.getChildElementsByTagName(aspectElement, POINTCUT);
  for (Element pointcutElement : pointcuts) {
    parsePointcut(pointcutElement, parserContext);
  }
	// AspectJExpressionPointcut
  在 
  parsePointcut() -> createPointcutDefinition() -> new RootBeanDefinition(AspectJExpressionPointcut.class);
  注入表达式, 这个表达式的scope是原型模式 BeanDefinition.SCOPE_PROTOTYPE
  
-------------------------------------------------------------------------------------------------------
  
  想让注解使用aop生效, 除了配置componentScan, 还需要配置 @EnableAspectJAutoProxy 注解, 注册aop标签的解析类
  
  @Import(AspectJAutoProxyRegistrar.class)
	public @interface EnableAspectJAutoProxy
    
  AspectJAutoProxyRegistrar # registerBeanDefinitions() 源码:
  AopConfigUtils.registerAspectJAnnotationAutoProxyCreatorIfNecessary(registry);
    
    类似于 AnnotationConfigUtils / AopConfigUtils 注入 internalAutoProxyCreator
    AopConfigUtils # registerAspectJAutoProxyCreatorIfNecessary() 注册了 			
    		AspectJAwareAdvisorAutoProxyCreator.class
    registerAspectJAnnotationAutoProxyCreatorIfNecessary() 注册了 
        AnnotationAwareAspectJAutoProxyCreator.class // 是 AspectJAwareAdvisorAutoProxyCreator 的子类
	
  注意, 配置文件和注解不能独立开, 是一样的解析过程
  
  复习 parseCustomElement 的解析过程
  spring-aop -> src -> main -> resources -> META-INF -> spring.handlers
  http\://www.springframework.org/schema/aop=org.springframework.aop.config.AopNamespaceHandler
  此类注入
  	public class AopNamespaceHandler extends NamespaceHandlerSupport {
      @Override
      public void init() {
        registerBeanDefinitionParser("config", new ConfigBeanDefinitionParser());
        // 注意这一行
        registerBeanDefinitionParser("aspectj-autoproxy", new AspectJAutoProxyBeanDefinitionParser());
        registerBeanDefinitionDecorator("scoped-proxy", new ScopedProxyBeanDefinitionDecorator());
        registerBeanDefinitionParser("spring-configured", new SpringConfiguredBeanDefinitionParser());
      }
    }
  最后注入 AnnotationAwareAspectJAutoProxyCreator 对象
  
  AnnotationAwareAspectJAutoProxyCreator 是 AspectJAutoProxyBeanDefinitionParser 的子类, 归属于 BPP
  在 registerBeanPostProcessors 创建好, 后面可以直接用了
    
  parseCustomElement()
  ->
  getNamespaceURI()
  根据命名空间找到对应的handler, 执行 handler.parse()
  
-------------------------------------------------------------------------------------------------------
  
     今日课程总结
   准备beanDefinition
   1. AspectJExpressionPointcut
   2. advisor 1 ~ 4
   3. AnnotationAwareAspectJAutoProxyCreator
    
   问题: aop是怎么实现的事务?
   PlatformTransactionManager
   DataSourceTransactionManager # doGetTransaction()
   涉及到事务的提交 回滚
  
-------------------------------------------------------------------------------------------------------
   
   // 初始化剩下的单实例（非懒加载的）
   finishBeanFactoryInitialization()
   
   在创建 logUtil 的时候, 
   createBean() 
   -> 
   // 给BeanPostProcessors一个机会来返回代理来替代真正的实例，应用实例化前的前置处理器,用户自定义动态代理的方式，针对于当前的被代理类需要经过标准的代理流程来创建对象
   Object bean = resolveBeforeInstantiation(beanName, mbdToUse);
   ->
   applyBeanPostProcessorsBeforeInstantiation()
   ->
   AbstractAutoProxyCreator # postProcessBeforeInstantiation()
   ->
   // shouldSkip() 这个方法中会进行advisor对象的创建!!!
   AspectJAwareAdvisorAutoProxyCreator # shouldSkip() 注意是子类实现
     
            // 找出Aspect切面和解析通知器的方法，通知器Advisor里面有通知Advice实例
            @Override
            protected boolean shouldSkip(Class<?> beanClass, String beanName) {
              // findCandidateAdvisors() 查找候选的advisor对象, 一共五个, before, after, ...
              List<Advisor> candidateAdvisors = findCandidateAdvisors();
              // 如果是切面对象, 后面是不需要被代理, 所以这里有判断
              for (Advisor advisor : candidateAdvisors) {
                if (advisor instanceof AspectJPointcutAdvisor &&
                    ((AspectJPointcutAdvisor) advisor).getAspectName().equals(beanName)) {
                  return true;
                }
              }
              return super.. shouldSkip(beanClass, beanName);
            }
     
    在 postProcessBeforeInstantiation() 中,
   		shouldSkip返回true 直接跳过, 返回null值, 不走下面的createProxy逻辑. 
        意味着这里logUtil 不会执行 createProxy() 方法进行动态代理
   			之后创建logUtil的方式, 会调用其无参构造方法, 然后往里初始化, 返回结果值
   
   之后走 doCreateBean, 
   populateBean(beanName, mbd, instanceWrapper); 并不会赋值, 因为没有属性
   exposedObject = initializeBean(beanName, exposedObject, mbd);
   ->
   在 initializeBean() 中, applyBeanPostProcessorsBeforeInitialization !!!!!!!!
   // 如果mdb不为null || mbd不是"synthetic"。
   // 一般是指只有AOP相关的prointCut配置或者Advice配置才会将 synthetic设置为true
   // logUtil对象的 synthetic 属性值 (翻译为"合成的") 为 false
   if (mbd == null || !mbd.isSynthetic()) {
       // 将BeanPostProcessors应用到给定的现有Bean实例，调用它们的postProcessBeforeInitialization初始化方法。
       // 返回的Bean实例可能是原始Bean包装器
       wrappedBean = applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);
   }
   ... 省略
   postProcessBeforeInstantiation()
   AnnotationAwareAspectJAutoProxyCreator # isInfrastructureClass()
   shouldSkip() 这个方法中会进行advisor对象的创建!!!
   shouldSkip() -> findCandidateAdvisors() 此处不需要重新创建 advisor 对象了, 从容器中 getBean()
   shouldSkip() 的父类 AutoProxyUtils.isOriginalInstance(beanName, beanClass);
   
   在 initializeBean() 中, applyBeanPostProcessorsAfterInitialization !!!!!!!!
   if (mbd == null || !mbd.isSynthetic()) {
       // 将BeanPostProcessors应用到给定的现有Bean实例，调用它们的postProcessAfterInitialization方法。
       // 返回的Bean实例可能是原始Bean包装器
       wrappedBean = applyBeanPostProcessorsAfterInitialization(wrappedBean, beanName);
   }
   AbstractAutoProxyCreator # postProcessAfterInitialization()
       // 判断当前bean是否正在被代理，如果正在被代理则不进行封装
       if (this.earlyProxyReferences.remove(cacheKey) != bean) {
       // 如果它需要被代理，则需要封装指定的bean
          return wrapIfNecessary(bean, beanName, cacheKey);
       }
   

-------------------------------------------------------------------------------------------------------
  
  
   		 wrapIfNecessary() 创建具体的动态代理对象
       wrapIfNecessary 也出现在 bean创建 往三级缓存放对象的时候:
       doCreateBean()
       // 为避免后期循环依赖，可以在bean初始化完成前将创建实例的ObjectFactory加入工厂
       addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
       AbstractAutoProxyCreator # getEarlyBeanReference()
       return wrapIfNecessary(bean, beanName, cacheKey);
       
       logUtil 需要被代理吗? 不需要
       虽然进到 wrapIfNecessary() 里面去, 但是被 下面这个判断 打回来了
       和 resolveBeforeInstantiation() 的逻辑一致
       // 这里advisedBeans缓存了已经进行了代理的bean，如果缓存中存在，则可以直接返回
       if (Boolean.FALSE.equals(this.advisedBeans.get(cacheKey))) {
       		return bean;
       }
       
       具体的执行过程:
       当使用 spring 的 aop 的时候, 需要进行n多个对象的创建, 
       但是在创建过程中需要做很多判断, 判断当前对象是否需要被代理,
       而代理之前, 需要的 advisor 对象必须要提前创建好, 才能进行后续的判断
       问:
       如果定义了一个普通的对象, 会进入 resolveBeforeInstantation() 方法的处理吗?
   		 会, 这个方法判断当前容器里有没有 instantionBPP, 有的话会进行处理
   		 动态处理的时候, 用来判断是否需要被代理, 前面一定要把 advisor 对象创建好
   
   		一个对象需要被代理, 那么它的普通对象也会被创建.
   		只不过在后续的创建流程中, 会对普通对象进行一个替换而已
   		
   		继续创建 myCalculator
   		doGetBean() -> getSingleton() -> createBean()
   		-> 
   		resolveBeforeInstantiation() 
   		-> 
   		postProcessBeforeInstantiation()
   		
   		
-------------------------------------------------------------------------------------------------------
   		
   		
   		JDK / Cglib
   		最终都是为了得到class文件
   		注意执行流程
   
   		在 initializeBean() # applyBeanPostProcessorsAfterInitialization()
   		wrapIfNecessary()
   		此时 shouldSkip() 不会被跳过 
   		->
   		// 获取当前bean的Advices和Advisors
   		getAdvicesAndAdvisorsForBean()
   		
   		关键接口 MethodInterceptor, 还可以实现其他接口吗?  搜索类 MyCglib
   		CallbackInfo 类, 其static代码块
			        CALLBACKS = new CallbackInfo[]{
			        	new CallbackInfo(NoOp.class, NoOpGenerator.INSTANCE), 
			        	new CallbackInfo(MethodInterceptor.class, MethodInterceptorGenerator.INSTANCE), 
			        	new CallbackInfo(InvocationHandler.class, InvocationHandlerGenerator.INSTANCE), 
			        	new CallbackInfo(LazyLoader.class, LazyLoaderGenerator.INSTANCE), 
			        	new CallbackInfo(Dispatcher.class, DispatcherGenerator.INSTANCE), 
			        	new CallbackInfo(FixedValue.class, FixedValueGenerator.INSTANCE), 
			        	new CallbackInfo(ProxyRefDispatcher.class, DispatcherGenerator.PROXY_REF_INSTANCE)};

  	InvocationHandler 在 JDK实现动态代理的时候必须实现
  	包含其中类型, MethodInterceptor, InvocationHandler 最常见
  	跳转到 AbstractAdvisorAutoProxyCreator # getAdvicesAndAdvisorsForBean()
      
      // 检查前面切面解析是否有通知器advisors创建，有就返回，没有就是null
      @Override
      @Nullable
      protected Object[] getAdvicesAndAdvisorsForBean(
          Class<?> beanClass, String beanName, @Nullable TargetSource targetSource) {
        // 找合适的增强器对象
      	// eligible 符合条件的
      	// 动态代理之前, 要返回对应的拦截器. 这里面一定会有通过expression进行筛选的过程
      	// 找符合要求的advisor
      	// 复习advisor 的类图
        List<Advisor> advisors = findEligibleAdvisors(beanClass, beanName);
        // 若为空表示没找到
        if (advisors.isEmpty()) {
          return DO_NOT_PROXY;
        }
        return advisors.toArray();
      }
  		->
      // 对获取到的所有Advisor进行判断，看其切面定义是否可以应用到当前bean，从而得到最终需要应用的Advisor
			List<Advisor> eligibleAdvisors = findAdvisorsThatCanApply(candidateAdvisors, beanClass, beanName);
  	  	
  	表达式:
		<aop:pointcut id="myPoint" 
    		expression="execution( Integer com.mashibing.aop.xml.service.MyCalculator.*  (..))"/>
    
    introduction 理解为类级别, 而不是方法级别
    introductionInfo 接口
    所以的都是方法级别的
    	->
      // 从候选的通知器中找到合适正在创建的实例对象的通知器
			return AopUtils.findAdvisorsThatCanApply(candidateAdvisors, beanClass);
			->
    先匹配类在匹配方法
    // 真正的判断增强器是否合适当前类型
    if (canApply(candidate, clazz, hasIntroductions)) {
   		 eligibleAdvisors.add(candidate);
    }
    先匹配类, 再匹配方法. 注意类 ClassFilter 类过滤器  MethodMatcher 方法匹配. 这是两个接口
    
    在 pointcut 里面有这两个对象 classFilter 和 methodMatcher. 这两个对象在
        构建对象 public class AspectJExpressionPointcut extends AbstractExpressionPointcut
        		implements ClassFilter, IntroductionAwareMethodMatcher, BeanFactoryAware
		时完成赋值
    通过这两个值来判断 candidate 中的 advisor 是不是符合条件 -> canApply方法
    AopUtils # canApply()
    
    findEligibleAdvisors -------------------------------

          
    findEligibleAdvisors()
   	// 对获取到的所有Advisor进行判断，看其切面定义是否可以应用到当前bean，从而得到最终需要应用的Advisor
		List<Advisor> eligibleAdvisors = findAdvisorsThatCanApply(candidateAdvisors, beanClass, beanName);
		// 提供的hook方法，用于对目标Advisor进行扩展
		extendAdvisors(eligibleAdvisors);
    ->
    AspectJProxyUtils.makeAdvisorChainAspectJCapableIfNecessary(candidateAdvisors);
    ->
    // ExposeInvocationInterceptor 就是用来传递 MethodInvocation的。
		// 在后续的任何下调用链环节，只要需要用到当前的 MethodInvocation 就通过	
    // ExposeInvocationInterceptor.currentInvocation() 静态方法获得
    advisors.add(0, ExposeInvocationInterceptor.ADVISOR);
    
    mi.process
    
    // 获取拦截器链里面的 mi MethodInvocation对象
    AbstractAspectJAdvice # currentJoinPoint
    // 获取当前的MethodInvocation 即ReflectiveMethodInvocation的实例
		MethodInvocation mi = ExposeInvocationInterceptor.currentInvocation();

```

## aop整体流程图

```java
   AOP一二节课整体逻辑流程图
    1. 遍历当前容器中存在的beanDefinition, 并进行对象的创建工作
    2. 从list中按照顺序开始进行对象的创建
    3. getBean()
    4. doGetBean()
    5. createBean()
    6. resolveBeforeInstantiation()
    		// 给BeanPostProcessors一个机会来返回代理来替代真正的实例，应用实例化前的前置处理器,用户自定义动态代理的方式，针对于当前的被代理类需要经过标准的代理流程来创建对象
     		Object bean = resolveBeforeInstantiation(beanName, mbdToUse);
    		在当前BPP中存在一个 AutoProxyCreator 的对象, 因此会执行后续的判断逻辑
    7. AbstractAutoProxyCreator # postProcessorBeforeInstantiation()
    8. 判断当前处理的对象是否需要被代理 shouldSkip 判断
    		shouldSkip 对 步骤8 的描述补充:
				注意这里是子类实现的 AspectJAwareAdvisorAutoProxyCreator # shouldSkip() 这个方法中会进行advisor对象的创建!!!
    		->
    		创建动态代理需要的 advisor 对象
    		->
    		遍历当前容器中存在的 advisor 的 beanDefinition
    		->
    		经过循环将所有的 advisor 对象准备好
            ->
            1. findCandidateAdvisor() 
          			注意子类实现 AnnotationAwareAspectJAutoProxyCreator # findCandidateAdvisor()
                @Override
                protected List<Advisor> findCandidateAdvisors() {
                  // 找到系统中实现了Advisor接口的bean
                  List<Advisor> advisors = super.findCandidateAdvisors();
                  if (this.aspectJAdvisorsBuilder != null) {
                    // 找到系统中使用@Aspect标注的bean，并且找到该bean中使用@Before，@After等标注的方法，
                    // 将这些方法封装为一个个Advisor
                    advisors.addAll(this.aspectJAdvisorsBuilder.buildAspectJAdvisors());
                  }
                  return advisors;
                }
            2. buildAdvisor() 标注了 @Aspect 切面的对象
            ->
            创建过程比较麻烦, 包含了N多个对象的迭代创建, 包括 AspectJAroundAdvice 
            (MethodLocationFactoryBean, AspectJExpressionPointcut, SimpleBeanFactoryAwareAspectInstanceFactory)
    		->
    		判断能否匹配成功(判断能否跳过)
    9.1 如果跳过的话, 直接返回空对象
    		doCreateBean
    		->
    		createBeanInstance
    		->
    		populateBean
    		->
    		initializeBean
    		->
    		BPP 的 after 方法, 判断是否需要被代理 
    		1. 是, 判断是否需要被跳过
    		1.1 是, 直接返回普通对象
    		1.2 否, 获取符合条件的 advisor
    					获取容器中存在的advisor
    					进行规则的匹配 (通过表达式在类上和方法上进行匹配, 具体的匹配方式看源码)
    							匹配不上: 直接跳转下一个
    							匹配上: 通过 expression 进行验证匹配, 添加到符合条件的集合
    									添加 exposeInvocationInterceptor 的advisor
    									进行排序操作
    				-> 完成1.2 获取符合条件的 advisor
    				进行代理的创建
    				createProxy
    				未完待续.... jdk/cglib
    		2. 如果不是, 直接返回普通对象
    				返回步骤2: 从list中按照顺序开始进行对象的创建
    9.2	如果不可以跳过, 判断是否有自定义的 targetSource     如果有, 自行创建; 没有, 自行退出
```



## createProxy

### jdk 创建动态代理详解

```java
参考包: package com.mashibing.proxy.jdk;

程序在运行期间生成 class 文件, 通过class文件生成具体的对象
 
 把生成的class文件保存到本地
 System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");
 
 Object proxy = Proxy.newProxyInstance(classLoader, interfaces, invocationHandler);
 ->
 // 用来生成具体的class文件
 Class<?> cl = getProxyClass0(loader, intfs);
 ->
 // 实现接口个数不能超过65535个
 if (interfaces.length > 65535) {
   throw new IllegalArgumentException("interface limit exceeded");
 }
 // proxyClassCache : 简单的缓存对象, 类似于map
 // 类似于spring容器, 容器有, 从缓存里面取; 容器没有, 走创建流程
 // 包含很多java8的新特性, supplier, bifunction
 // private static final WeakCache<ClassLoader, Class<?>[], Class<?>> proxyClassCache 
 //			    = new WeakCache<>(new KeyFactory(), new ProxyClassFactory());
 return proxyClassCache.get(loader, interfaces);
 -> get() 方法进去
   
 		ConcurrentMap<Object, Supplier<V>> oldValuesMap = map.putIfAbsent(cacheKey, valuesMap = new ConcurrentHashMap<>());

     private final BiFunction<K, P, ?> subKeyFactory;
     private final BiFunction<K, P, V> valueFactory;
     
     BiFunction 函数式接口, 泛型前两个代表参数, 最后一个代表结果. 在 WeakCache 创建的时候已经完成赋值
     
     subKeyFactory.apply(key, parameter)
     在初始化的时候赋值是构造函数传入的subKeyFactory
     因此跳转到 Proxy 类 # KeyFactory 类 # apply() 方法
     
     // 此处相当于获取了一个接口 interface Calculator
     Object subKey = Objects.requireNonNull(subKeyFactory.apply(key, parameter));
     Supplier<V> supplier = valuesMap.get(subKey);
     // 此处的factory 是在 proxy 类里面的 ProxyClassFactory, 包含核心处理逻辑
     Factory factory = null;
     
     第二次if (supplier != null) 
     可以执行 V value = supplier.get();
     跳转到 WeakCache # get()
 -> 
     WeakCache 类 # Factory 类 implements Supplier 接口 # get() (继承Supplier接口的方法)
 		 value = Objects.requireNonNull(valueFactory.apply(key, parameter));
 		 apply 跳转到
 		 Proxy 类 # ProxyClassFactory 类 # apply()方法
 		 此处才是正儿八经处理字节码生成的一个类
 		 
 		 // 如果一个类实现n多个接口, 所有接口的方法都要实现
     Map<Class<?>, Boolean> interfaceSet = new IdentityHashMap<>(interfaces.length);

		 String proxyPkg = null; 根据访问修饰符去判断生成的代理类放在什么包里面
     // if no non-public proxy interfaces, use com.sun.proxy package
     proxyPkg = ReflectUtil.PROXY_PACKAGE + ".";
     
     // 生成具体的class文件
     byte[] proxyClassFile = ProxyGenerator.generateProxyClass(proxyName, interfaces, accessFlags);
     
 -> generateProxyClass
 			final byte[] var4 = var3.generateClassFile();
 			->
 				// 添加hashCode, equals, toString 这三个方法. 在Object里面会自定义实现, 每次调用的时候回直接访问Object里面的方法
 				// 此时访问的是代理类, 保证访问代理对象也是通过invoke这样的方式才能访问到, 所以提前准备好
        this.addProxyMethod(hashCodeMethod, Object.class);
        this.addProxyMethod(equalsMethod, Object.class);
        this.addProxyMethod(toStringMethod, Object.class);
        
        // for接口, for方法, 然后添加方法
        this.addProxyMethod(var8, var4);
        
        checkReturnTypes 挨个验证返回值
        
        // 生成构造方法, 实例化进行属性值设置的时候, 需要获取到InvocationHandler, 就是作为构造器的参数设置进去的
        this.methods.add(this.generateConstructor()); 
        
        // 把 proxyMethods 集合中的代理方法放到 methods 集合中去
        this.fields.add(new ProxyGenerator.FieldInfo(var16.methodFieldName, "Ljava/lang/reflect/Method;", 10));
        this.methods.add(var16.generateMethod());
        
        // 生成的静态代码块
        this.methods.add(this.generateStaticInitializer());
        
        OutputStream 输出到一个地方
    ->
    返回到 final byte[] var4 = var3.generateClassFile();
    此时var4中 都是 coffee baby, 字节码
    ->
    返回到 byte[] proxyClassFile = ProxyGenerator.generateProxyClass(proxyName, interfaces, accessFlags);
    
    defineClass0 把刚刚生存的内存数组加载到内存当中去
    
    ->
    getProxyClass0 有返回值了
    
    final Constructor<?> cons = cl.getConstructor(constructorParams); 获取构造器
    return cons.newInstance(new Object[]{h}); 反射 传入 invocationHandler
        点进去newInstance
        T inst = (T) ca.newInstance(initargs);
        返回具体代理对象
        
        然后执行 proxy.add(1,1);
        执行时跳到代理对象, super.h.invoke, 跳到 CalculatorProxy # InvocationHandler
        之后执行method.invoke 调到具体的子类实现 MyCalculator 返回结果
          
          
```

### cglib 创建动态代理详解

```java
    public static void main(String[] args) {
        //动态代理创建的class文件存储到本地
        System.setProperty(DebuggingClassWriter.DEBUG_LOCATION_PROPERTY,"d:\\code");
        //通过cglib动态代理获取代理对象的过程，创建调用的对象,在后续创建过程中EnhanceKey的对象，所以在进行enhancer对象创建的时候需要把EnhancerKey（newInstance）对象准备好,恰好这个对象也需要动态代理来生成
        Enhancer enhancer = new Enhancer();
        //设置enhancer对象的父类
        enhancer.setSuperclass(MyCalculator.class);
        //设置enhancer的回调对象
        enhancer.setCallback(new MyCglib());
        //创建代理对象
        MyCalculator myCalculator = (MyCalculator) enhancer.create();
        //通过代理对象调用目标方法
        myCalculator.add(1,1);
        System.out.println(myCalculator.getClass());
    }
    
    Enhancer 创建该对象的时候会注册缓存 ClassLoaderData, 比较重要, 断点打到 new Enhancer


MethodIntercept 其实是一种回调方法, 用得最多
还有很多回调方法, 放在 CallbackInfo.class
        CALLBACKS = new CallbackInfo[]{
        		new CallbackInfo(NoOp.class, NoOpGenerator.INSTANCE), 
        		new CallbackInfo(MethodInterceptor.class, MethodInterceptorGenerator.INSTANCE), 
        		new CallbackInfo(InvocationHandler.class, InvocationHandlerGenerator.INSTANCE), 
        		new CallbackInfo(LazyLoader.class, LazyLoaderGenerator.INSTANCE), 
        		new CallbackInfo(Dispatcher.class, DispatcherGenerator.INSTANCE), 
        		new CallbackInfo(FixedValue.class, FixedValueGenerator.INSTANCE), 
        		new CallbackInfo(ProxyRefDispatcher.class, DispatcherGenerator.PROXY_REF_INSTANCE)};
        		
        		
        FastClass 是一个抽象类, cglib 在运行时通过 fastclass 内的 generator 这个内部类将其子类动态生成出来, 然后再利用 classloader 将生成的子类加载进jvm里面去
        
        new Enhancer()
        ->
        // 使用key工厂创建出对应class的代理类，后面的KeyFactory_HASH_ASM_TYPE即代理类中创建HashCode方法的策略
				private static final EnhancerKey KEY_FACTORY = (EnhancerKey) KeyFactory.create(EnhancerKey.class, KeyFactory.HASH_ASM_TYPE, null);
        ->
        KeyFactory # create()
        ->
        AbstractClassGenerator # create()
        
        // 当前类加载器对应的缓存  缓存key为类加载器，缓存的value为ClassLoaderData，
        // 可以理解为一个缓存对象，只不过此缓存对象中包含的是具体的业务逻辑处理过程，有两个function的函数式接口，
        // 一个是返回gen.key,对应的名称叫GET_KEY, 还有一个是为了创建具体的class，名字叫做load
			  Map<ClassLoader, ClassLoaderData> cache = CACHE;
			  
        if (data == null) {
            // 新建一个缓存Cache，并将之前的缓存Cache的数据添加进来，并将已经被gc回收的数据给清除掉
            Map<ClassLoader, ClassLoaderData> newCache = new WeakHashMap<ClassLoader, ClassLoaderData>(cache);
            // 新建一个当前加载器对应的ClassLoaderData并加到缓存中，但ClassLoaderData中此时还没有数据
            data = new ClassLoaderData(loader);
            newCache.put(loader, data);
            // 刷新全局缓存
            CACHE = newCache;
        }
					
              注意初始化 new ClassLoaderData() 的两个function函数式接口
              // 可以理解为一个缓存对象，只不过此缓存对象中包含的是具体的业务逻辑处理过程，有两个function的函数式接口，
        			// 一个是返回gen.key,对应的名称叫GET_KEY, 还有一个是为了创建具体的class，名字叫做load
              // 返回当前生成器的key值
              private static final Function<AbstractClassGenerator, Object> GET_KEY = 
              		new Function<AbstractClassGenerator, Object>() {
                  public Object apply(AbstractClassGenerator gen) {
                      return gen.key;
                  }
              };
              // 新建一个回调函数，这个回调函数的作用在于缓存中没获取到值时，调用传入的生成的生成代理类并返回
              // 返回具体的class对象
              Function<AbstractClassGenerator, Object> load = new Function<AbstractClassGenerator, Object>() {
                  public Object apply(AbstractClassGenerator gen) {
                      Class klass = gen.generate(ClassLoaderData.this);
                      return gen.wrapCachedClass(klass);
                  }
              };
              // 为这个ClassLoadData新建一个缓存类
              generatedClasses = new LoadingCache<AbstractClassGenerator, Object, Object>(GET_KEY, load);
      
			// 设置一个全局key
			this.key = key;
      // 在刚创建的data(ClassLoaderData)中调用get方法 并将当前生成器，
			// 以及是否使用缓存的标识穿进去 系统参数 System.getProperty("cglib.useCache", "true")
			// 返回的是生成好的代理类的class信息
			Object obj = data.get(this, getUseCache());
			->
      // 传入代理类生成器 并根据代理类生成器获取值返回
      Object cachedValue = generatedClasses.get(gen);
      ->
      // 在此处, this 代表 generatedClasses = new LoadingCache<AbstractClassGenerator, Object, Object>(GET_KEY, load);
      // 看构造函数, 所以 this.keyMapper 代表 GET_KEY, 这里调用的是 GET_KEY 的 apply 
      // 之前有一个步骤 this.key = key; , 这里只是返回 Enhance # EnhanceKey
      KK cacheKey = this.keyMapper.apply(key);
      ->
      this.createEntry(key, cacheKey, v);
      
      // load 里面的 apply 方法
      task = new FutureTask(new Callable<V>() {
          public V call() throws Exception {
          		return LoadingCache.this.loader.apply(key);
          }
      });
      当 task.run(); 执行的时候, 执行的是 load 中的 apply() 方法,
      ->
      laod 中 apply1 : Class klass = gen.generate(ClassLoaderData.this);
      
      // 生成代理类名字
      String name = generateClassName(data.getUniqueNamePredicate());
      
      // 生成字节码
      byte[] b = strategy.generate(this); ----------------------------
      跳转到 DefaultGeneratorStrategy # generate()
      
      // DebuggingClassWriter 中 debugLocation 设置字节码生成目录
      DebuggingClassWriter cw = this.getClassVisitor();
      this.transform(cg).generateClass(cw);
      跳转到 KeyFactory # generateClass()
      (((( 第二次跳转到 enhancer # generateClass ))))
      
      ----------------------------
      // 获取到字节码代表的class的名字
			String className = ClassNameReader.getClassName(new ClassReader(b));
			// 设置访问作用域
			ProtectionDomain protectionDomain = getProtectionDomain();
			synchronized (classLoader) { // just in case
				// SPRING PATCH BEGIN
				// 往内存中进行加载
				gen = ReflectUtils.defineClass(className, b, classLoader, protectionDomain, contextClass);
				// SPRING PATCH END
			}

			laod 中 apply2 : return gen.wrapCachedClass(klass);
        
        
      ----------------------------------- 完成 enhance 对象的创建 -----------------------------------
      
      在后续过程中, 需要一个 enhancerKey 的对象. 
      所以创建 enhance 对象的时候, 需要把 enhancerKey (包含 newInstance 方法) 对象准备好, 恰好这个对象也需要动态代理来生成
      
      MyCalculator myCalculator = (MyCalculator) enhancer.create();
      ->
      return createHelper();
      ->
      // 通过newInstance方法来创建EnhancerKey对象，正常情况下，只需要new一个对象就可以调用方法了，
      // 但是Key_Factory是一个EnhancerKey类型，是一个内部接口，需要动态代理来实现，最终是为了调用newInstance方法
      Object key = KEY_FACTORY.newInstance(
      		(superclass != null) ? superclass.getName() : null,
          ReflectUtils.getNames(interfaces),
          filter == ALL_ZERO ? null : new WeakCacheKey<CallbackFilter>(filter),
          callbackTypes,
          useFactory,
          interceptDuringConstruction,
          serialVersionUID);

    // 调用父类即AbstractClassGenerator的创建代理类
    Object result = super.create(key);
    跳转到 AbstractClassGenerator # create() 方法
    
    回调方法当做参数传入: 不知道什么时候触发

```





## wrapIfNecessary # createProxy()

wrapIfNecessary() 方法 debug 定位到 MyCalculator, 此对象需要被动态代理



```java
		// 一共六个 advisor 对象, 除了五个还有一个 expose advisor 对象
		// 获取当前bean的Advices和Advisors
		Object[] specificInterceptors = getAdvicesAndAdvisorsForBean(bean.getClass(), beanName, null);
		
		有接口用jdk, 没接口用cglib, 但是在spring中有接口也不一定用jdk
		
		
		当使用cglib创建代理对象的时候, 需要先创建一个 enhancer 的对象
		key_factory的对象(Enhancer.EnhancerKey类型, 内部接口, 没有实现类), 需要通过动态代理的方式来实现
		设置superClass
		设置callback
		enhancer.create()
		
		advisor 通知器. 包含 advice 和 pointcut
		advice 具体的某一个消息通知
		adviced 用来配置代理(即proxyFactory)
		
		wrapIfNecessary # createProxy()
    // 真正创建代理对象
		return proxyFactory.getProxy(getProxyClassLoader());
		->
    // createAopProxy() 用来创建我们的代理工厂
		return createAopProxy().getProxy(classLoader);
		->
		CglibAopProxy # getProxy()
		
		// 用户没有自定义, 但是spring中添加了两个 SpringProxy, SpringAdviced
		enhancer.setInterfaces(AopProxyUtils.completeProxiedInterfaces(this.advised));
		
		// 获取callbacks
    Callback[] callbacks = getCallbacks(rootClass);
    		// 对于expose-proxy属性的处理,是否暴露当前对象为ThreadLocal模式，在当前上下文中能够进行引用
				boolean exposeProxy = this.advised.isExposeProxy();
				exposeProxy 举例:
				某个代理类中包含m1和m2两个方法, 当分别调用两个方法的时候, 能否执行通知? 可以
				如果m1调用m2的方法, 那么在调用m2的时候会执行通知吗? 不会执行, 如果想让它执行的话, 必须设置expose-proxy属性值为true
				...
				最后返回一个拦截器的集合(参见下图   粘贴进来的图片!!!!!!!)
				
    // 通过 Enhancer 生成代理对象，并设置回调
    return createProxyClassAndInstance(enhancer, callbacks);
    ->
    enhancer.create()
    ->
    this.createHelper()
		->
		Object result = super.create(key);
		
		动态代理对象的创建过程
		
		

		
		
		
```

![image-20220615165200875](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0dk3uxj20nu09sgms.jpg)





## add 方法调用过程

```java
		add 方法调用过程
		
		--------------------------------------------------------------------
		后续执行的时候, 
    MyCalculator bean = ac.getBean(MyCalculator.class);
    bean.add(1,1);
    ->
    首先会调到 MyCalculator 的字节码对象 add() 方法
    methodInterceptor var10000 = this.CGLIB$CALLBACK_0, 往上找 setCallbacks 的数组第一个, 即下图的第一个
    主要到执行 div mul 的时候 都是 CALLBACK_0
    ->
    DynamicAdvisedInterceptor # intercept() 方法
    
		当生成代理对象之后, 应该进行方法的调用了. 
		但是此时, 有6个advisor, 他们在执行的时候试试按照某个顺序来执行的, 而且由一个通知会跳转到另一个通知,
		所以此时, 需要构建一个拦截器链(责任链模式), 只有创建好当前链式结构, 才能顺利往下执行
		
		拓扑排序(sortAdvisors) -> 进行通知的排序操作 -> 排序后的结果
		
		0. exposeInvocationInterceptor
		1. after throwing 抛异常后 后续执行具体逻辑 返回步骤1
		2. after returning 返回后 后续执行具体逻辑 返回步骤1  --- aspect j after returning advice 没有 invoke 方法 
		3. after 后续执行具体逻辑     AspectJAfterAdvice
		4. around    AspectJAroundAdvice # invoke()方法 没有看到 mi.proceed() 调用
		5. before    AspectJMethodBeforeAdvice # before 方法
		
		
		执行结果:
		log---环绕通知start: add方法开始执行, 参数为[1,1]     around
		log---add方法开始执行: 参数是[1,1]										before
		log---环绕通知stopadd方法执行结束										 around
		log---环绕返回通知: add方法返回结果是: 2					     around
		log---add方法执行结束...over                        after
		log---add方法执行结束, 结果是: 2               			after returning
		
		在 around 的执行逻辑过程中会调用 before 来执行
		before 是在 around 中间过程中执行的
		
		exposeInvocationInterceptor 调用下一个的时候, 必须通过他
		根据索引的下标一次获取对应的通知来执行, 相当于是联系者
		通过调用super.proceed()
		
		
		跳转到 CglibAopProxy # intercept()
		
		
		执行mi.proceed 代表要回到 invocation interceptor 执行下一个逻辑了
      
      
   	思考 :
    五个消息通知, 为什么只有三个适配器

    DefaultAdvisorAdapterRegistry # 构造方法
		
```





## 拦截器链的执行



```java
MyCalculator bean = ac.getBean(MyCalculator.class);
bean 包含了7个回调对象
bean.add(1,1);
->
跳转到 CglibAopProxy # DynamicAdvisedInterceptor 类 # intercept() 方法

TargetSource targetSource = this.advised.getTargetSource();
在 wrapIfNecessary中() 有, targetSource最终放的是myCalculator这个对象
		// 根据获取到的Advices和Advisors为当前bean生成代理对象
			Object proxy = createProxy(
					bean.getClass(), beanName, specificInterceptors, new SingletonTargetSource(bean));
					
// 获取拦截器链
// 从advised中获取配置好的AOP通知
List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);
在整个创建过程中, 一共包含6个 advisor 对象, 挨个创建
advised 代表 proxyFactory 对象
->
getInterceptorsAndDynamicInterceptionAdvice()
->
跳转到 DefaultAdvisorChainFactory # getInterceptorsAndDynamicInterceptionAdvice()

// Registry, 代表对某个对象的增删改操作
// 这里用了一个单例模式 获取 DefaultAdvisorAdapterRegistry 实例
// 在Spring中把每一个功能都分的很细，每个功能都会有相应的类去处理 符合单一职责原则的地方很多 这也是值得我们借鉴的一个地方
// AdvisorAdapterRegistry这个类的主要作用是 <<<将Advice适配为Advisor>>> <<<将Advisor适配为对应的MethodInterceptor>>>
AdvisorAdapterRegistry registry = GlobalAdvisorAdapterRegistry.getInstance();
这个register其实是 DefaultAdvisorAdapterRegistry, 在初始化时候注册了三个 adapter 适配器. 
为什么只注册这三个? MethodBeforeAdviceAdapter / AfterReturningAdviceAdapter / ThrowsAdviceAdapter
  // 此方法把已有的advice实现的adapter加入进来
	public DefaultAdvisorAdapterRegistry() {
		registerAdvisorAdapter(new MethodBeforeAdviceAdapter());
		registerAdvisorAdapter(new AfterReturningAdviceAdapter());
		registerAdvisorAdapter(new ThrowsAdviceAdapter());
	}

// 返回六个通知, expose, before, after, after throwing, after returning, around
Advisor[] advisors = config.getAdvisors();

for (Advisor advisor : advisors) 
    methodMatch 匹配表达式的
    前期验证的时候, 用advisors去匹配类中所有的方法, 有一个方法就返回
    所以后面会再做一次验证
    matches 用于匹配
    
// 拦截器链是通过AdvisorAdapterRegistry来加入的，这个AdvisorAdapterRegistry对advice织入具备很大的作用
// 通过 advisor 返回 methodInterceptor
MethodInterceptor[] interceptors = registry.getInterceptors(advisor);
getInterceptors() 方法中, for (AdvisorAdapter adapter : this.adapters)
这里注意加适配器的作用: advisor中哪些实现了methodInterceptor, 通过适配器来判断能否获取对应的拦截器

AspectJMethodBeforeAdvice 没有  ----  对应适配器 MethodBeforeAdviceAdapter
AspectJAfterReturningAdvice 没有  ----  对应适配器  AfterReturningAdviceAdapter

实现了 MethodInterceptor
AspectJAfterAdvice 实现
AspectJAfterThrowingAdvice 实现
AspectJAroundAdvice 实现

三个适配器, 都实现了 AdvisorAdapter 接口:
MethodBeforeAdviceAdapter
AfterReturningAdviceAdapter
ThrowsAdviceAdapter
  
public interface AdvisorAdapter {
	boolean supportsAdvice(Advice advice);
	MethodInterceptor getInterceptor(Advisor advisor);
}

一切都是为了拓展使用

如果想自定义一个拦截器, 定义一个消息通知

for 循环结束后有6个interceptor, 这六个有明显的分类
为什么要做这样的分类?
定义一个 MyAspectJMethodBeforeAdvise

原本可以所有的advise 都实现 methodInterceptor 接口, 但是为了提高拓展性, 还需要提供适配器的模式, 
那么在进行 methodInterceptor 组装的时候需要多加额外的判断逻辑, 不能添加两次, 
所以可以把实现了 methodInterceptor 接口的某些advice 直接用过适配器来实现, 而不需要通过原来的方式实现了

继续 CglibAopProxy # intercept()
// 从advised中获取配置好的AOP通知
List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);

// 拦截器链, 需要提前创建出一个method invocation 并执行其中的 proceed 方法
// 通过cglibMethodInvocation来启动advice通知
retVal = new CglibMethodInvocation(proxy, target, method, args, targetClass, chain, methodProxy).proceed();


return super.proceed();
->
计算下标
在拦截器链中, 第一个是 ExposeInvocationInterceptor, 调用到里面的invoke 方法
有内部对象
  private static final ThreadLocal<MethodInvocation> invocation = new NamedThreadLocal<>("Current AOP method invocation");
->
return mi.proceed();
->
后面又会调用
return super.proceed();

前年的东西都会直接调到链中去
从around方法的时候回有点变化, 

ProceedingJoinPoint pjp = lazyGetProceedingJoinPoint(pmi);
->
CglibMethodInvocation # proceed

...

// 验证返回值类型
retVal = processReturnType(proxy, target, method, retVal);

责任链模式




-----------------------------------------------------
  
注意 advisor的执行顺序, 使用注解的时候, 标签在代码中的上下顺序会影响通知的前后顺序
可以使用 @Order注解 控制吗?

在使用xml方式的时候, ConfigBeanDefinitionParser # parseAspect() 方法中, 
for (int i = 0; i < nodeList.getLength(); i++) 
// 解析advice节点并注册到bean工厂中
AbstractBeanDefinition advisorDefinition = parseAdvice(aspectName, i, aspectElement, (Element) node, 
                                                       parserContext, beanDefinitions, beanReferences);
其中 i 传入的是 order 值, 也就是说 advisor 是按照xml 定义的顺序执行的

        
```





## aop 复习

```java
在创建代理对象的过程中, 是通过 wrapIfNecessary() 方法来创建对象的, 此过程不再赘述
->
获取代理对象, 并且执行对应的方法来进行处理
->
看到的是反编译之后的字节码文件
调用任何方法, 执行的都是 DynamicAdvisedInterceptor 对象当中的 intercept() 方法, 开始执行具体的流程
->
// 从advised中获取配置好的AOP通知
List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);
getInterceptorsAndDynamicInterceptionAdvice()
获取整个消息通知的责任链, 按照链式结构依次调用执行

拦截器中包含多少个对象, 由之前定义的消息通知来决定, 如果全部定义的话, 包含的是6个
  AspectJMethodBeforeAdvice
  AspectJAfterAdvice
  AspectJAfterReturningAdvice
	AspectJAfterThrowingAdvice
  AspectJAroundAdvice
  ExposeInvocationInterceptor
	声明事务的时候: TransactionInterceptor
->
  开始调用具体的方法
  DynamicAdvisedInterceptor # intercept() 
  ...
  // 通过cglibMethodInvocation来启动advice通知
  retVal = new CglibMethodInvocation(proxy, target, method, args, targetClass, chain, 
                                     methodProxy).proceed();
	CglibAopProxy 的内部类
  private static class CglibMethodInvocation extends ReflectiveMethodInvocation
 	创建 CglibMethodInvocation 对象 # 调用 proceed() 方法: return super.proceed();
	
 	注意, proceed() 实际上调用的是父类 ReflectiveMethodInvocation 的 proceed() 方法, 
			然后从拦截器链中, 根据指定的下标 依次获取到每一个元素来进行相关的处理
  ReflectiveMethodInvocation 中 
  public Object proceed() throws Throwable {} 方法: 通过下标获取 advice 对象

->
ReflectiveMethodInvocation # proceed() 中: 获取执行的元素
	从 -1 的下标位置开始依次获取对应的元素
  先获取 0号位置的元素, ExposeInvocationInterceptor: 
	// 获取下一个要执行的拦截器，沿着定义好的interceptorOrInterceptionAdvice链进行处理
	Object interceptorOrInterceptionAdvice = this.interceptorsAndDynamicMethodMatchers.get(++this.currentInterceptorIndex);
  // 普通拦截器，直接调用拦截器，将this作为参数传递以保证当前实例中调用链的执行
  return ((MethodInterceptor) interceptorOrInterceptionAdvice).invoke(this);
->
  ExposeInvocationInterceptor # invoke()
  public Object invoke(MethodInvocation mi) throws Throwable {
    MethodInvocation oldInvocation = invocation.get();
    // 当前的mi对象 是 CglibAopProxy对象
  	// 把当前对象设置到 ThreadLocal 变量中, 方便在链中直接进行获取, 并且根据原来的下标依次获取元素
  	// 这里的invocation什么时候执行? currentInvocation
    invocation.set(mi);
    try {
      // 所以这里执行proceed() 方法的时候, 跳回到 CglibMethodInvocation # proceed()
      // 然后是super.proceed, 跳转到 ReflectiveMethodInvocation # proceed()
      // 从 0号下标 -> 1号下标
      return mi.proceed();
    }
    finally {
      invocation.set(oldInvocation);
    }
  }
->
  获取 1号位置的 afterThrowing 元素
  		AspectJAfterThrowingAdvice # invoke()
 			什么处理工作都不做, 直接向下执行, 获取下一个元素, 
			因为afterThrowing对应的是程序执行过程中出现的异常情况, 此时还没有执行任何业务代码, 所以直接跳过  
      ... 判断是否有异常, 如果有, 执行异常处理逻辑, 没有直接返回
      @Override
      public Object invoke(MethodInvocation mi) throws Throwable {
        try {
          // 执行下一个通知/拦截器  methodInvocation
          return mi.proceed();
        }
        catch (Throwable ex) {
          // 抛出异常
          if (shouldInvokeOnThrowing(ex)) {
            // 执行异常通知
            invokeAdviceMethod(getJoinPointMatch(), null, ex);
          }
          throw ex;
        }
      }
->
  获取 2号位置的 afterReturning 元素
      AfterReturningAdviceInterceptor # invoke()
      什么处理工作都不做, 直接哎正常的业务逻辑执行完成之后再进行调用
      ... 执行后续的处理逻辑
      @Override
      public Object invoke(MethodInvocation mi) throws Throwable {
        // 执行下一个通知/拦截器
        Object retVal = mi.proceed();
        // 返回通知方法
        this.advice.afterReturning(retVal, mi.getMethod(), mi.getArguments(), mi.getThis());
        return retVal;
    	}
->
  获取 3号位置的 after 元素
      AspectJAfterAdvice # invoke()
			什么操作都不做, 在正常的业务逻辑执行完成之后在进行调用
      后置通知的方法总是会被执行, 原因是invokeAdviceMethod()语句在finally中
      ... 执行后置的处理逻辑
  		@Override
  		public Object invoke(MethodInvocation mi) throws Throwable {
        try {
          // 执行下一个通知/拦截器
          return mi.proceed();
        }
        finally {
          // 后置通知的方法总是会被执行,原因就在这finally
          invokeAdviceMethod(getJoinPointMatch(), null, null);
        }
			}
->
  获取 4号位置的 around 元素
  		AspectJAroundAdvice # invoke()
      @Override
      public Object invoke(MethodInvocation mi) throws Throwable {
        if (!(mi instanceof ProxyMethodInvocation)) {
          throw new IllegalStateException("MethodInvocation is not a Spring ProxyMethodInvocation: " + mi);
        }
        ProxyMethodInvocation pmi = (ProxyMethodInvocation) mi;
        ProceedingJoinPoint pjp = lazyGetProceedingJoinPoint(pmi);
        JoinPointMatch jpm = getJoinPointMatch(pmi);
        return invokeAdviceMethod(pjp, jpm, null, null);
      }
      开始执行 around 消息通知的执行逻辑, 在业务流程执行之前先执行 around 的<<前置逻辑>>
        		AspectJAroundAdvice # invoke()
        		return invokeAdviceMethod(pjp, jpm, null, null);
						->
            return invokeAdviceMethodWithGivenArgs(argBinding(jp, jpMatch, returnValue, t));
						->
            // 反射调用通知方法
						// this.aspectInstanceFactory.getAspectInstance()获取的是切面的实例
						return this.aspectJAdviceMethod.invoke(this.aspectInstanceFactory.getAspectInstance(), actualArgs);
						->
            NativeMethodAccessorImpl # invoke()
						->
						跳到 LogUtil 的 around() 方法, 开始执行around 的前置逻辑
						->
            // 一堆前置逻辑的执行.... 
          	// 通过反射的方式调用目标的方法，相当于执行method.invoke(), 可以自己修改结果值
            // 注意: pjp 对象中包含了 cglibMethodInvocation对象, 所以在进行调用的时候, 能够回到拦截器链中继续向下执行
            // 顺序是根据前面的 sort advice 的顺序
            result = pjp.proceed(args);
            ->
   					MethodInvocationProceedingJoinPoint # proceed()
            return this.methodInvocation.invocableClone(arguments).proceed();// mi.proceed()  CglibMethodInvocation
            ->
            又跳转到 CglibMethodInvocation # proceed()
            super.proceed
->
  获取 5号位置的元素 before
      在实际的业务逻辑处理之前, 先把核心的before中的逻辑执行了.
      		MethodBeforeAdviceInterceptor # invoke()
            // 这个invoke方法是拦截器的回调方法，会在代理对应的方法被调用时触发回调
            @Override
              public Object invoke(MethodInvocation mi) throws Throwable {
              // 执行前置通知的方法
              this.advice.before(mi.getMethod(), mi.getArguments(), mi.getThis());
              // 执行下一个通知/拦截器，但是该拦截器是最后一个了，所以会调用目标方法
              return mi.proceed();
            }
          ->
          AspectJMethodBeforeAdvice # before()
            @Override
            public void before(Method method, Object[] args, @Nullable Object target) throws Throwable {
              // 这里传进来的目标对象、目标参数、目标方法都没有用到
              invokeAdviceMethod(getJoinPointMatch(), null, null);
            }
					->
          return invokeAdviceMethodWithGivenArgs(argBinding(getJoinPoint(), jpMatch, returnValue, ex));
					->
            ... 一系列 invoke
          ->
          实际的before的消息通知
->
   没有六号元素了, 六个元素下标是 0-5
   回到 ReflectiveMethodInvocation # proceed()
   此时链上所有的拦截器都已经执行完毕, 该执行具体的方法逻辑了
       // 从索引为-1的拦截器开始调用，并按序递增，如果拦截器链中的拦截器迭代调用完毕，开始调用target的函数，这个函数是通过反射机制完成的
       // 具体实现在AopUtils.invokeJoinpointUsingReflection方法中
       if (this.currentInterceptorIndex == this.interceptorsAndDynamicMethodMatchers.size() - 1) {
          return invokeJoinpoint();
       }
	 经过一系列的invoke方法的执行, 调用到业务方法中, 然后开始返回
->
   回到before方法
->
   回到around 的流程中
   执行 around 的后置逻辑操作
->
   返回到after, 执行完成后 执行
            

      
注意:
    虽然配置了具体的advice的顺序, 但是根据实际的配置情况, 顺序可能有所不同, 所以要根据业务代码来判断优先执行哪个
      
      
      
      
ExposeInvocationInterceptor
	private static final ThreadLocal<MethodInvocation> invocation = new NamedThreadLocal<>("Current AOP method invocation");        

	// 此处是继续调用ReflectiveMethodInvocation的proceed方法来进行递归调用
	public static MethodInvocation currentInvocation() throws IllegalStateException {
		MethodInvocation mi = invocation.get();
		if (mi == null) {
			throw new IllegalStateException();
		}
		return mi;
	}
			AspectJAfterAdvice
			invokeAdviceMethod(getJoinPointMatch(), null, null);
			AspectJAfterReturningAdvice
			invokeAdviceMethod(getJoinPointMatch(), returnValue, null);
			AspectJAfterThrowingAdvice
			invokeAdviceMethod(getJoinPointMatch(), null, ex);
			AspectJMethodBeforeAdvice
			invokeAdviceMethod(getJoinPointMatch(), null, null);
			->
      getJoinPointMatch() // 获取mi元素. mi是伴随着整个 链的调用过程
      // 贼多注释. 注意看 !!!!!
      MethodInvocation mi = ExposeInvocationInterceptor.currentInvocation();
        
```





# 声明式事务讲解



## 配置文件的加载 和 对象创建

```java



1. 配置文件的加载 和 准备 beanFactory -----------------------------------------------------------------------------------
  
需要哪些具体的对象?  这五个对象必须要有
  advice, AspectJAwareAdvisorAutoProxyCreator
	advisor, DefaultBeanFactoryPointcutAdvisor
	methodInterceptor, TransactionInterceptor, 包含 NameMatchTransactionAttributeSource
	pointcut, AspectJExpressionPointcut

以配置文件开始使用事务 tx.xml, 测试类 TxTest

loadBeanDefinitions() 加载配置文件 --> 对象的属性 --> 对象之间的包含关系

在aop里面:
advisor 里面 -> advise ->
包含
1. method
2. 表达式
3. singletonFactory

在 parseBeanDefinitions() 方法中, 解析
[context: property-placeholder] 标签时会往容器中注入 beanFactoryPostProcessor 对象 --- PlaceholderConfigurerSupport
会在后面进行一个替换过程 $ , {} 替换为实际配置文件中的值

<bean>标签会使用 parseDefaultElement()
<aop>, <tx>, <context> 标签会使用 parseCustomElement()

spring.handlers 文件, 根据命名空间去找handler对象, 执行parse的解析

  
tx.xml <aop:config>标签, 解析时
跳转到 ConfigBeanDefinitionParser # parse() 方法
->
使用aop, 肯定会使用动态代理, Cglib/jdk, 最终会出现autoProxyCreator (需要提前准备好, 解析时也会出现)
AspectJAwareAdvisorAutoProxyCreator
// 注册自动代理模式创建器, AspectJAwareAdvisorAutoProxyCreator
configureAutoProxyCreator(parserContext, element);
->
AopNamespaceUtils.registerAspectJAutoProxyCreatorIfNecessary(parserContext, element);
->
BeanDefinition beanDefinition = AopConfigUtils.registerAspectJAutoProxyCreatorIfNecessary(
		parserContext.getRegistry(), parserContext.extractSource(sourceElement));
->
return registerOrEscalateApcAsRequired(AspectJAwareAdvisorAutoProxyCreator.class, registry, source);

注入 AspectJAwareAdvisorAutoProxyCreator 一个动态代理的自动创建器

继续parse方法, 
parsePointcut() -> 创建的是 AspectJExpressionPointcut.class 对象
parseAdvisor() -> DefaultBeanFactoryPointcutAdvisor.class
parseAspect() -> 

循环依赖的时候, 有一个 runtimeBeanReference
RuntimeBeanReference
RuntimeBeanNameReference
提前有一个引用, 方便后续属性依赖注入的时候, 可以把属性值注入进去
  
  
解析 <tx>标签的时候, 使用 TxNamespaceHandler, 在 spring-tx 包下的 spring.handlers文件中
AbstractBeanDefinitionParser # parse()
->
// 处理内部的内置对象
AbstractSingleBeanDefinitionParser # parseInternal()
	BeanDefinitionBuilder builder = BeanDefinitionBuilder.genericBeanDefinition();

  // tx 标签使用 TransactionInterceptor, 实现了 methodInterceptor 接口
  // 获取自定义标签中的class，此时会调用自定义解析器
  Class<?> beanClass = getBeanClass(element);
	其子类实现 TxAdviceBeanDefinitionParser # getBeanClass()
	protected Class<?> getBeanClass(Element element) {
		return TransactionInterceptor.class;
	}
->
TxAdviceBeanDefinitionParser # doParse();
->
// 获取attribute子元素
RootBeanDefinition attributeSourceDefinition = parseAttributeSource(attributeSourceElement, parserContext);
在这个方法中
  RootBeanDefinition attributeSourceDefinition = new RootBeanDefinition(NameMatchTransactionAttributeSource.class);
添加了一个 NameMatchTransactionAttributeSource 元素
  
  
  
  -----------------
  
调试的时候 propertySourcePlaceHolderConfigure 出现了两次
  DropFrame 按钮(调试的时候用) 会创建两个相同的对象
  
2. BFPP的调用 -----------------------------------------------------------------------------------------------------
  
invokeBeanFactoryPostProcessor()
取出 PropertySourcesPlaceholderConfigurer 完成 ${jdbc.username} 等的替换
  
3. registerBPP -----------------------------------------------------------------------------------------------------
  
会创建 internalAutoProxyCreator
指向 AspectJAwareAdvisorAutoProxyCreator 对象
  
4.finishBeanFactoryInitialization() ---------------------------------------------------------------------------------
  
配置AOP需要哪些具体的对象?? 必须要看到这个五个对象的创建
1. advice
2. advisor  ---  DefaultBeanFactoryPointcutAdvisor
3. methodInterceptor  ---  TransactionInterceptor  ---  包含 NameMatchTransactionAttributeSource
4. pointcut  ---  AspectJExpressionPointcut
5. AspectJAwareAdvisorAutoProxyCreator


getBean -> doGetBean -> createBean (包含五种创建对象的方式) -> doCreateBean

1. 自定义BPP, 生成代理对象 instantiationAwareBeanPostProcessor
2. 通过反射创建对象
3. 通过factoryMethod创建对象
4. 通过factoryBean创建对象
5. 通过supplier创建对象


createBean() // dataSource
->
resolveBeforeInstantiation()
->
applyBeanPostProcessorsBeforeInstantiation()
->
AbstractAutoProxyCreator # postProcessBeforeInstantiation()
->
shouldSkip(), 这个方法中会进行advisor对象的创建!!!
->
AspectJAwareAdvisorAutoProxyCreator # shouldSkip()
findCandidateAdvisors 获取候选的advisor对象
->
findAdvisorBeans()
			// 获取当前BeanFactory中所有实现了Advisor接口的bean的名称
  		// 这里只能取到一个 DefaultBeanFactoryPointcutAdvisor 对象, 是需要创建的五个对象之一
			advisorNames = BeanFactoryUtils.beanNamesForTypeIncludingAncestors(
					this.beanFactory, Advisor.class, true, false);
			this.cachedAdvisorBeanNames = advisorNames;

			// 将当前bean添加到结果中, 这里包含 DefaultBeanFactoryPointcutAdvisor 的创建过程
			// 在 populateBean()中填充属性, pvs 两个值: 
			// adviceBeanName 指向tx标签myAdvice对象
			// 		applyPropertyValues() -> resolveValueIfNecessary() -> RuntimeBeanNameReference 暂未创建
			// pointcut 指向txPoint表达式对象 具体是AspectJExpressionPointcut对象
			// 		applyPropertyValues() -> resolveValueIfNecessary() -> RuntimeBeanReference 直接创建
			// 在 initializeBean() -> applyBeanPostProcessorsAfterInitialization() 中会进行处理吗?
			// 		AbstractAutoProxyCreator # postProcessAfterInitialization() -> wrapIfNecessary ()
			// 		if (Boolean.FALSE.equals(this.advisedBeans.get(cacheKey))) 在这里停掉了
			//    意味着不需要动态代理对象的创建 直接返回
 		  advisors.add(this.beanFactory.getBean(name, Advisor.class));

		
			beanFactory 是通过什么来填充的? 
      		通过 beanFactoryAware 接口
        
      // 整理
      // 创建 dataSource的时候把 DefaultBeanFactoryPointcutAdvisor 和 AspectJExpressionPointcut 创建好了
      // 但是 TransactionInterceptor 和 NameMatchTransactionAttributeSource 还没有创建好
        
      // 根据表达式, 在tx包下是都需要进行动态代理的
      // 只有bookService 和 bookDao是需要进行动态代理
        
      preInstantiateSingletons()中 循环到myAdvice, 之前没创建的advice 对象. 
      此时debug发现已经有值且指向 TransactionInterceptor
      在哪里创建的呢?
        不管创建 advice 还是 advisor, 都是给aop使用的, 在创建具体的动态代理对象的时候需要去调用 这些advice 和 advisor对象. 完成创建
        视频1h58min
        
        
     	// 获取当前bean的Advices和Advisors
			Object[] specificInterceptors = getAdvicesAndAdvisorsForBean(bean.getClass(), beanName, null);

			注意 一些 advice 是 实现了 methodInterceptor接口, 所以可以认为一些 methodInterceptor接口的子类 是 advice对象
        或者通过适配器, 对方法进行拦截
        
      bookService一开始是普通对象, 后面替换成代理对象, 过程:
			initializeBean()
      ->
      applyBeanPostProcessorsAfterInitialization
      ->
      AbstractAutoProxyCreator # postProcessAfterInitialization()
      ->
      wrapIfNecessary
      这里会分情况 返回 普通对象和代理对象, 完成替换
        
      在 getEarlyBeanReference() 也是调用的 wrapIfNecessary()

```







## spring 注解配置的声明式事务处理

```java


注意使用注解和使用xml的时候某些对象的属性不同
  
  // 测试类
  public class TransactionTest {
    public static void main(String[] args) {
      	// 此时父类已经 new DefaultListableBeanFactory();
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext();
      	// 注册配置类 TransactionConfig
        applicationContext.register(TransactionConfig.class);
        applicationContext.refresh(); // 此时没有进行类上的注解扫描, 进行扫描需要配置 ConfigurationClassPostProcessor
        BookService bean = applicationContext.getBean(BookService.class);
        bean.checkout("zhangsan",1);
    }
	}

  AnnotationConfigApplicationContext 构造方法:
	public AnnotationConfigApplicationContext() {
		this.reader = new AnnotatedBeanDefinitionReader(this);
		this.scanner = new ClassPathBeanDefinitionScanner(this);
	}
	
	// 创建容器对象：DefaultListableBeanFactory
	// 加载xml配置文件的属性值到当前工厂中，最重要的就是BeanDefinition
	refresh2: ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();
	->
  // 初始化BeanFactory,并进行XML文件读取，并将得到的BeanFactory记录在当前实体的属性中
  refreshBeanFactory();
	注意: 使用注解的时候, 实现类是 GenericApplicationContext # refreshBeanFactory()
    
  invokeBFPP 阶段:
  AnnotationConfigUtils # CONFIGURATION_ANNOTATION_PROCESSOR_BEAN_NAME = internalConfigurationAnnotationProcessor 
  -> 注入 ConfigurationClassPostProcessor
    
  ConfigurationClassPostProcessor # postProcessBeanDefinitionRegistry() # processConfigBeanDefinitions()
  解析注解扫描的类, 主要解析 transactionConfig !!!!!!!!!!!!!!!!!!!!!!!!!!1
  
  注意, 虽然 TransactionConfig 没有直接使用 @import 注解, 但是在 @EnableTransactionManagement 注解中包含 		
  @Import(TransactionManagementConfigurationSelector.class) 注解
  TransactionManagementConfigurationSelector 选择器, 注入 AutoProxyRegistrar / ProxyTransactionManagementConfiguration
  // 处理@Import注解
  processImports(configClass, sourceClass, getImports(sourceClass), filter, true);

	registerBPP 阶段:
	// 注册内部管理的用于处理@Autowired，@Value,@Inject以及@Lookup注解的后置处理器bean
	internalAutowiredAnnotationProcessor --- AutowiredAnnotationBeanPostProcessor
  // 注册内部管理的用于处理JSR-250注解，例如@Resource,@PostConstruct,@PreDestroy的后置处理器bean
  internalCommonAnnotationProcessor --- CommonAnnotationBeanPostProcessor
  internalAutoProxyCreator --- 
    之前指代的是 AspectJAwareAdvisorAutoProxyCreator 现在是 InfrastructureAdvisorAutoProxyCreator
    都继承自 AbstractAutoProxyCreator
   	只有实现对象不一样而已
  
  实例化阶段:
  要实例化的对象:
  	AspectJExpressionPointcut
    DefaultBeanFactoryPointcutAdvisor
    TransactionInterceptor
    NameMatchTransactionAttributeSource
  
  // 在 TransactionTest register transactionConfig 的时候 是普通对象, 不是代理对象
  // 在 invokeBFPP的时候变成代理对象 ConfigurationClassPostProcessor implements BeanDefinitionRegistryPostProcessor
  // 在 ppPDR没处理, 观察后面ppBF
  preInstantiateSingletons() beanNames.for 创建对象到 transactionConfig
  ->
  createBean 中
      
  // 锁定class，根据设置的class属性或者根据className来解析class
  Class<?> resolvedClass = resolveBeanClass(mbd, beanName);

  // 给BeanPostProcessors一个机会来返回代理来替代真正的实例，应用实例化前的前置处理器,用户自定义动态代理的方式，针对于当前的被代理类需要经过标准的代理流程来创建对象
  Object bean = resolveBeforeInstantiation(beanName, mbdToUse);
	->
	AbstractAutoProxyCreator # postProcessBeforeInstantiation
  ->
  doCreateBean
    
    
	为什么 transactionConfig 对象要生成代理对象? 
  @EnableTransactionManagement 开启事务, 把一堆bean对象(不需要动态代理支撑)定义到配置类, 所以和@Bean无关
  
  <某回答>获取@Bean对象时都从这个代理获取, 从而转到beanFactory中获取, 保证对象单例
    
  在此处将 transactionConfig 的普通对象替换成代理对象
  ConfigurationClassPostProcessor # postProcessBeanFactory()
  ->
  enhanceConfigurationClasses(beanFactory);
	->
  注意注释
  If a @Configuration class gets proxied, always proxy the target class
  只要配置了 @Configuration 注解, 总是会生成代理
    
  为什么需要加代理? 代理的作用在什么地方?spring为什么 @Configuration 修饰的类要使用代理?
    <<<查看下图>>>
  
  如何判断需不需要进行动态代理??
    enhanceConfigurationClasses(beanFactory); 方法中
    
  注意在 ConfigurationClassPostProcessor # processConfigBeanDefinitions() 方法中:
  // 判断当前BeanDefinition是否是一个配置类，并为BeanDefinition设置属性为lite或者full，此处设置属性值是为了后续进行调用
  // 如果Configuration配置proxyBeanMethods代理为true则为full
  // 如果加了@Bean、@Component、@ComponentScan、@Import、@ImportResource注解，则设置为lite
  // 如果配置类上被@Order注解标注，则设置BeanDefinition的order属性值
```



![image-20220725214507957](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0fvouoj212i0asmyn.jpg)

![image-20220725220254453](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0hsc24j20o60icjti.jpg)









```java
七种传播特性:
	事务方法嵌套事务方法的时候, 如何使用事务

如果外层方法中包含事务, 那么内层方法是否需要支持当前事务:
    1.支持外层事务
    		1.1 required 使用当前事务, 如果没有, 创建一个
    		1.2 supports 如果有, 就使用当前事务, 如果没有, 那么就不用事务了
    		1.3 mandatory 使用当前的事务, 如果没有事务的话, 就抛异常
    2.不支持外层事务
      	2.1 required_new 如果存在事务, 将事务挂起(将原来状态做一个保存), 自己新建一个
      	2.2 not_supported 不需要事务, 如果存在, 就挂起它
      	2.3 never 不要事务, 如果存在事务, 就抛异常
    3.nested 如果存在事务, 会创建保存点; 不存在就新建事务
      
      
    
    自己设计事务系统, 如何操作?
      1. 创建或者获取一个基本的事务
      2. 执行事务操作, 执行sql操作
      3. 是否有异常?
      		3.1 没有异常, 
			 事务正常执行, 清除当前要执行的sql信息 <transactionInfo>
              commit 事务 <最终依托数据库事务> -> 数据库事务
      		3.2 有异常
              事务异常执行, 清除事务信息 oldTransactionInfo -> 回滚事务 -> 数据库事务
              恢复之前的事务信息
                 
              数据库事务 -> 释放连接 / 关闭连接
                 
                 
	after 
	return 方式1. 和before组成 实际操作的前置或者后置
	afterReturning 在返回结果的时候进行正常 TransactionAspectSupport # commitTransactionAfterReturning()
	afterThrowing 事务回滚 completeTransactionAfterThrowing()
	around 方式2. 实际操作的前置或者后置

    可以选择n多种不同的方式进行操作, 哪种方式最简单?
    正常提交用around方式, 异常回滚选择afterThrowing的方式
    查看源码, 验证spring的事务处理是如何完成的?
                 
     整个事务处理过程中, 包含了几个advisor?
           1. ExposeInvocationInterceptor 为了方便责任链的调用
           2. DefaultBeanFactoryPointcutAdvisor : TransactionInterceptor <<<核心方法>>>
                 invoke()
                 invokeWithinTransaction()
               		1. 获取事务的属性信息 getTransactionAttributeSource()
               		2. 决定事务的管理器是哪个 getTransactionAttribute()
               		3. 创建事务 createTransactionIfNecessary()
               		4. 进行具体方法的调用
               			// 执行被增强方法,调用具体的处理逻辑
					   retVal = invocation.proceedWithInvocation();
				   5. 异常回滚 completeTransactionAfterThrowing()
				   6. 清除事务信息 cleanupTransactionInfo()
                     7. 正常提交事务 commitTransactionAfterReturning()
              
                整体分为三部分:
					1. 准备事务处理相关的对象: 事务对象, 连接器, 事务信息, 事务状态, 事务属性
                      2. 执行
                      3. 是否有异常?
                       		3.1 有 回滚操作
                        	3.2 没有 正常提交
                        
                正常操作的时候是存在事务方法嵌套事务方法的, 此时怎么处理?
                由不同的传播特性来决定不同方法的事务应该如何获取.
                        
```





![image-20220725222013880](https://tva1.sinaimg.cn/large/e6c9d24ely1h57k0iqij5j218y0hc42g.jpg)





## spring 声明式事务的运行流程



```java

tx.xml
    <tx:advice id="myAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="checkout" propagation="REQUIRED" />
            <tx:method name="updateStock" propagation="REQUIRES_NEW" />
        </tx:attributes>
    </tx:advice>

TxTest
bookService.checkout("zhangsan",1); 跳到哪?
->
CglibAopProxy # DynamicAdvisedInterceptor # intercept()
// 一共只有两个
// 1. ExposeInvocationInterceptor 为了保证拦截器链能够正常的执行
// 2. TransactionInterceptor <<<核心处理逻辑>>>
List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);
// 通过cglibMethodInvocation来启动advice通知
retVal = new CglibMethodInvocation(proxy, target, method, args, targetClass, chain, methodProxy).proceed();

先执行 ExposeInvocationInterceptor
    其中 invocation 属性是 一个 ThreadLocal 的变量, 因此需要把刚才创建的对象放进去. this 指的是CglibMethodInvocation
    private static final ThreadLocal<MethodInvocation> invocation =
			new NamedThreadLocal<>("Current AOP method invocation");
再执行 mi.proceed(); 取到 TransactionInterceptor
    ->
    invokeWithinTransaction()
     		整体分为三部分:
                1. 准备事务处理相关的对象: 事务对象, 连接器, 事务信息, 事务状态, 事务属性
                2. 执行
                3. 是否有异常?
                    3.1 有 回滚操作
                    3.2 没有 正常提交
    ->
    // 获取我们的事务属性源对象
	TransactionAttributeSource tas = getTransactionAttributeSource();
		注意, 此时调用的是 checkout() 方法, 有没有被事务修饰?
	       <tx:advice id="myAdvice" transaction-manager="transactionManager">
            	<tx:attributes>
                	<tx:method name="checkout" propagation="REQUIRED" />
            		<tx:method name="updateStock" propagation="REQUIRES_NEW" />
            	</tx:attributes>
            </tx:advice>
            解析出两个 TransactionAttributeSource 对象
                
    // 通过事务属性源对象获取到当前方法的事务属性信息
	final TransactionAttribute txAttr = (tas != null ? tas.getTransactionAttribute(method, targetClass) : null);

	// 获取我们配置的事务管理器对象
	final TransactionManager tm = determineTransactionManager(txAttr);

	createTransactionIfNecessary: --------------------------------------------------------

	// 创建TransactionInfo
	TransactionInfo txInfo = createTransactionIfNecessary(ptm, txAttr, joinpointIdentification);
	
  public interface TransactionStatus extends TransactionExecution, SavepointManager, Flushable {
    // 是否有保存点. 什么是保存点? 与事务的回滚操作有关, 只回滚到某一个具体的操作<指定的位置>
    boolean hasSavepoint();
    // 将会话刷新到数据库中
    @Override
    void flush();
  }

	public interface TransactionExecution {
    // 是否为新事务
    boolean isNewTransaction();
    // 设置为只回滚
    void setRollbackOnly();
    // 是否为只回滚
    boolean isRollbackOnly();
    // 当前事务是否已经完成
    boolean isCompleted();
  }

  // 获取TransactionStatus事务状态信息
  TransactionStatus status = tm.getTransaction(txAttr);
	AbstractPlatformTransactionManager # getTransaction()
    
  // 获取事务
  Object transaction = doGetTransaction(); -----------------------------------------------
	DataSourceTransactionManager # doGetTransaction()
    
  // 创建一个数据源事务对象
  DataSourceTransactionObject txObject = new DataSourceTransactionObject();
	
	// 数据源事务对象，代表一个连接持有器，用作事务管理器的事务对象
	private static class DataSourceTransactionObject extends JdbcTransactionObjectSupport {
		// 新链接的包装器
		private boolean newConnectionHolder;
		// 是否必须同意自动提交
		private boolean mustRestoreAutoCommit;
  }

	public abstract class JdbcTransactionObjectSupport implements SavepointManager, SmartTransactionObject {
    // 链接的包装器
    @Nullable
    private ConnectionHolder connectionHolder;
    // 之前设置的隔离级别
    @Nullable
    private Integer previousIsolationLevel;
    // 是否是只读
    private boolean readOnly = false;
    // 是否允许设置保存点
    private boolean savepointAllowed = false;
  }

  // 是否允许当前事务设置保持点
  // Nested 表示嵌套的隔离级别
  // 查看在哪里进行设置 setNestedTransactionAllowed 在 DataSourceTransactionManager() 中调用
	// 也就是说在实例化 DataSourceTransactionManager 的时候将 nestedTransactionAllowed 设置为 true 
  txObject.setSavepointAllowed(isNestedTransactionAllowed());
	
  /**
    * TransactionSynchronizationManager 事务同步管理器对象(该类中都是局部线程变量)
    * 用来保存当前事务的信息,我们第一次从这里去线程变量中获取 事务连接持有器对象 通过数据源为key去获取
    * 由于第一次进来开始事务 我们的事务同步管理器中没有被存放.所以此时获取出来的conHolder为null
    */
	// ConnectionHolder 跟每个线程相关的包装器
  ConnectionHolder conHolder = 
    (ConnectionHolder) TransactionSynchronizationManager.getResource(obtainDataSource());
	// 管理每个线程的资源和事务同步的中央委托。
	// 内部一堆 ThreadLocal 的属性, 变量副本, 只跟线程相关
	// 之前在<bean>标签中 transactionmanager 中设置过 dataSource 属性
	TransactionSynchronizationManager
  ->
  Object actualKey = TransactionSynchronizationUtils.unwrapResourceIfNecessary(key);
	Object value = doGetResource(actualKey);
	TransactionSynchronizationManager # doGetResource()
  如果 map != null, 直接从map中获取数据源
    
  // 非新创建连接则写false
  txObject.setConnectionHolder(conHolder, false);
	// 返回事务对象
	return txObject;
	
	到这里就设置了一个属性值: savepointAllowed = true 允许进行事务保存点的设置
  接下来 isExistingTransaction() 判断仍然没有事务
	doGetTransaction() -----------------------------------------------
	
  	 // PROPAGATION_REQUIRED，PROPAGATION_REQUIRES_NEW，PROPAGATION_NESTED都需要新建事务
     else if (def.getPropagationBehavior() == TransactionDefinition.PROPAGATION_REQUIRED ||
             def.getPropagationBehavior() == TransactionDefinition.PROPAGATION_REQUIRES_NEW ||
             def.getPropagationBehavior() == TransactionDefinition.PROPAGATION_NESTED) {
      //没有当前事务的话，REQUIRED，REQUIRES_NEW，NESTED挂起的是空事务，然后创建一个新事务
      SuspendedResourcesHolder suspendedResources = suspend(null);
      if (debugEnabled) {
        logger.debug("Creating new transaction with name [" + def.getName() + "]: " + def);
      }
      try {
        return startTransaction(def, transaction, debugEnabled, suspendedResources);
      }
      catch (RuntimeException | Error ex) {
        // 恢复挂起的事务
        resume(null, suspendedResources);
        throw ex;
      }
     ->
     private TransactionStatus startTransaction(TransactionDefinition definition, 
                                                  Object transaction,
                                                  boolean debugEnabled, 
                                                  @Nullable SuspendedResourcesHolder suspendedResources) {
       // 是否需要新同步
       // transactionSynchronization = SYNCHRONIZATION_ALWAYS = 0 始终激活事务同步，即使是“空”事务
       // SYNCHRONIZATION_NEVER = 2 永远不要进行活动事务同步，即使对于实际的事务也是如此。
       boolean newSynchronization = (getTransactionSynchronization() != SYNCHRONIZATION_NEVER);
       // 创建新的事务. 注意, 到这一步里还没有链接
       DefaultTransactionStatus status = newTransactionStatus(
         definition, transaction, true, newSynchronization, debugEnabled, suspendedResources);
       // 开启事务和连接
       doBegin(transaction, definition);
       // 新同步事务的设置，针对于当前线程的设置
       // 对于 TransactionSynchronizationManager 中其他的 ThreadLocal 变量进行设置, 主要是除了 resources 和 synchronization 的后四个
       prepareSynchronization(status, definition);
       return status;
     }
       
  		DataSourceTransactionManager # doBegin() ---------------------------------
      // 通过数据源获取一个数据库连接对象
			Connection newCon = obtainDataSource().getConnection();
      DruidDataSource # getConnection()
        
      // 关闭自动提交
      if (con.getAutoCommit()) {
        //设置需要恢复自动提交
        txObject.setMustRestoreAutoCommit(true);
        if (logger.isDebugEnabled()) {
          logger.debug("Switching JDBC Connection [" + con + "] to manual commit");
        }
        // 关闭自动提交
        // 之后所有操作的提交 / 回滚 由 spring 提供的事务的提交和回滚机制来管理
        con.setAutoCommit(false);
      }
       
      // 判断事务是否需要设置为只读事务
			prepareTransactionalConnection(con, definition);
       
     	// 绑定我们的数据源和连接到我们的同步管理器上，把数据源作为key,数据库连接作为value 设置到线程变量中
			if (txObject.isNewConnectionHolder()) {
				// 将当前获取到的连接绑定到当前线程
				TransactionSynchronizationManager.bindResource(obtainDataSource(), txObject.getConnectionHolder());
			}
       
      // 线程私有事务资源, 和线程绑定
			private static final ThreadLocal<Map<Object, Object>> resources = new NamedThreadLocal<>("Transactional resources");
       
       public static void bindResource(Object key, Object value) throws IllegalStateException {
         Object actualKey = TransactionSynchronizationUtils.unwrapResourceIfNecessary(key);
         Assert.notNull(value, "Value must not be null");
         Map<Object, Object> map = resources.get();
         if (map == null) {
           map = new HashMap<>();
           resources.set(map);
         }
         //每次在进行获取的时候都要根据obtainDataSource()返回的数据源来获取connectionHolder,现在经过设置会后，有了
         Object oldValue = map.put(actualKey, value);
       }
       
       解释:
       methodA required, methodB required
         第一次进来的时候, 没有任何事务及连接的相关信息, 所以获取不到,
       	 当调用 methodB 的时候, 能否从 resources 获取到对应的 connectionHolder? 能
         如果获取到了, 就省略创建连接的时间,
       	 而且注意, required 本身就是可以直接使用外层方法所对应的事务.
       
      DataSourceTransactionManager # doBegin() ---------------------------------
 
           
    回到 createTransactionIfNecessary()
    // 根据指定的属性与status准备一个TransactionInfo，
		return prepareTransactionInfo(tm, txAttr, joinpointIdentification, status);
    TransactionInfo 事务信息
    ->
    // 事务信息绑定到当前线程
		txInfo.bindToThread();
    
    TransactionAspectSupport # 
    private void bindToThread() {
      // 这里保留了一个老状态
			this.oldTransactionInfo = transactionInfoHolder.get();
			transactionInfoHolder.set(this);
		}

		private void restoreThreadLocalStatus() {
      // 设置老状态
			transactionInfoHolder.set(this.oldTransactionInfo);
		}
       
    老状态 explain:
       如果嵌套事务是 required_new, 需要新建事务, 同时也需要保留老的状态
       or 进程在切换到时候需要保留状态 / 才能恢复进程
    

	createTransactionIfNecessary --------------------------------------------------------
	
  返回到 createTransactionIfNecessary(), 跟事务相关的信息已经准备好
  ->
  // 执行被增强方法
	retVal = invocation.proceedWithInvocation();
  ->
  跳转到 CglibAopProxy # proceed()
  ->
  ReflectiveMethodInvocation # proceed()
  ->
  invokeJoinpoint();
  ->
  CglibAopProxy # invokeJoinpoint
  ->
  BookService # checkout()
  其中 bookDao也是也个动态代理类, 需要进行 aop 的切面, 和上面的逻辑一样
  点进去 回到 CglibAopProxy # DynamicAdvisedInterceptor # intercept()
         
	当sql执行完之后, db 的值不变 (因为事务还没有提交)
    
  同的是同一个数据库链接, 如何控制提交哪个sql语句, 不提交哪个sql语句?
  保证事务一致性, 多个sql 最后才提交事务

  // 提交事务，就算没有异常，但是提交的时候也可能会回滚，因为有内层事务可能会标记回滚。所以这里先判断是否状态是否需要本地回滚，
	// 也就是设置回滚标记为全局回滚，不会进行回滚，再判断是否需要全局回滚，就是真的执行回滚。但是这里如果是发现有全局回滚，还要进行提交，就会报异常
  AbstractPlatformTransactionManager # commit()
  // 处理事务提交
	processCommit(defStatus);
	->
  // 处理提交，先处理保存点，然后处理新事务，如果不是新事务不会真正提交，要等外层是新事务的才提交，
	// 最后根据条件执行数据清除,线程的私有资源解绑，重置连接自动提交，隔离级别，是否只读，释放连接，恢复挂起事务等
  AbstractPlatformTransactionManager # processCommit()
    
    		// 有保存点
				if (status.hasSavepoint()) {
					if (status.isDebug()) {
						logger.debug("Releasing transaction savepoint");
					}
					//是否有全局回滚标记
					unexpectedRollback = status.isGlobalRollbackOnly();
					// 如果存在保存点则清除保存点信息
					status.releaseHeldSavepoint();
				}
				//当前状态是新事务
				else if (status.isNewTransaction()) {
					if (status.isDebug()) {
						logger.debug("Initiating transaction commit");
					}
					unexpectedRollback = status.isGlobalRollbackOnly();
					// 如果是独立的事务则直接提交
					doCommit(status);
				}
				else if (isFailEarlyOnGlobalRollbackOnly()) {
					unexpectedRollback = status.isGlobalRollbackOnly();
				}
       
       	// 提交后清除线程私有同步状态
				triggerAfterCompletion(status, TransactionSynchronization.STATUS_COMMITTED);

```



## 声明式事务传播特性解析

```java

测试案例1: 
	  <tx:advice id="myAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="checkout" propagation="REQUIRED" />
            <tx:method name="updateStock" propagation="REQUIRED" />
        </tx:attributes>
    </tx:advice>
	bookService 和 bookDao 都是 required
  其中 bookDao 当中抛出异常, bookService 使用 try catch 捕获异常
  启动报错: transaction rolled back because it has been marked as rollback-only
  
  两个都是 REQUIRED, 使用的是同一个事务, 在外层方法进行一个提交
    
    
测试案例1运行流程:

  因为 bookService 和 bookDao 都进行了代理, 所以从 checkout() 进入到 updateStock() 的时候, 
	先到 invokeJoinPoint() 执行 checkout(), 
	回到 CglibAopProxy # DynamicAdvisedInterceptor # intercept(), 重新走一遍
    
    走到 doGetTransaction()的时候, 从getResource()当中threadlocal的变量map获取外层事务
		此时已经存在事务, 走 handleExistingTransaction() 处理已经存在的事务
    // 此时内层事务, newTransaction 属性返回false.
    return prepareTransactionStatus(definition, transaction, false, newSynchronization, debugEnabled, null);
  	
		走到内层异常, 抛出 completeTransactionAfterThrowing()
		->
    // 进行回滚
		txInfo.getTransactionManager().rollback(txInfo.getTransactionStatus());
												    checkout()       updateStock
		newTransaction     			true						 false
    newSynchronization			true						 true -> false 
                              		在newTransactionStatus()方法中, 传入 newTransaction false, newSynchronization true
                              		将 newSynchronization 按逻辑设置为 false
                              
    内层方法的回滚, 只是设置了一个回滚标记

		//成功后提交，会进行资源储量，连接释放，恢复挂起事务等操作
		commitTransactionAfterReturning(txInfo);
		->
    AbstractPlatformTransactionManager # commit()
    ->
    unexpected这个一般是false，除非是设置rollback-only=true，才是true，表示是全局的回滚标记。首先会进行回滚前回调，
	 	然后判断是否设置了保存点，比如NESTED会设置，要先回滚到保存点。如果状态是新的事务，那就进行回滚，如果不是新的，就设置一个回滚标记，
	 	内部是设置连接持有器回滚标记。然后回滚完成回调，根据事务状态信息，完成后数据清除,和线程的私有资源解绑，
	 	重置连接自动提交，隔离级别，是否只读，释放连接，恢复挂起事务等
    processRollback()
    
    // 回滚完成前回调
		triggerBeforeCompletion(status);

		// 进行回滚
		doRollback(status);

		// 回滚完成后回调
		triggerAfterCompletion(status, TransactionSynchronization.STATUS_ROLLED_BACK);

   	// 在这里抛异常
		if (unexpectedRollback) {
				throw new UnexpectedRollbackException(
						"Transaction rolled back because it has been marked as rollback-only");
     				测试案例一的异常: transaction rolled back because it has been marked as rollback-only
    }

----------------------------------------------------------------------------------------------
  
测试案例2: 
	修改bookDao中方法为 <tx:method name="updateStock" propagation="NESTED" />
  数据库中的数据不会变 !!!

	测试案例2运行流程: 
		getTransaction()
    ->
    handleExistingTransaction()
    ->
    // 嵌套事务
    if (definition.getPropagationBehavior() == TransactionDefinition.PROPAGATION_NESTED) {
    	// 不允许就报异常
      // 注意 该参数在 doGetTransaction() 设置保存点的时候设置过
			if (!isNestedTransactionAllowed()) {
				throw new NestedTransactionNotSupportedException(
						"Transaction manager does not allow nested transactions by default - " +
						"specify 'nestedTransactionAllowed' property with value 'true'");
			}
    }

		// 如果没有可以使用保存点的方式控制事务回滚，那么在嵌入式事务的建立初始简历保存点
		// 注意 boolean newTransaction, boolean newSynchronization 都等于 false
		DefaultTransactionStatus status = 
      		prepareTransactionStatus(definition, transaction, false, false, debugEnabled, null);

		// 为事务设置一个回退点
		// 注意status 中的属性 savepoint = null
		status.createAndHoldSavepoint();
		->
    setSavepoint(getSavepointManager().createSavepoint());
		->
    JdbcTransactionObjectSupport # createSavepoint()
    ->
    跳到 mysql jdbc save point 里面了, 不再是spring的东西
      
    注意: 在整个 NESTED 新链接中, 有创建新链接吗? 没有, 用的外层方法一样的事务
    
    内层方法抛异常后, 观察此方法的处理:
		//成功后提交，会进行资源储量，连接释放，恢复挂起事务等操作
		TransactionAspectSupport invokeWithinTransaction() # commitTransactionAfterReturning(txInfo);
		->
    txInfo.getTransactionManager().rollback(txInfo.getTransactionStatus());
		->
    processRollback(defStatus, false);
		->
				// 有保存点回滚到保存点
				if (status.hasSavepoint()) {
					if (status.isDebug()) {
						logger.debug("Rolling back transaction to savepoint");
					}
          // 回滚到保存点, 可以点进去细看 !!!
          // 在这里进行实际的回滚操作
					status.rollbackToHeldSavepoint();
				}
		->
    JdbcTransactionObjectSupport # rollbackToSavepoint()
      // 回滚到保存点
			conHolder.getConnection().rollback((Savepoint) savepoint);
			// 重置回滚标记，不需要回滚
			// 这里会将 this.rollbackOnly = false;
			conHolder.resetRollbackOnly();

			这个时候 回滚标志被置为 false, 保存点清空
      返回到外层事务的时候, 正常提交就行.
        
      NESTED 和 REQUIRED 都使用的同一个链接
         - REQUIRED 会在内层有异常的时候做一个回滚标记, 哪怕捕获了也没有用
         - 但是对于 NESTED 而言, 会创建一个保存点, 并且把当前事务回滚到保存点里去. 
        						之后爱提交提交, 爱回滚回滚, 对内层事务无影响
			
    
----------------------------------------------------------------------------------------------
   
   测试案例3:
			把外层的 try-catch 移到 内层会有什么效果?
      就相当于没有异常, 把异常消化掉了, 并没有往外部抛
        
----------------------------------------------------------------------------------------------
        
        
    测试案例4: 将内层换为 required-new, 外层仍旧是 required. 内层catch 异常
      内层 required-new 进入 createTransactionIfNecessary:
			->
     	getTransaction() 
      -> 
      DataSourceTransactionManager # doGetTransaction() 获取到外层的链接对象
      ->
      存在外层事务
      handleExistingTransaction()
        
    /**
		 * 当前的事务属性状态是PROPAGATION_REQUIRES_NEW表示需要新开启一个事务状态
		 */
		if (definition.getPropagationBehavior() == TransactionDefinition.PROPAGATION_REQUIRES_NEW) {
			if (debugEnabled) {
				logger.debug("Suspending current transaction, creating new transaction with name [" +
						definition.getName() + "]");
			}
			// 挂起当前事务并返回挂起的资源持有器
			SuspendedResourcesHolder suspendedResources = suspend(transaction);
			try {
				// 创建一个新的非事务状态(保存了上一个存在事务状态的属性)
				return startTransaction(definition, transaction, debugEnabled, suspendedResources);
			}
			catch (RuntimeException | Error beginEx) {
				resumeAfterBeginException(transaction, suspendedResources, beginEx);
				throw beginEx;
			}
		}

		注意 
      suspend()
      ->
      // 挂起的资源，连接持有器
      // 1. 接触 data source 和 connection holder 之间的关系
      // 2. 从 thread local 当中移除. 全部都清空
      // 3. 为了之后恢复, thread local 当中的属性都需要记录
			suspendedResources = doSuspend(transaction);
          DataSourceTransactionManager # doSuspend()
          @Override
          protected Object doSuspend(Object transaction) {
            DataSourceTransactionObject txObject = (DataSourceTransactionObject) transaction;
            // 清空连接持有器
            txObject.setConnectionHolder(null);
            // 解绑线程私有的资源
            return TransactionSynchronizationManager.unbindResource(obtainDataSource());
          }
          将线程私有的 thread local 变量置null
      ...      
      // 把上诉从线程变量中获取出来的存在事务属性封装为挂起的事务属性返回出去
			return new SuspendedResourcesHolder(
			 			suspendedResources, suspendedSynchronizations, name, readOnly, isolationLevel, wasActive);
      
			此时, 内层和外层事务都是相互独立的
      在提交回滚操作的时候不会相互影响
        
      TransactionAspectSupport invokeWithinTransaction() # commitTransactionAfterReturning(txInfo);
			->
      rollback
      ->
      1. 没有保存点
      2. 新链接, 是一个新事物
      ->
      doRollback
      ->
      此时事务已经完成回滚, 清空状态信息
      AbstractPlatformTransactionManager # cleanupAfterCompletion() # resume()
      // 结束之前事务的挂起状态
			resume(transaction, (SuspendedResourcesHolder) status.getSuspendedResources());
			->
      // 把挂起的事务和数据源重新绑定回去
      doResume(transaction, suspendedResources);


      ----------------------------------------------------------------------------------------------
        测试案例5: 内层不去捕获异常会怎样?
          TransactionAspectSupport invokeWithinTransaction() # 
          // 先回滚 回滚完了接着往上抛异常
          commitTransactionAfterReturning(txInfo, ex);
					throw ex;
					// 内层事务抛异常, 外层事务会受到影响.
					// 所以整体都回滚了. 整个过程中一共回滚了2次. 内层回滚影响外层回滚

			如果外层事务 required 出异常, 内层事务 required-new 是否会提交?  会. 外层不会影响内层
```





## 事务的创建提交与回滚流程 复习

```java
TransactionInterceptor # invoke()
// 事务的方式调用目标方法
// 在这埋了一个钩子函数 用来回调目标方法的
return invokeWithinTransaction(invocation.getMethod(), targetClass, invocation::proceed);
->
// 开启事务的处理逻辑
TransactionAspectSupport # invokeWithinTransaction()
  
// 获取我们的事务属性源对象
// getTransactionAttributeSource() 获取事务方法所对应配置的事务属性集合
TransactionAttributeSource tas = getTransactionAttributeSource();

// 通过事务属性源对象获取到当前方法的事务属性信息
// getTransactionAttribute() 获取某一个方法对应的具体的事务属性等相关信息
final TransactionAttribute txAttr = (tas != null ? tas.getTransactionAttribute(method, targetClass) : null);

// 获取我们配置的事务管理器对象
// 获取事务管理器
final TransactionManager tm = determineTransactionManager(txAttr);

// 创建TransactionInfo. 创建事务
TransactionInfo txInfo = createTransactionIfNecessary(ptm, txAttr, joinpointIdentification);

->
	// 如果没有名称指定则使用方法唯一标识，并使用DelegatingTransactionAttribute封装txAttr
  if (txAttr != null && txAttr.getName() == null) {
    // 封装到一个委托对象中
    txAttr = new DelegatingTransactionAttribute(txAttr) {
      @Override
      public String getName() {
        return joinpointIdentification;
      }
    };
  }
  
	TransactionStatus status = null;
	if (txAttr != null) {
  	if (tm != null) {
    // 获取TransactionStatus事务状态信息
    status = tm.getTransaction(txAttr);
  	}
  }
->
  AbstractPlatformTransactionManager # getTransaction() 获取事务

  // 如果没有事务定义信息则使用默认的事务管理器定义信息
  // 获取事务定义信息
	TransactionDefinition def = (definition != null ? definition : TransactionDefinition.withDefaults());

	// 获取事务
	Object transaction = doGetTransaction();

->
  // 创建一个DataSourceTransactionObject当作事务，设置是否允许保存点，然后获取连接持有器ConnectionHolder
	// 里面会存放JDBC的连接，设置给DataSourceTransactionObject, 当然第一次是空的
  DataSourceTransactionManager # doGetTransaction();

      protected Object doGetTransaction() {
        // 创建一个数据源事务对象
        // 对象中包含了保存点, 链接持有器等相关信息
        DataSourceTransactionObject txObject = new DataSourceTransactionObject();
        // 是否允许当前事务设置保持点
        txObject.setSavepointAllowed(isNestedTransactionAllowed());
        /**
         * TransactionSynchronizationManager 事务同步管理器对象(该类中都是局部线程变量)
         * 用来保存当前事务的信息,我们第一次从这里去线程变量中获取 事务连接持有器对象 通过数据源为key去获取
         * 由于第一次进来开始事务 我们的事务同步管理器中没有被存放.所以此时获取出来的conHolder为null
         */
        // TransactionSynchronizationManager.getResource() 通过此对象来获取连接持有器, 在第一次调用的时候, 对象一定为空
        // 后续创建完成之后会进行设置工作, 方便写一次获取
        ConnectionHolder conHolder =
            (ConnectionHolder) TransactionSynchronizationManager.getResource(obtainDataSource());
        // 非新创建连接则写false
        txObject.setConnectionHolder(conHolder, false);
        // 返回事务对象
        return txObject;
      }
->
  继续 AbstractPlatformTransactionManager # getTransaction() 获取事务
	
		// 判断当前线程是否存在事务，判断依据为当前线程记录的连接不为空且连接中的transactionActive属性不为空
		if (isExistingTransaction(transaction)) {
			// 当前线程已经存在事务
			return handleExistingTransaction(def, transaction, debugEnabled);
		}
		
		1.存在事务:
			// 当前线程已经存在事务
			return handleExistingTransaction(def, transaction, debugEnabled);
			->
      1.1 事务传播特性 == PROPAGATION_NEVER, 抛出异常
      /**
       * 判断当前的事务行为是不是PROPAGATION_NEVER的
       * 表示为不支持事务,但是当前又存在一个事务,所以抛出异常
       */
      if (definition.getPropagationBehavior() == TransactionDefinition.PROPAGATION_NEVER) {
        throw new IllegalTransactionStateException(
            "Existing transaction found for transaction marked with propagation 'never'");
      }

			1.2 事务传播特性 == PROPAGATION_NOT_SUPPORTED, 挂起当前事务, 以非事务状态开始运行
      1.3 事务传播特性 == PROPAGATION_REQUIRES_NEW, 挂起当前事务, 开启一个新的事务开始执行
      1.4 事务传播特性 == PROPAGATION_NESTED, 设置保存点, 以当前事物状态继续运行
      1.5 isValidateExistingTransaction() 验证当前存在的事务, 以当前的事务状态继续执行
				
        1.2 ~ 1.5最后也是返回到 TransactionAspectSupport # invokeWithinTransaction()
                // 执行被增强方法, 执行方法的具体处理逻辑
								retVal = invocation.proceedWithInvocation();

		2.不存在事务:
			// 事务超时设置验证, 超时直接抛异常
      if (def.getTimeout() < TransactionDefinition.TIMEOUT_DEFAULT) {
        throw new InvalidTimeoutException("Invalid transaction timeout", def.getTimeout());
      }
      // 如果当前线程不存在事务，但是PropagationBehavior却被声明为PROPAGATION_MANDATORY抛出异常
      if (def.getPropagationBehavior() == TransactionDefinition.PROPAGATION_MANDATORY) {
        throw new IllegalTransactionStateException(
          "No existing transaction found for transaction marked with propagation 'mandatory'");
      }
			判断传播特性是否是 REQUIRED / REQUIRES_NEW / NESTED
        	2.1是:
							挂起空事务
              开始事务
              return startTransaction(def, transaction, debugEnabled, suspendedResources);
							->
              // 开启事务和连接
							doBegin(transaction, definition);
							->
              DataSourceTransactionManager # doBegin()
                	1. 获取数据库连接
                	2. 设置事务属性
                	3. 设置自动提交关闭
              ->
              返回到 TransactionAspectSupport # createTransactionIfNecessary()
              // 根据指定的属性与status准备一个TransactionInfo，
              // 创建事务信息对象, 包含了事务处理过程中相关属性值
              return prepareTransactionInfo(tm, txAttr, joinpointIdentification, status);
							->
              返回到 TransactionAspectSupport # invokeWithinTransaction()
                // 执行被增强方法, 执行方法的具体处理逻辑
								retVal = invocation.proceedWithInvocation();
          
         	2.2不是:
							创建空的事务
                
                
----------------------------------------------------------------------------------------
返回到 TransactionAspectSupport # invokeWithinTransaction()
		// 执行被增强方法, 执行方法的具体处理逻辑
		retVal = invocation.proceedWithInvocation();
->
    try {
      // 执行被增强方法
      retVal = invocation.proceedWithInvocation();
    }
    catch (Throwable ex) {
      // 异常回滚
      // 完成事务的回滚操作
      completeTransactionAfterThrowing(txInfo, ex);
      throw ex;
    }
    finally {
      //清除事务信息，恢复线程私有的老的事务信息
      cleanupTransactionInfo(txInfo);
    }
    //成功后提交，会进行资源储量，连接释放，恢复挂起事务等操作
		// 以正常的方式提交事务
		commitTransactionAfterReturning(txInfo);

		// 后面是else 编程式事务, 基本用不到


status 状态
info 整个事务的状态信息
  
如果一个事务方法调用了另一个事务方法, 需要进行传播特性的判断. 因为传播特性会影响到事务的提交和回滚
    
```



# Spring 中常用的设计模式

```java
责任链: 使用AOP在进行通知调用的时候, 会使用责任链模式
工厂模式: beanFactory, 在 abstractAutoProxyCreator 中的 proxyFactory (用什么样的方式去生成具体的代理对象)
适配器模式: 通知的时候, 有adapter  adviceAdapter
代理模式: 使用cglib/jdk动态代理
模板方法: postProcessBeanFactory() / onRefresh() / initPropertySources()
观察者模式: 监听器/监听事件/广播器(多播器)
单例模式: spring中所有的bean对象默认都是单例的
原型模式: 可以通过作用域的方式来改变bean为prototype
策略模式: ClassPathXmlApplicationContext / FileSystemApplicationContext / XmlBeanDefinitionApplicationContext
  						/ PropertiesBeanDefinitionReader / 实例化策略(simple, cglib)
构建者模式: BeanDefinitionBuilder
访问者模式: BeanDefinitionVisitor
装饰者模式: BeanWrapper
委托者模式: delegate / BeanDefinitionParserDelegate
  
  
```











 
