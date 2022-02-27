# 输入/输出

## 绝对路径和相对路径

绝对路径：从盘符开始的路径

相对路径：从当前路径开始的路径

## 字节流

```java
字节输入流 `InputStream`
    FileInputStream 文件输入流，直接从硬盘读数据

字节输出流 `OutputStream`
    FileOutputStream 文件输出流，直接写数据到硬盘
    BufferedOutputStream 缓冲输出流，如果缓冲区满了，会自动写入，否则需要调用 flush() 或者 close() 强制清空缓冲区（将缓冲区数据写入硬盘）
```

## 字符流

```java
字节字符输入转换流 InputStreamReader

字节字符输出转换流 OutputStreamWriter
    BufferedWriter

```

## 序列化和反序列化

序列化：把Java 对象转换成字节序列的过程
反序列化：把字节序列恢复 成Java 对象的过程