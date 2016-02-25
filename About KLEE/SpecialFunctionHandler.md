[返回主页](../README.md)
KLEE 插桩函数 (SpecialFunctionHandler)
=========================
`SpecialFunctionHandler` 是KLEE搜索算法的父类，我们可以在`klee/lib/Core/SpecialFunctionHandler.cpp`中找到它的实现，在他之下还继承了许多子类，有
`DFSSearcher`
`BFSSearcher`
`RandomSearcher`
`WeightedRandomSearcher`
`RandomPathSearcher`
`MergingSearcher`
`BumpMergingSearcher`
`BatchingSearcher`
`IterativeDeepeningTimeSearcher`
`InterleavedSearcher`

`Searcher`父类之中，比较关键的是两个个函数
```
virtual ExecutionState &selectState() = 0;

virtual void update(ExecutionState *current,
                    const std::set<ExecutionState*> &addedStates,
                    const std::set<ExecutionState*> &removedStates) = 0;
```
`selectState()`函数是`Searcher`核心的算法，主要是根据搜索策略来选择下一个`state`。
`update()`函数是在一个`Instruction`执行完之后，更新`states`列表，将新产生的`state`加入到`states`列表中。在`Executor`中的`updateStates(&state);`函数会用到。