[返回主页](../README.md)
InstructionInfo (对LLVM Instruction的信息）
=========================
InstructionInfo 是 KLEE对LLVM Instruction的封装之后获得的信息，我们可以在`klee/include/klee/Internal/Module/InstructionInfoTable.h`中找到它的声明，在`klee/lib/Module/InstructionInfoTable.cpp`中找到`InstructionInfoTable`的实现。

我们可以看一看`InstructionInfo`的结构：
```
  struct InstructionInfo {
    unsigned id;
    const std::string &file;
    unsigned line;
    unsigned Column;
    unsigned assemblyLine;

  public:
    InstructionInfo(unsigned _id,
                    const std::string &_file,
                    unsigned _line,
					unsigned _Column,
                    unsigned _assemblyLine)
      : id(_id), 
        file(_file),
        line(_line),
		Column(_Column),
        assemblyLine(_assemblyLine) {
    }
  };
```
