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
        assemblyLine(_assemblyLine) {
    }
  };
```
我们可以看出，在`InstructionInfo`之中，存储了`Instruction`的`id`、所在的文件路径、所在文件的行号，最后还有在LLVM IR中的行号。

下面我们再来看一下`InstructionInfoTable`
```
  class InstructionInfoTable {
    struct ltstr { 
      bool operator()(const std::string *a, const std::string *b) const {
        return *a<*b;
      }
    };

    std::string dummyString;
    InstructionInfo dummyInfo;
    std::map<const llvm::Instruction*, InstructionInfo> infos;
    std::set<const std::string *, ltstr> internedStrings;

  private:
    const std::string *internString(std::string s);
    bool getInstructionDebugInfo(const llvm::Instruction *I,
                                 const std::string *&File,
								 unsigned &Line,
								 unsigned &Column);

  public:
    InstructionInfoTable(llvm::Module *m);
    ~InstructionInfoTable();

    unsigned getMaxID() const;
    const InstructionInfo &getInfo(const llvm::Instruction*) const;
    const InstructionInfo &getFunctionInfo(const llvm::Function*) const;
  };

}
```
在`getInstructionDebugInfo()`这个函数中`InstructionInfoTable`根据`Instruction`获取`Instruction`的debug信息，比如所在文件，行号等。