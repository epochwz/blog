# 树的抽象数据类型

```
ADT 树(Tree)
Data
    树由一个根结点和若干棵子树构成。
    树中结点具有相同的数据类型及层次关系
Operation
    initTree(*T)                构造空树
    destoryTree(*T)             销毁树T
    clearTree(*T)               若树T存在则置为空树
    isEmpty(T)                  若树为空则返回TRUE，否则返回FALSE
    treeDepth(T)                返回树T的深度
    root(T)                     返回树T的根结点
    value(T,cur_e)              返回树T中结点cur_e的值
    assign(T,cur_e,value)       将value赋值给树T中结点cur_e
    parent(T,cur_e)             返回非根结点cur_e的双亲结点，否则返回空
    leftChild(T,cur_e)          返回非叶结点的最左孩子
    rightSibiling(T,cur_e)      返回结点cur_e的右兄弟，若不存在则返回空
    insertChild(*T,*p,i,c)      插入非空树c为树T中p所指结点的第i棵子树
                                p:指向树T中的某个结点
                                c:非空树、与T不相交
                                i:p所指结点的度+1
    deleteChild(*T,*p,i)        删除树T中p所指结点的第i棵子树
    createTree(*T,definition)   按definition中给出树的定义来构造树
endADT
```



