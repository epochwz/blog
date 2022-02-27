# 队列

## 定义

只能从一端(队首)添加元素,只能从另一端(队尾)取出元素的线性表

## 实现

- ArrayQueue: 基于动态数组实现

## 复杂度分析

### ArrayQueue

- dequeue() O(n)
- enqueue() O(1) 均摊
- peek() O(1)

### LoopQueue

- dequeue() O(1) 均摊
- enqueue() O(1) 均摊
- peek() O(1)

### LinkedQueue

- dequeue() O(1) 创建结点对象的操作较为耗时
- enqueue() O(1) 创建结点对象的操作较为耗时
- peek() O(1)