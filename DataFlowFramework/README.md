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

2、在`klee/lib/Core/UserSearcher.cpp`中，修改函数`Searcher *klee::constructUserSearcher(Executor &executor)`，增加自定义Searcher：DataFlowSearcher，代码如下：
```
 if(UseDataFlow != ""){
  	std::cerr << "Work with data-flow Searcher!";
  	searcher = new DataFlowSearcher(executor, UseDataFlow);
  }
```
将KLEE执行器以及cil-info文件地址传进去。

3、在`klee/lib/Core/Searcher.h`中继承`Searcher`父类，声明一个新的子类`DataFlowSearcher`，在`klee/lib/Core/Searcher.cpp`中实现`DataFlowSearcher`。目前先实现一个简单的DFS。

4、在`klee/include/klee/Internal/Module/`中新建一个`CilInfoTable.h`，再在`klee/lib/Module/`中新建一个`CilInfoTable.cpp`。

