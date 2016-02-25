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