[返回主页](../README.md)
KLEE 搜索器 (Searcher)
=========================
`Searcher` 是KLEE搜索算法的父类，在他之下还继承了许多子类，有
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
`update()`函数是在一个`Instruction`执行完之后，更新`ProcessTree`，将新产生的`state`加入到`ProcessTree`中