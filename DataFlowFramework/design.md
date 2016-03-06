[返回主页](../README.md)
设计思想
=========================
总体来看该框架的实现主要分为这几个部分：1、读取Cil信息；2、构建存储和维护Data-Flow的数据结构；3、实现Cut-point guided search方法；4、优化Cut-point guided search方法。

1、读取Cil信息

在之前的研究中，我们发现目前还没有成熟的方法利用LLVM找到代码的Def-Use pairs，在经过尝试以及与擅长LLVM的工程师沟通过后，决定还是先采用Cil工具来找到代码的Def-Use pairs，然后将信息传递给KLEE，再在KLEE内部维护这些信息。

要将Cil的信息传递给KLEE需要两个步骤，第一，需要将Cil找到的Def-Use pair信息存储在文件中。第二，需要在代码中插桩标识Cut-point以及Definition和Use的位置。而KLEE在初始化时，首先读取Def-Use pair的信息结构，构建数据结构，然后在执行