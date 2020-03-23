# 模板方法

类型：行为型

定义：定义了一个算法的骨架，并允许子类为其中的一个或多个步骤提供实现

- 模板模式使得算法子类可以在不改变算法结构的前提下，重新定义算法的某些步骤

适用场景
- 一次性实现一个算法的不变的部分，并将可变的行为留给子类来实现
- 提取各个子类中的公共行为并集中到一个父类中，从而避免代码重复

优点
- 提高复用性：在父类中实现公共代码
- 提高扩展性：在子类中实现专属代码 (扩展子类行为)
- 符合开闭原则

缺点
- 增加系统的类数目
- 增加系统的复杂度
- 如果父类添加新的抽象方法，所有子类都必须改一遍

扩展
- 钩子方法：提供算法的缺省行为，而子类可以在必要时进行扩展

相关模式

- 工厂方法：模板方法是工厂方法的一种特殊实现
- 策略模式
  - 策略模式：使不同的算法可以相互替换，且不会影响客户端代码
  - 模板方法：将算法中的可变步骤交由子类实现但不改变算法流程