[返回主页](../README.md)
设计思想
=========================
总体来看该框架的实现主要分为这几个部分：1、读取Cil信息；2、构建存储和维护Data-Flow的数据结构；3、实现Cut-point guided search方法；4、优化Cut-point guided search方法。

1、读取Cil信息

在之前的研究中，我们发现目前还没有成熟的方法利用LLVM找到代码的Def-Use pairs，在经过尝试以及与擅长LLVM的工程师沟通过后，决定还是先采用Cil工具来找到代码的Def-Use pairs，然后将信息传递给KLEE，再在KLEE内部维护这些信息。