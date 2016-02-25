[返回主页](../README.md)
KLEE 插桩函数 (SpecialFunctionHandler)
=========================
`SpecialFunctionHandler` 是KLEE 中插桩函数，我们可以在`klee/lib/Core/SpecialFunctionHandler.cpp`中找到它的实现。
在这个文件中
```
  add("calloc", handleCalloc, true),
  add("free", handleFree, false),
  add("klee_assume", handleAssume, false),
  add("klee_check_memory_access", handleCheckMemoryAccess, false),
  add("klee_get_valuef", handleGetValue, true),
  add("klee_get_valued", handleGetValue, true),
  add("klee_get_valuel", handleGetValue, true),
  add("klee_get_valuell", handleGetValue, true),
  add("klee_get_value_i32", handleGetValue, true),
  add("klee_get_value_i64", handleGetValue, true),
  add("klee_make_symbolic", handleMakeSymbolic, false),
```
类似这样的定义，就是在声明插桩函数，比如说`klee_make_symbolic`这个函数，就是通过` add("klee_make_symbolic", handleMakeSymbolic, false)`这一句语句将`handleMakeSymbolic`控制器与`klee_make_symbolic`插桩函数绑定在了一起。

比如，当KLEE符号执行到插入到被测代码中的插桩函数`klee_make_symbolic()`时，便会执行`void SpecialFunctionHandler::handleMakeSymbolic(ExecutionState &state,KInstruction *target,std::vector<ref<Expr> > &arguments)`这个函数里面的内容，其中`arguments`可以获取到传入插桩函数里面的参数，`state`可以获取到执行到该函数时的`state`,`target`可以获取到该函数所在的`KInstruction`。

-------
###新增一个插桩函数

在`klee/lib/Core/SpecialFunctionHandler.h`中，增加一个`HANDLER(handleMakeSymbolic);`参数对应的是插桩函数控制器的名字，`HANDLER(handleMakeSymbolic);`相当于声明了一个插桩函数控制器。

在`klee/lib/Core/SpecialFunctionHandler.cpp`中，类比`void SpecialFunctionHandler::handleMakeSymbolic(ExecutionState &state,KInstruction *target,std::vector<ref<Expr> > &arguments)`实现该控制器，并通过`add("klee_make_symbolic", handleMakeSymbolic, false),`绑定函数名与控制器，在`klee/include/klee/klee.h`中声明插桩函数，便可以在被测代码中使用插桩函数。

未解决的问题：使用时无实际问题，但会出现`KLEE: WARNING: undefined reference to function: xxxxx`，暂不知道原因，但不影响使用。