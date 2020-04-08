# IoC & AOP

## AOP

面向切面编程 (Aspect oriented Programming,AOP) 采用横向抽取机制取代传统的纵向继承体系来达到消除重复性代码的目的

- AOP 就是指在 `运行期 / 编译期 / 类装载期` 通过 `横向抽取机制 (代理机制)` 向 `目标类` `织入` `增强代码` 并产生 `代理对象` 的过程
- AOP 通常应用于性能监视、事务管理、安全检查、缓存等通用功能场景

AOP 体现了软件编程的一个基本原则： **关注点分离**，即不同的问题交给不同的部分去解决

**相关术语**

| 术语       | 术语翻译    | 解释                                        | 备注                                                          |
|:-----------|:-----------|:--------------------------------------------|:--------------------------------------------------------------|
| JointPoint | 连接点      | 指那些可以被拦截的点                         | 在 Spring 中这些点指的是方法，因为 Spring 只支持方法类型的连接点 |
| PointCut   | 切入点      | 指真正被拦截的连接点                         |                                                               |
| Advice     | 通知 / 增强 | 指拦截后所要做的事情                         |                                                               |
| Aspect     | 切面        | 指切入点和通知的组合                         | 切面定义了 需要拦截哪些连接点 以及 如何对其进行增强              |
| Target     | 目标对象    | 需要被代理的目标对象                         |                                                               |
| Proxy      | 代理对象    | 一个对象被应用了增强代码后就会产生一个代理对象 |                                                               |
| Weaving    | 织入        | 指把增强代码应用到目标对象并产生代理对象的过程 |                                                              |

**通知类型**

- 普通通知：前置通知、后置通知、环绕通知、异常通知、最终通知
- 特殊通知：引介通知 (能够在不修改类代码的前提下，在运行期给一个类动态的添加一些属性或方法)

**织入类型**

- 动态代理：运行期织入 (Spring AOP)
- 静态代理：编译期织入、类装载期织入 (AspectJ)

**总结**：将 `切面 (切入点和增强代码)` `织入` 到 `目标对象` 从而产生 `代理对象` 的过程，称为 `面向切面编程 (AOP)`

## Proxy

代理类型

- 静态代理
- 动态代理
  - JDK
  - CGLib

Spring AOP 底层通过 JDK / CGLib 动态代理技术，在运行期向目标对象织入增强代码并生成代理对象 (由于完全使用纯 Java 实现，因此不需要专门的编译器或类加载器)

- 若目标对象实现了若干个接口，则 Spring 使用 JDK 的 Proxy 类生成代理
- 若目标对象没有实现任何接口，则 Spring 使用 CGLib 库生成目标对象的子类

代理

- 程序中应该优先对接口创建代理，便于程序解耦和维护
- 程序中应该格外注意目标类 final 修饰符的使用情况
  - JDK: 接口中的方法不能使用 final 修饰，因为 final 方法无法被重写
  - CGLib: 类和方法都不能使用 final 修饰，因为 final 方法无法被重写，final 类无法被继承

## Advice

Spring AOP 通知类型：按照通知 (Advice) 在目标类方法的连接点位置，将其分成 5 类

| 通知类型 | 通知作用                    | Spring AOP 接口         | AspectJ 注解    |
|:--------|:---------------------------|:------------------------|:----------------|
| 前置通知 | 在目标方法执行之前就实施增强 | MethodBeforeAdvice      | @Before         |
| 后置通知 | 在目标方法执行之后才实施增强 | AfterReturningAdvice    | @AfterReturning |
| 环绕通知 | 在目标方法执行的前后实施增强 | MethodInterceptor       | @Around         |
| 异常通知 | 在目标方法抛出异常后实施增强 | ThrowsAdvice            | @AfterThrowing  |
| 最终通知 | 无论是否发生异常都会进行通知 |                         | @After          |
| 引介通知 | 给目标对象添加新的属性和方法 | IntroductionInterceptor | @DeclareParents |

Spring AOP 切面类型

- Advisor: 一般切面，Advice 本身就是一个切面，会对目标类的所有方法进行拦截
- PointcutAdvisor: 带有切点的切面，可以指定拦截目标类的特定方法
- IntroductionAdvisor: 引介切面，针对引介通知使用的切面

Spring AOP 自动代理

每个代理都需要手动通过 ProxyFactoryBean 织入切面代理，开发维护量极大

解决方案：自动创建代理

- BeanNameAutoProxyCreator 根据 Bean 名称创建代理
- DefaultAdvisorAutoProxyCreator 根据 Advisor 本身包含信息创建代理
- AnnotationAwareAspectJAutoProxyCreator 基于 Bean 中的 AspectJ 注解进行自动代理

原始的代理：先产生目标类，再通过目标类产生代理

自动的代理：在目标类产生的过程中就生成代理，目标类就是代理类

## AspectJ

AspectJ 是一个基于 Java 语言的独立的 AOP 框架，Spring 2.0 开始集成并推荐使用 AspectJ 进行 AOP 开发

AspectJ 通过 `切点表达式` 在 `通知` 上定义 `切入点`，两者可以自由组合成 AOP 的 `切面`

AspectJ 切点表达式的语法：`execution(Modifier ReturnValue package.class.method(parameters) throws Exception)`

```
修饰符：可以省略
  public/protected/private  精确匹配
  *			                任意匹配
  
返回值：不能省略
  void			        空
  String		        系统类 (同包或无需导包的类)
  fun.epoch.learn.Demo  指定类 (不同包的类)
  * 			        任意
  
包：    可以省略
  fun.epoch.learn			    固定包
  fun.epoch.learn.*.service	    learn 包下的子包任意 （例如：fun.epoch.learn.staff.service）
  fun.epoch.learn..			    learn 包下的所有子包（含自己）
  fun.epoch.learn.*.service..	learn 包下的任意子包，固定目录service，service目录任意包
  
类：    可以省略
  *			任意类
  UserDao   指定类
  *Impl		以Impl结尾
  User*		以User开头
  
方法名：不能省略
  *         任意方法
  doThing	固定方法
  do*		以do开头
  *do		以do结尾
  *do*      包含有do
  
(参数)
  ()            没有参数
  (int)         一个参数
  (int,String)  两个参数
  (*)           任意参数
  (int,*)       一个整型 + 任意参数
  
throws  可以省略
```

示例

```
匹配任意类的任意公共方法        execution(public * *(..))                         省略了包、类
匹配指定包中的任意类的任意方法  execution(* fun.epoch.learn.*(..))   不包含子包    省略了修饰符、类
匹配指定包及其子孙包中的任意类的任意方法  execution(* fun.epoch.learn..*(..))
匹配指定类的所有方法            execution(* fun.epoch.learn.Demo.*(..))
匹配实现指定接口的所有类的任意方法 execution(* fun.epoch.learn.IDemo+.*(..))
匹配任意 save 开头的方法        execution(* save*(..))
```

## 另外的解读

面向切面编程 (AOP) 体现了软件编程的一个基本原则：**关注点分离**，即不同的问题交给不同的部分去解决
- 通用化功能代码的实现，对应的就是 切面 (Aspect)
- 业务功能代码和切面代码分离后，架构变得高内聚低耦合
- 确保功能的完整性：切面最终需要被合并到业务中 (Weave)

AOP 的三种织入方式

- 编译时织入：在代码编译时，就把切面代码融合进去，生成完整功能的字节码，因此需要特殊的编译器，如 AspectJ
- 类加载时织入：在 Java 字节码加载的时候，把切面的字节码融合进去，因此需要特殊的类加载器，如 AspectJ / AspectWerkz
- 运行时织入：在运行时通过动态代理的方式，调用切面代码，增强业务功能，如 Spring AOP
  - 动态代理会产生额外的性能开销，但是无需特殊的编译器或类加载器，只需要按照普通的 Java 开发方式写代码即可
