# MyBatis

简介

- MyBatis 是一款优秀的持久层框架
- 使用 XML 将 SQL 与程序解耦，便于维护
- 学习简单，执行高效，是 JDBC的延伸

开发流程

- 引入 MyBatis 依赖
- 创建 核心配置文件
- 创建 Entity 实体类
- 创建 Mapper 映射文件
- 初始化 SessionFactory
- 使用 SqlSession 对象操作数据

MyBatis 环境配置

- MyBatis 使用 XML 文件 `mybatis-config.xml` 配置数据库环境信息

```xml
<!-- 配置环境 - 不同的环境使用不同的名称 -->
<environment id="dev">
    <!-- 使用 JDBC 方式进行数据库事务管理 -->
    <transactionManager type="JDBC"></transactionManager>
    <!-- 使用连接池的方式管理数据库连接 -->
    <dataSource type="POOLED">
        <property name="driver" value="com.mysql.jdbc.Driver" />
        <property name="url" value="jdbc:mysql://localhost:3306/dbname" />
        <property name="username" value="root" />
        <property name="password" value="root" />
    </dataSource>
</environment>
```

SqlSessionFactory 是 MyBatis 的核心对象，用于加载配置文件，完成 MyBatis 初始化,创建 SqlSession 对象，应该保证 SqlSessionFactory 在应用中全局唯一

SqlSession 对象是 MyBatis 操作数据库的核心对象，使用 JDBC 与数据库交互，提供了数据表的 CRUD 对应方法。可以看成增强版的 Connection 对象

## 开发步骤

### 查询

1. MyBatis 配置文件开启驼峰命名映射
2. 创建实体类
3. 创建 Mapper XML, 编写 SQL
4. MyBatis 配置文件中新增 Mapper 类
5. SqlSession 执行 SQL 语句

### SQL 传参

- 单个参数：parameterType="dataType"
- 多个参数: parameterType="java.util.Map"

### 结果映射

- resultType="java.util.LinkedHashMap"
- resultType="dtoType"+resultMap Mapping Rule
