# Spring

## IOC机制

### 最原始的tomcat+servlet的编码原理

服务通过tomcat启动后，tomcat会监听一个端口号的http请求，然后把请求转交给servlet,jsp配合处理请求

我们对一个业务逻辑处理时，如果直接使用new 创建对象，在很多地方使用。这个时候我们需要使用一个新的类来替换这个对象，在系统中就会涉及到很多地方的改动。改动过程中的成本很大，改动之后的测试成本也很大。耦合程度高。出现任何改动都会导致要大批量的改动代码

Spring IOC框架，控制反转，依赖注入

基于xml文件来进行配置，现在更多的基于注解来进行自动依赖的注入

**tomcat启动时会启动spring容器，spring会自动去扫描全包并根据注解进行实例对象的创建和管理**

### spring ioc

pring ioc，spring容器，根据xml配置，或者是你的注解，去实例化你的一些bean对象，然后根据xml配置或者注解，去对bean对象之间的引用关系，去进行依赖注入，某个bean依赖了另外一个bean

底层的核心技术，反射，他会通过反射的技术，直接根据你的类去自己构建对应的对象出来，用的就是反射技术

spring ioc，系统的类与类之间彻底的解耦合

## AOP机制

AOP （Aspect Orient Programming）,直译过来就是 面向切面编程。AOP 是一种编程思想，是面向对象编程（OOP）的一种补充。面向对象编程将程序抽象成各个层次的对象，而面向切面编程是将程序抽象成各个切面。

spring核心框架里面，最关键的两个机制就是IOC和AOP

AOP的核心技术是**动态代理**

AOP 领域中的特性术语：

- 通知（Advice）: AOP 框架中的增强处理。通知描述了切面何时执行以及如何执行增强处理。
- 连接点（join point）: 连接点表示应用执行过程中能够插入切面的一个点，这个点可以是方法的调用、异常的抛出。在 Spring AOP 中，连接点总是方法的调用。
- 切点（PointCut）: 可以插入增强处理的连接点。
- 切面（Aspect）: 切面是通知和切点的结合。
- 引入（Introduction）：引入允许我们向现有的类添加新的方法或者属性。
- 织入（Weaving）: 将增强处理添加到目标对象中，并创建一个被增强的对象，这个过程就是织入。

其实就是对于一些公用的逻辑进行抽取，然后集中的在某些请求之前去执行，spring在运行的时候，动态代理技术会给那些类生成动态代理对象

## 动态代理

动态的创建一个代理类出来，创建这个代理类的实例对象，在代理类里面引用真的开发的类，所有接口的调用都先走代理类的对象，代理类负责做代码上的增强，再去调用实际开发类

spring aop会使用jdk动态代理，**生成一个同样接口的代理类**，构造实例对象，所以jdk动态代理在类有接口的时候就会使用。如果没有实现接口的，aop会使用cglib来动态代理

cglib的生成动态代理，是**生成一个类的子类**，可以动态生成字节码去覆盖方法，在方法中加入增加的代码

## Bean

### bean的作用域

1. singleton：默认，每个容器中只有一个bean实例（单例）
2. prototype：为每一个bean提供一个实例（每次调用创建一个新的实例）
3. request：每一次网络请求创建一个实例，请求完成后销毁
4. session:每个session中创建一个实例
5. global-session

大部分来说，基本上开发中使用到的都是单例的，prototype也使用的极少

**bean不是线程安全的**，即便是默认的singleton也是线程不安全的

在多线程请求下，多个请求其实请求到的是同一个实例，在对象中的实例变量是不安全的

## 事务

### 实现原理

添加了@Transactional注解后，spring会使用aop思想，在这个方法执行之前开启事务，执行完毕后根据是否报错来决定是提交事务还是回滚事务

### 事务传播机制

1. PROPAGATION_REQUIRED：当前不存在事务则创建事务，存在事务则加入事务
2. PROPAGATION_SUPPORTS：存在事务则加入事务，不存在则已非事务执行
3. PROPAGATION_MANDATORY：存在事务则加入事务，不存在则抛出异常
4. PROPAGATION_REQUIRES_NEW：不管存不存在事务，都创建事务
5. PROPAGATION_NOT_SUPPORTED：非事务执行，如果有当前事务，则挂起事务
6. PROPAGATION_NEVER：非事务执行，存在事务则抛出异常
7. PROPAGATION_NESTED：存在当前事务，在事务内执行，没有事务，则按照REQUIRED属性执行

嵌套事务：外层事务回滚会导致内部事务回滚，内部事务回滚，只回滚自己的操作

如果需要两个事务都只回滚自己的部分，则可以使用REQUIRES_NEW

## spring boot核心架构

### 核心思想

基于spring + spring mvc + orm框架，在一定程度上来简化开发流程

spring boot 内嵌一个tomcat去直接把写好的java web系统启动起来，直接运行一个方法，直接把tomcat服务器给跑起来，把代码运行起来

### 自动装配

只需要引入starter依赖，在一定程度上自动完成依赖的配置和定义，只需要做一些简单必须的配置，不需要手工做大量的配置，简化搭建工程成本。

自动引入需要的jar包，并生成对应的bean

mian方法：直接启动内嵌tomcat服务器，后续的流程就是spring容器启动的流程

## spring核心架构

### spring bean的生命周期

创建 -> 使用 -> 销毁

使用xml或者注解定义一大堆的bean



1. 实例化bean：在使用bean的时候
2. 设置对象属性（依赖注入）：需要看bean的依赖关系，把依赖的bean创建并进行注入（构造方法或者setter）
3. 处理Aware接口：如果bean实现了ApplicationContextAware接口，spring容器就会调用我们的bean的setApplicationContext(ApplicationContext)方法，传入Spring上下文，把spring容器给传递给这个bean
4. BeanPostProcessor：如果我们想在bean实例构建好了之后，此时在这个时间，我们想要对Bean进行一些自定义的处理，那么可以让Bean实现了BeanPostProcessor接口，那将会调用postProcessBeforeInitialization(Object obj, String s)方法。
5. InitializingBean 与 init-method：如果Bean在Spring配置文件中配置了 init-method 属性，则会自动调用其配置的初始化方法。
6. 如果这个Bean实现了BeanPostProcessor接口，将会调用postProcessAfterInitialization(Object obj, String s)方法
7. DisposableBean：当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，会调用其实现的destroy()方法；
8. destroy-method：最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。
   大致流程：初始化bean ->进行依赖注入 ->是否需要把spring容器传递给bean ->执行定义的初始化bean之前的方法 ->初始化bean ->执行初始化bean后的方法 ->bean不再需要时销毁bean（两个调用函数可选择）

## 设计模式

spring源码中使用了很多的设计模式

### 工厂模式

spring ioc核心的设计模式，把所有的bean实例都放在spring容器里，如果需要使用bean，直接找spring容器拿就可以了

### 单例模式

spring默认来说，bean都是单例模式，类在系统运行期间只有一个实例对象，只能有一个bean

### 代理模式

aop的核心模式就是代理模式，创建动态代理对象，在访问对象时先访问代理对象，然后通过代理对象做增强后去访问真正的对象

## spring web mvc核心架构

1. tomcat的工作线程将请求转交给mvc框架的DispatcherServlet
2. DispatcherServlet去寻找有@Controller的Controller，一般会对Controller增加@RequestMapping注解，用于标注Controller对哪些请求做处理，根据uri去定位Controller
3. 根据@RequestMapping去找地址请求处理的方法
4. 调用方法进行业务逻辑的处理
5. 处理完成后，返回值给前端（前后端分离）



