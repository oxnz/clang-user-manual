#"Clang" CFE Internals Manual
* 介绍
* LLVM 支持库
* Clang 基本库
  * 诊断子系统
    * Diagnostic*Kinds.td 文件
    * 格式化串
    * 格式化一个诊断参数
    * 生成诊断信息
    * Fix-It 提示
    * DiagnosticClient 接口
    * 为 Clang 添加翻译
  * SourceLocation 和 SourceManager 类
  * SourceRange 和 CharSourceRange
* 驱动库
* 预编译的头文件
* 前端库
* Lexer 和 预处理器库
  * Token 类
  * Annotation 关键字
  * Lexer 类
  * TokenLexer 类
  * MultipleIncludeOpt 类
* Parser 库
* AST 库
  * Type 类及其子类
    * Canonical 类型
  * QualType 类
  * 声明名称
  * 声明上下文
    * 重声明(Redeclarations)和重载(Overloads)
    * 词法和语义上下文
    * 透明声明上下文(Transparent Declaration Contexts)
    * 多重定义声明上下文(Multiply-Defined Declarations Contexts)
  * CFG 类
    * 基本块
    * 入口和出口块
    * 条件控制流
  * Clang AST 中的常量折叠
    * 实现方法
    * 扩展
* 如何改变 Clang
  * 如何添加属性
    * 属性基础
    * `include/clang/Basic/Attr.td`
    * 引用
    * The meat of your attribute
  * 如何添加表达式和 statement


## 介绍
这份文件描述了 Clang C 前端所做的一些更重要的API和内部设计决策。此文的目的在于既抓住高层信息，又描述了其背后所做的的设计决策。也就表明了此文面向对修改 Clang 感兴趣的人，而不是终端用户。下面描述按照库分类，并不描述任何库的用户。

## LLVM 支持库
LLVM `libsupport` 库提供了许多底层库(underlying libraries) 和 数据结构，包括了命令行选项的处理，各种容器(various containers) 和 一个用于访问文件系统系统抽象层。

## Clang "Basic" 库
这个库明显需要一个更好的名称。