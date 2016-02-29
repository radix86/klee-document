[返回主页](../README.md)
修改步骤
=========================
1、在`klee/lib/Core/UserSearcher.cpp`中，增加一个命令行参数
```
  cl::opt<std::string>
  UseDataFlow("use-data-flow-with",
  	  	   cl::desc("Use data flow search with file contents cil def-use info"));
```
之后便可以使用`-use-data-flow-with=cil-info-file-adress`来激活数据流测试模式并确定cil信息文件位置。

2、在`klee/lib/Core/UserSearcher.cpp`中，修改函数`Searcher *klee::constructUserSearcher(Executor &executor)`中，