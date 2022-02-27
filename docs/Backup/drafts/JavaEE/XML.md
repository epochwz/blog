# XML

**XML**(Extensible Markup Language, 可扩展标记语言)：一种类似 HTML 的标记语言，文件扩展名是 `.xml`. XML 具备良好的人机可读性，通常用于以下场景

- 描述配置文件
- 保存程序数据
- 网络数据传输

**XML & HTML**

- 都是标记语言，都需要编写标签
- XML 没有预定义标签，HTML 有大量预定义标签
- XML 用于保存和传输数据，HTML 用于显示信息

## 文档结构

- 第一行必须是 XML 声明：说明 XML 文档的基本信息，包括版本号和字符集

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    ```

- 有且只有一个根节点

## 书写规范

- 适当的注释和缩进
- 合法的标签名
  - 标签名应该反映实际意义
  - 使用小写字母，单词之间使用 `-` 连接
  - 不同级标签不能重名
- 有序的子元素：多层嵌套的子元素中，各个子元素标签的前后顺序应该保持一致，具备更好的可读性
- 合理使用属性：标签属性通常用于描述标签不可或缺的信息，如标签的分组信息 (category) 和唯一标识 (id)
- 特殊字符的两种正确的表示方式
  - 实体引用

    ```xml
    < &lt;
    > &gt;
    & &amp;
    ' &apos;
    " &quot;
    ```

  - CDATA TAG: 指定不应该由 XML 解析器进行解析的文本数据 `<![CDATA[]]>`

## 语义约束

有时候即便 XML 文档结构是正确的，但仍然不是有效的，而 **XML 语义约束** 就是用来规定 XML 文档允许出现的元素，避免无效标签

- **DTD**(Document Type Definition, 文档类型定义)：一种简单易用的语义约束方式
- **XML Schema**：一种复杂的、功能丰富的语义约束方式，是 W3C 的标准规范，提供了数据类型、格式限定、数据范围等高级特性

## 文档对象模型

**DOM**(Document Object Model, 文档对象模型): 定义了访问和操作 XML 文档的标准方法

- DOM 把 XML 文档作为树结构来查看，能够通过 DOM 树来读写所有元素

## XPath 路径表达式

XPath 是一门在 XML 文档中查找数据的表达式语言，可以极大地提高提取 XML 数据的效率

- **基本表达式**：通过路径筛选需要选取的节点，表达式之间可以进行任意的组合

  ```bash
  .           选取当前节点
  ..          选取当前节点的父节点
  nodename    选取当前节点下的所有子节点 nodename
  /nodename   选取根节点下的所有子节点 nodename
  //nodename  选取整个文档下的所有子节点 nodename
  @attrname   选取拥有属性 attrname 的所有节点
  ```

- **谓语表达式**：对基本表达式选出的节点进行条件限制

  ```bash
  [1]         选取第一个元素
  [last()]    选取最后一个元素
  [last()-1]  选取倒数第二个元素
  [position<3] 选取前三个元素
  [@attrname] 选取拥有属性 attrname 的元素
  [@attrname='attrvalue'] 选取 属性 attrname=attrvalue 的元素
  [element<3] 选取子元素 element<3 的元素
  ```

- 示例

  ```bash
  # 选取所有 node1 节点下的前 10 个 node2 节点，再选择其中属性 name='target' 的 node3 节点下面的 node4 节点
  //node1/node2[position()<10]/node3[@name='target']/node4
  ```

## Dom4j & Jaxen

**Jaxen**：一个 Java 编写的开源 XPath 库，适应多种不同的对象模型 (DOM,XOM,JDOM,dom4j)

**Dom4j**: 一个基于 Java 的性能优异、功能强大、极易使用的开源 XML 解析库，底层依赖 Jaxen 实现 XPath 查询

## 参考资源

[W3Schools XML Tutorial](https://www.w3schools.com/xml/default.asp)