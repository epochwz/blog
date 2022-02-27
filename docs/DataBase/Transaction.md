# Transaction

## 简单介绍

**概念**：一般特指数据库事务 (Database Transaction)，指一系列操作，要么全都执行，要么全都不执行

**特性**：ACID (Atomicity,consistency,isolation,durability)

- **原子性**：一个事务必须是一个不可分割的执行单元
- **一致性**：一个事务必须使数据库从一个一致性状态 A 变成 另一个一致性状态 B
  - 当然，允许 A & B 在数据上不发生任何改变，但在逻辑上依然可以认为数据库完成了一次一致性状态的转换
- **隔离性**：一个事务在执行时必须不受其他事务干扰 (并发安全性)
- **持久性**：一个事务一旦提交，则其对数据库的改变就是永久性的

## 并发问题

### 并发安全问题

- **脏读**：TA 读取到 TB 的数据，但随后 TB 回滚了
  - 只允许事务读取持久性数据
- **不可重复读**：TA 先读取数据，随后 TB 修改数据并提交，TA 再次读取数据 (同一次事务内两次读取结果不一致)
  - 锁行
- **幻读**：TA 先修改数据，随后 TB 插入数据并提交，TA 再次读取数据时发现有新的记录 (再次读取与期望不一致)
  - 锁表

### 事务隔离级别

| 隔离级别 | 英文描述          | 脏读 | 不可重复读 | 幻读 |
|:-------|:-----------------|:-----|:---------|:----|
| 读未提交 | READ UNCOMMITTED | 是   | 是       | 是   |
| 读已提交 | READ COMMITTED   | 否   | 是       | 是   |
| 可重复读 | REPEATABLE READ  | 否   | 否       | 是   |
| 串行化   | SERIALIZABLE     | 是  | 是        | 是  |

```sql
# 查询数据库默认隔离级别
SELECT @@tx_isolation;
# 设置当前会话的隔离级别
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED ;
```

## Spring 事务传播行为

## 分布式事务

ACID
- Atomicity
- consistency
- isolation
- durability

CAP
- Consistency
- Availability
- Tolerance of network Partition

BASE
- Basically Availability
- Soft State
- Eventual Consistency

ACID 刚性事务 -> CAP 理论 -> BASE 柔性事务