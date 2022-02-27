# Shell运算符

**声明变量类型**

```
$ declare [Options] varName

[Options]
+    撤销变量类型(退回默认字符串类型)
-    设置变量类型

-a    数组
-i    整数
-x    环境变量
-r    只读变量
-p    显示指定变量的被声明类型
```

**声明数组变量**

```
$ arrName[index]=varValue
或
$ declare -a arrName[index]=varValue
```

**数值运算**

```
$ $(( 运算式 ))

eg.
$ $(( $var1+$var2 ))
```

**运算符**

![](/Images/Linux/Shell运算符.png)

