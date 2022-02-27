# JSON

JSON(JavaScript Object Notation,JavaScript 对象表示法)，一种轻量级的文本数据交换格式

- JSON 独立于特定语言，具有自我描述性，更易理解

语法规则

- 数据由键值对(key-value)描述，由逗号分隔
- 大括号代表一个完整的对象，拥有多个键值对
- 中括号保存数组，多个对象之间由逗号分隔

标准的 JSON 格式中键值对都需要双引号

## JSON 与 字符串 相互转换

- JSON.parse() 将字符串转换成 JSON 对象
- JSON.stringify() 将 JSON 对象转换成字符串

## Java 常用 JSON 工具库

JSON 工具库：实现 JSON 和 Java 对象的相互转换，序列化和反序列化

- Jackson
- FastJson
- Gson
- Json-lib

