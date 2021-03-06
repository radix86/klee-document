[返回主页](../README.md)
KLEE 执行器 (Executor)
=========================
Executor是KLEE执行的入口，我们可以在`klee/lib/Core/Executor.cpp`中找到它的实现。

`void Executor::runFunctionAsMain(Function *f,int argc,char **argv,char **envp)` 这个函数可以看做是整个KLEE的入口，这个函数里面对KLEE的执行做了一些准备工作。

我们可以看这一段代码：
```
 processTree = new PTree(state);
 state->ptreeNode = processTree->root;
 run(*state);
 delete processTree;
 processTree = 0;
```
这一段函数中，首先它初始化了`processTree`，然后执行符号执行的过程，最后删除了`processTree`。
我们可以看到，最核心的一段代码就是在`run(*state)`这个函数中。


我们看一下`void Executor::run(ExecutionState &initialState)`这个函数比较关键的一段是
```
while (!states.empty() && !haltExecution) {
ExecutionState &state = searcher->selectState();
KInstruction *ki = state.pc;
stepInstruction(state);

executeInstruction(state, ki);
processTimers(&state, MaxInstructionTime);
```
这一段的意思大概是，看看是否还有没有执行的`states`，如果有，那根据不同的[Searcher](Searcher.md)找到下一个需要执行的`states`，接下来，它会存储将要执行的语句(`KInstruction *ki = state.pc;`)以及准备好下一条准备执行的语句(`stepInstruction(state);`)，然后我们执行刚才存储的语句(`executeInstruction(state, ki);`)。

我们看一下`stepInstruction(state);`函数的内部实现
```
  ++stats::instructions;
  state.prevPC = state.pc;
  ++state.pc;
```
就是将`states.pc`移到下一条`Instruction`上去。
`executeInstruction(state, ki);`是根据不同的`Instruction`进行不同的符号执行，比如说，如果是`if`语句的话，就拆分成两个`state`。

而在`updateStates(&state);`中，会调用Searcher的Update函数，去更新`states`列表，保证在`executeInstruction(state, ki);`过程中生成的新`state`能够及时的加入`states`列表。
