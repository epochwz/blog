# JDBC

JDBC: Java DataBase Connectivity, Java 数据库连接

- 用于执行 SQL 语句的 Java API
- 可以为多种关系型数据库提供统一的访问

## HelloWorld

JDBC 编程步骤

1. 加载数据库驱动
2. 连接数据库
3. 获取用于向数据库发送 SQL 的 Statement 对象
4. 从结果集 ResultSet 中取出数据
5. 断开数据库连接，释放资源

## 常用 API

```bash
DriverManager:  驱动管理
    注册数据库驱动
    获取数据库连接

Connection:     连接对象
    获取 SQL 语句执行器
        Statement createStatement();                        执行 SQL 语句，存在 SQL 注入的漏洞
        PrepareStatement prepareStatement(String sql);      预编译 SQL 语句并执行，解决 SQL 注入
        CallableStatement prepareCall(String sql);          执行 存储过程
    事务管理
        setAutoCommit(boolean);                 设置是否自动提交事务
        commit();                               提交事务
        rollback();                             事务回滚

Statement:      SQL 执行对象
    执行 SQL 语句
        boolean execute(String sql);        执行 SQL 语句；如果执行 SELECT 则返回 true, 否则返回 false
        ResultSet executeQuery(String sql); 执行 SELECT 语句
        int executeUpdate(String sql);      执行 INSERT/UPDATE/DELETE 语句；返回影响行数
    执行 SQL 批处理
        addBatch(String sql);               添加 SQL 到批处理中
        executeBatch();                     执行批处理
        clearBatch();                       清空批处理

ResultSet:      结果集，查询语句(SELECT)查询结果的封装
    next();     是否还有记录，如果有则指向下一行记录并返回 true, 否则返回 false
    getXXX(String colName);     获取当前记录指定名称的字段值
    getXXX(int colIndex);       获取当前记录指定位置的字段值
```

## 资源释放

JDBC 程序运行完成之后，应该释放数据库交互对象(Connection/Statement/ResultSet)

- Connection 是极其稀有的资源，如果不能及时、正确的关闭，极易导致系统宕机。因此 Connection 应该晚创建、早释放
