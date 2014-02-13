#LLVM Command Guide

本文档是LLVM工具的命令描述。介绍了LLVM命令如何使用以及其选项。但并非所有选项。为了得到完整的选项列表，传递--help(普通选项)或者--help-hidden(普通和调试选项)参数给你感兴趣的工具。

##基本命令
* llvm-as - LLVM assembler 汇编器
* llvm-dis - LLVM disassembler 反汇编器
* opt - LLVM optimizer 优化器
* llc - LLVM static compiler 静态编译器
* lli - directly execute programs from LLVM bitcode 直接执行LLVM 字节码
* llvm-link - LLVM bitcode linker 字节码连接器
* llvm-ar - LLVM archiver 归档器
* llvm-nm -list LLVM bitcode and object file's symbol table 列出LLVM字节码和目标文件中的符号表
* llvm-config - Print LLVM compilation options 打印LLVM编译选项
* llvm-diff - LLVM structual 'diff' LLVM结构上的diff
* llvm-cov - emit coverage information 省略覆盖信息
* llvm-stress - generate random .ll files 生成随机的.ll文件
* llvm-symbolizer - convert addresses into source code locations 把地址值转换成源代码位置
##调试工具
* bugpoint - automatic test case reduction tool 自动测试用例下降工具
* llvm-extract - extract a function from an LLVM module 从LLVM模块中抽取一个函数
* llvm-bcanalyzer - LLVM bitcode analyzer LLVM字节码分析器
##开发者工具
* FileCheck - Flexible pattern matching file verifier 弹性模式匹配的文件验证器
* tblgen - Target Description To C++ Code Generator 目标描述到C++代码生成器
* lit - LLVM Integrated Tester LLVM集成的测试器
* llvm-build - LLVM Project Build Utility LLVM项目生成工具
* llvm-readobj - LLVM Object Reader LLVM目标文件阅读器
