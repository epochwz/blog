# 面试准备

## 自我介绍

面试官您好，我叫纪伟忠，是广东工业大学网络工程系 2018 届的本科毕业生。毕业之后一直就职于广州南盾通讯设备有限公司，现在已经从事 Java 开发一年半时间左右。

就职期间，我主要负责维护和扩展一个《数据中心微模块管理系统》，这个系统主要做的是对机房环境进行实时监控并提供异常告警等功能。

- 工作前期，我主要负责维护、扩展、实施该项目。在这期间，我开发了一个配置生成器和一个设备模拟器，大大简化了项目部署时的工作量并降低了实施时的出错率
- 工作后期，我主要负责开发了一个告警规则管理模块和一个门禁管理系统，并全程参与了需求分析、原型设计、用例描述、接口设计、数据库设计和代码实现的整个过程

在这段工作经历中，我主要收获了对复杂业务进行抽象并解决实际问题的能力，也熟悉了如何从零开始设计并实现一个功能模块

我对自己的评价是基础比较扎实，有深入钻研技术的热情，并且对代码质量有比较高的追求，在工作之余会主动去学习像设计模式之类的知识，然后尝试应用到工作的项目中去

- 比如说我在实现门禁系统底层所依赖的设备通信模块时，就运用了简单工厂、享元模式、原型模式、模板模式、策略模式等去实现了一个类似 IoC 容器的设备工厂，具有比较强的扩展性，让外部程序的使用变得十分简单，在后续开发门禁系统的应用层，以及定制一些需要设备控制功能的项目的时候变得十分方便

以上就是我的自我介绍，谢谢！

##  职业规划

- 工作规划：尽快熟悉公司的业务，在做好本职工作的同时，尝试重构和优化项目
- 学习规划：不断巩固基础知识，同时学习前沿技术，并通过写博客、读源码、参与开源项目等不断提高自己的技术水平
- 未来规划：尽早成为独当一面的高级工程师，最终希望能够成为架构师
  1. 巩固基础知识 (设计模式、数据结构和算法、计算机网络、操作系统、并发编程、JVM)
  2. 学习前沿技术 (微服务，大数据，各种框架)
  3. 坚持运营博客
  4. 阅读优秀源码
  5. 参与开源项目

## 离职原因

- 主观原因
  1. 业务方面，因为公司的项目主要面向政府或者内部人员使用，所以希望可以接触一些面向用户的，有高并发、大数据之类的场景
  2. 技术方面，由于公司缺乏代码审查、测试、技术培训等环节，所以感觉技术提升遇到了瓶颈，希望体验一下比较规范的开发流程
- 客观原因：当时手头上负责开发的模块刚好结束，又准备去考驾照

## 待业原因

刚辞职的时候是花了一个多月去考驾照，然后准备面试的时候，可能因为一开始目标定的比较高，自己又没什么面试经验，加上工作中没接触过像并发之类的东西，所以就花了比较长的时间在学习和准备，但是时间一长，工作的项目又给忘的差不多了，加上断档时间比较长，就越来越缺乏自信，所以一直迟迟没有投简历，拖到了现在。

但是在这期间，我也是一直在很认真的学习和提高自己，总结和巩固基础知识，学习并发、数据结构和算法、JVM 等等，尽量完善自己的知识体系，希望能够让自己更好地胜任工作，所以说我对技术还是比较热忱的，也不存在工作态度的问题，希望您可以尽量忽略这个空窗期的因素

## 主动提问

### 技术面

1. 产品介绍：我来之前了解到贵司是做 xxx 的，但对这个业务还不是特别的了解，希望您可以介绍一下
2. 团队介绍：可以简单介绍一下我们的开发团队吗 (人员数量，人员组成、平均年龄)
3. 工作内容：还想了解一下我们部门的工作内容和开发流程，比如说有没有 Code Review, 测试等环节 (因为我上家公司是没有这些，所以比较看重这一块)
4. 技术培训：那我们部门技术氛围怎么样，平时有没有一些技术培训或者技术分享
5. 个人建议：针对本次面试情况，您对我有什么建议呢？

好的，基本情况我都了解了，最后想请问一下，我大概什么时候可以收到面试结果呢？

好的谢谢，感谢您耐心的面试 (非常希望可以加入你们)

### 人事面

**个人发展**

- 公司现状
- 职位晋升
- 薪资调整 (固定涨薪？主动申请？--> 绩效考评？)

**薪酬福利**

- 工资
  - 转正后 10k+
  - 试用期 08k+ (时长？转正条件？)
  - 年终奖 年底双薪 (年限？绩效？)
- 社保
  - 五险一金 (基数？工资？)
  - 薪资组成 (底薪？绩效？)
- 加班：频率 (调休？补贴？)
- 补贴：餐补？交通补贴？
- 假期：法定？
- 福利：其他 (学习补贴？)

## 项目介绍

### 数据中心微模块管理系统

**项目功能**

- 实时监控：比如说室内的温湿度，空调出风回风的温湿度，配电柜的电流电压功率，烟雾告警、水浸告警等
- 异常告警：比如说温湿度过高过低、漏水的时候，就需要发送短信告警等
- 设备控制：比如说发生烟雾告警的时候，需要打开天窗进行通风，温度过高时可能需要降低空调温度

**项目实现**

请求命令 -> 发送器 -> 请求响应 -> 解析器 -> 设备定义 (传感器值) -> 数据处理

数据处理

- 保存数据
- 推送数据
- 触发告警

#### 配置生成器

**为什么有那么多的配置？**

- 因为业务的数据模型比较复杂，主要是对设备进行监控，所以需要抽象出各种设备
  - 不同型号的设备有不同的协议，每个设备又拥有大量的传感器
  - 设备、传感器、告警规则、数据处理任务、推送地址

**痛点**

- 需要手动进行配置文件的复制和修改
- 各种设备的配置散落在各个项目，难以查找

**实现**：实现不同系统配置文件和多种设备对应的配置文件生成器 (FileGenerator), 根据通道配置中的设备清单和通道配置项，使用策略模式生成配置文件

**未解决**：旧项目新增设备时，由于无法确认之前的改动，因此只能手动复制配置文件

#### 设备模拟器

**为什么需要设备模拟器？**

- 一方面是，因为每个项目的设备比较繁杂，又是通过配置文件来表示，所以之前手动配置时极易出错，即便后来使用工具生成，也不能百分百的确保正确性
- 另一方面，因为不同的项目经常需要对接其他厂商的设备，并且只能够拿到他们提供的设备通信协议，没有真实的设备可以进行测试
  - 而项目的核心功能就是要完成设备之间的通信，包括按照协议规则来生成请求命令，解析响应数据，所以有必要验证程序是否能够正确的拼装和解析数据
- 还有就是，有时候进行项目招标和演示的时候，也会需要仿真的数据
- 所以需要在实施项目之前模拟出需要的所有设备，提供一个仿真的测试环境，验证整个项目跑起来之后的正确性

**如何模拟？**

- 按照设备的通信方式，实现多种类型的 Server (UDP、TCP、Serial、Telnet)
- 按照设备的通信协议，实现不同类型的 DataProcessor (数据处理器：根据请求数据，按照协议规则返回相应的响应数据)
- 通过两者的排列组合，可以模拟出不同型号的设备，通过配置文件来表达需要模拟的各种设备及其监听的 IP 和端口，就可以对程序发送的请求命令进行响应，从而使得程序可以获得仿真的设备数据

### 告警规则模块

**为什么需要做这个模块？**

在原本的旧系统中，一个设备的告警规则是在配置文件中写死的，比如说温度不能高于 25，湿度不能高于 70% 这样子，用户无法自定义，也无法添加和删除，所以需要提供告警规则的管理功能。

然后由于设备是否产生告警是用传感器的值和告警规则进行匹配，不可能让用户自行添加。因此还需要提供一个告警规则模板库，用户可以拷贝模板库，然后在此基础上进行增删改

- 需求分析：提供一个设备告警规则模板库，同时支持自定义设备告警规则
- 原型设计：设计用户操作界面
- 用例描述：对用户界面的每一个操作进行详细的描述 (用例简述、前置条件、用户行为、异常流程、正常结果)
- 接口设计：设计系统需要提供的接口
- 数据设计：设计数据库表、视图、存储过程
- 代码实现：基于 SSM 实现

### 门禁管理模块

**设备库模块**

```
设备
    属性    命令 + 命令发送器
    方法    读 / 写 →→ 响应结果
    
命令
    请求    拼装请求命令：长度校验、CRC校验 --> 模板模式
    响应    解析响应结果
    
设备工厂: 获取设备 (享元(延迟加载)+原型)
```

**应用层模块**

1. 通过设备型号获取设备
2. 设置设备的可变的属性 (发送器，设备地址)
3. 通过设备实例和命令名称指定执行的命令，获取设备响应
4. 通过设备响应执行业务逻辑

1 & 2 可以通过自定义特定的设备工厂进行组装和享元 (该模块已属于应用层，知道自己在使用哪个设备)

1 & 2 & 3 可以通过配置预定义

#### 设备查询系统

1. 通过型号获取设备实例
2. 通过修改设备实例属性
3. 通过命令名称获取请求

#### 门禁管理系统

**人员管理**

系统管理员：负责账号管理
- 设备管理员：负责设备管理，可以管理(添加删除修改)设备
- 机房巡检员：负责机房巡检，可以查看个人信息、开锁日志、可以开锁 的设备

**设备管理**

```
控制器  CRUD     删改    → 重载
锁设备  CRUD     删改增   → 重载

设备控制
  对时
  开门时间
  开门延迟时间
  工作模式
      密码 || 卡号
      密码 && 卡号
      密码
      卡号
  设备地址
  远程开锁
  开锁状态
  读取 / 清空 SOE
  新增 / 修改 / 删除 (卡号 / 密码)
```

**日志管理**

- 应用层：支持通过 卡号、密码、账号、开锁结果、锁设备、控制器 等条件筛选日志
- 采集器：对每个控制器执行轮询任务：死循环轮询控制器中的所有锁设备，一旦发现产生新的日志记录，请求并解析，然后开启新线程执行日志处理任务