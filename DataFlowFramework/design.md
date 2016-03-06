[返回主页](../README.md)
设计思想
=========================
总体来看该框架的实现主要分为这几个部分：1、读取Cil信息；2、构建存储和维护Data-Flow的数据结构；3、实现Cut-point guided search方法；4、优化Cut-point guided search方法。

1、读取Cil信息

在之前的研究中，我们发现目前还没有成熟的方法利用LLVM找到代码的Def-Use pairs，在经过尝试以及与擅长LLVM的工程师沟通过后，决定还是先采用Cil工具来找到代码的Def-Use pairs，然后将信息传递给KLEE，再在KLEE内部维护这些信息。

要将Cil的信息传递给KLEE需要两个步骤，第一，需要将Cil找到的Def-Use pair信息存储在文件中。第二，需要在代码中插桩标识Cut-point以及Definition和Use的位置。而KLEE在初始化时，首先读取Def-Use pair的信息结构，构建数据结构，然后在执行时，如果遇到插桩函数，便维护KLEE内建的数据结构，最后在KLEE执行完之后，打印出执行的结果。

2、构建存储和维护Data-Flow的数据结构

再上一个模块中，我们使用KLEE读取了Cil的信息文件，Cil信息文件大致包含以下一些信息：
```
dua_id, dua_kind;
def_var_name, def_var_id, def_var_line, def_file_name, def_func_name, def_func_id, def_stmt_id; 
use_var_name, use_var_id, use_var_line, use_file_name, use_func_name, use_func_id, use_stmt_id;

dua_kind: 0 c-use , 1 p-true, 2 p-false

```
在KLEE内新增一个维护Def-Use pair的数据结构，在遇到插桩函数的时候，便更新表的状态。新增的数据结构包括，Definition、Use，这两个数据结构存储Cil文件中的信息，并且可以判断与其他对象是否相等。Def-Use pair除了包含一个Definition和一个Use之外，还包含了这个Def-Use pair的三个状态：未到达定义、到达定义、已被覆盖。最后CilInfoTable数据结构维护一个Def-Use pair的列表，作为程序最终的输出结果之一，并统计覆盖率。

3、实现Cut-point guided search方法

要实现Cut-point guided search方法，首先要在Definition和Use中维护一个Cut-points数据结构。Cut-points数据结构是cut-point数据结构的一个列表，在cut-point数据结构中，我们将会存储标识该cut-point的信息以及是否到达的状态，在符号执行遇到对应的插桩函数时，表明这个cut-point已到达。

之后新增一个KLEE的Searcher，该Searcher的算法为：
```
如果目标Def-Use pair未被覆盖
    执行BFS算法
    如果遇到Cut-point插桩函数
        则清空State列表中除了该Cut-point所在state以外的全部state。
        继续执行循环
    如果遇到该Def-Use pair的Definition插桩函数
        则更新该Def-Use pair的状态为到达定义
    如果遇到该Def-Use pair的重定义
        则更新该Def-Use pair的状态为未到达定义
    如果遇到该Def-Use pari的Use插桩函数且Def-Use pair状态为到达定义
        则更新该Def-Use pair的状态为已被覆盖
        跳出该循环
```
在KLEE初始化时让KLEE多次执行，每次目标是一对Def-Use pair