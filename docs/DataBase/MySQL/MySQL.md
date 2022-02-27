
```mysql
# 检索
SELECT DISTINCT col FROM table;   # 去重
SELECT * FROM table LIMIT 1,5;    # 从行2开始选择5行
SELECT * FROM table ORDER BY col; # 默认升序
SELECT * FROM table ORDER BY col DESC ; # 降序
SELECT * FROM table ORDER BY col1 DESC ,col2; # 先按col1降序，再按col2升序

SELECT * FROM table ORDER BY price DESC LIMIT 1;  # 选择最贵的一项

# 过滤
SELECT * FROM table WHERE col1=col2; # 比较
SELECT * FROM table WHERE col BETWEEN col1 AND col2; # 范围
SELECT * FROM table WHERE id IN (1000,2000); # 枚举
SELECT * FROM table WHERE col IS NULL ; # 空值
SELECT * FROM table WHERE (price > 10 AND email IS NULL) OR id=1000; # 多条件
# NOT IN | IS NOT NULL | NOT EXISTS # NOT 表示否定

# 通配符过滤
# 使用通配符，默认不区分大小写
# % 表示 任意0或多个字符，无法匹配 NULL 值
SELECT * FROM table WHERE col LIKE 'epoch%'; # 匹配 col 以 epoch 开头的行
# _ 表示 任意单个字符
SELECT * FROM table WHERE col LIKE '_'; # 匹配 col 是单个字符的行

# 正则表达式过滤
SELECT * FROM table WHERE col REGEXP '.000'; # 匹配 col=任意字符+'000' 的行
SELECT * FROM table WHERE col REGEXP BINARY 'Hello .000'; # 匹配 col=任意字符+'000' 的行,区分大小写

# 文本/计算/日期/聚集函数
# 函数可能存在移植性问题
SELECT CONCAT(TRIM(username),'(',gender , ')') AS name FROM students; # 去除空格、拼接字段、使用别名
SELECT amount,unit_price,amount*unit_price AS total_price FROM orders; # 算术计算函数

# 聚集函数-汇总数据
# AVG() COUNT() MAX() MIN() MAX() SUM()
SELECT AVG(DISTINCT price) AS avg_price FROM products WHERE vend_id=1003 # 去重，统计平均值

# 数据分组
SELECT vend_id, COUNT(prod_id) AS products_num FROM products GROUP BY vend_id HAVING products_num>2 ORDER BY products_num DESC LIMIT 1;

# 子查询:将查询结果用于另一个查询语句
SELECT * FROM customers WHERE cust_id IN (SELECT cust_id FROM orders WHERE order_num IN (SELECT orderitems.order_num FROM orderitems WHERE prod_id = 'TNT2')) 
# 联表查询
    # 内连接(等值连接)
        SELECT vend_name,prod_id,prod_name FROM products,vendors WHERE products.vend_id=vendors.vend_id
        SELECT vend_name,prod_id,prod_name FROM products INNER JOIN vendors ON products.vend_id=vendors.vend_id
    # 
    
# 自然连接：排除相同的列
```


