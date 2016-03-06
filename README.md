KLEE 文档
=======

这是分析与验证实验室针对KLEE的一份研究文档，主要记录在对KLEE进行研究和改进的过程中积累下来的一些心得和体会。

附录中是一些有帮助的论文列表。

## 目录

* ####第一部分 基本知识
    * [什么是符号执行](Basic/what-is-symbolic-execution.md)
    * [什么是KLEE](Basic/what-is-klee.md)

* ####第二部分 KLEE相关内容
    * [KLEE 执行器](About KLEE/Executor.md)
    * [KLEE 搜索器](About KLEE/Searcher.md)
    * [InstructionInfo](About KLEE/InstructionInfo.md)
    * [KLEE 插桩函数](About KLEE/SpecialFunctionHandler.md)
    
* ####第三部分 基于符号执行的数据流测试框架的实现相关内容
    * [框架目标](DataFlowFramework/target.md)
    * [设计思想](DataFlowFramework/design.md)
    * [执行步骤](DataFlowFramework/step.md)
    * [时间计划](DataFlowFramework/timetable.md)