# 泛型

## 为什么

在增加泛型之前，泛型程序设计是使用继承来实现的

- 需要强制类型转换，可能导致运行时 `ClassCastException`
- 可以向集合中添加任意类型的对象，存在风险

## 泛型的使用

`List<String> list=new ArrayList<String>();`

JDK1.7 之后，构造器可以省略泛型类型

`List<String> list=new ArrayList<>();`

## 多态与泛型

`List<Animal> list=new ArrayList<Cat>();`，错误，传递给实际对象的泛型类型必须匹配变量声明的泛型类型

## 泛型作为方法参数

## 自定义泛型类

## 自定义泛型方法

泛型方法不需要定义在泛型类中