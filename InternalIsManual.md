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
这个库明显需要一个更好的名称。基本库包含了一系列底层实用功能来跟踪和操作 source buffers，source buffer 内部位置，诊断信息，关键字，目标抽象和要编译的语言的子集信息。

这个架构的一部分是针对 C的（例如 `TargetInfo` 类），其他部分可以被重用于非基于 C 的语言（`SourceLocation`, `SourceManager`, `Diagnostics`, `FileManager`）。当将来需要的时候，我们可以看出引进一个新库，把一些 general 类移到其它地方，或者引入其他的解决方案是否有意义。

我们实用依赖顺序来描述这些类。

###诊断子系统

Clang 诊断子系统是编译器和人交互的一个重要部分。诊断是那些在代码不正确或者有歧义的时候产生的警告和错误信息。在 Clang 中，每条诊断信息都具有唯一的（至少）ID，一个关联的英语翻译，一个指示放^标记的`SourceLocation`，和一个重要级别（例如是`WARNING`或者`ERROR`）。它们也可以包含一系列参数给诊断(将填充串中的"%0")和一系列诊断相关的代码范围。

在这一节中，我们将会给出一些有 Clang 命令行驱动产生的例子，但是诊断信息可以根据`DiagnosticClient`接口的实现而具有不同的渲染方式。一个具有代表新的诊断信息：

	t.c:38:15: error: invalid operands to binary 	expression ('int *' and '_Complex float')
	P = (P-42) + Gamma*4;
    	~~~~~~ ^ ~~~~~~~

在这个例子中，你可以看到英文翻译，级别(error)，可以看到源位置(`^`号和文件/行/列信息)，源码范围"~~~~",诊断参数(`"int *"` 和 `"_Complex float"`)。你需要相信我，这个诊断背后有个唯一的 ID :)。

让所有这些发生需要几个步骤并牵扯到很多移动，这一节介绍了它们并且讨论了添加一个新的诊断的最佳实践。

####`Diagnostic*Kinds.td` 文件

诊断信息的创建通过添加入口到`clang/Basic/Diagnostic*Kinds.td`文件，取决于什么样的库将会用它。从这个文件，`tblgen`产生诊断的唯一 ID、级别和从格式化字符串产生英语翻译。

目前唯一的 ID 的命名中还有一些手段(sanity)。有些以`err_`, `warn_`, `ext_`开始来表明级别。由于枚举要引用到 C++代码来生成诊断信息，所以它相对较短就显得比较有意义。

严重程度来源于集合`{NOTE, REMARK, WARNING, EXTENSION, EXTWARN, ERROR}`。其中`ERROR`用于指示无论在何种情况下程序都是无法接受的。当错误发生时，输入程序的AST可能还没有完整生成。`EXTENSION`和`EXTWARN`用于Clang 接受的语言扩展。这意味着 Clang 可以完全理解并在 AST 中表示，但是我们还是生成诊断信息告诉用户它们的代码是不可移植的。其中的区别是前者默认被忽略，后者默认会警告。`WARNING`用于那些在当前源码语言中可以有效构建但是在某些情况下可能存在歧义。`REMARK`提供了与具体歧义无关的通用信息。`NOTE`用于给前边的诊断信息添加更多信息。

这些诊断级别被基于各种配置的诊断子系统映射为一个更小的输出级别集合(就是`Diagnostic::Level`枚举， {`Ignored`, `Note`, `Remark`, `Warning`, `Error`, `Fatal`})。Clang 内部支持一个完善的分层映射机制可以允许你映射几乎所有的诊断到你想要的输出级别。唯一不可被映射的就是`NOTE`S，它总是紧跟着前边的诊断和`ERROR`S,其中`ERROR`只能被映射为`Fatal`(例如不可能把错误变成警告)。

诊断映射具有多种使用方式。例如，如果用户指定了`-pedantic`, `EXTENSION`就被映射为`Warning`,如果指定了`-pedantic-errors`, 就被映射为`Error`。这被用来实现像`-Wunused_macros`, `-Wundef`等等选项。

映射为`Fatal`只应当在用于在错误严重到错误恢复无法修复的时候(这样会生成大量的伪错误)。这类例子之一就是`#include`一个文件失败。

####格式字符串(The Format String)

诊断信息的格式字符串非常简单有力。它是用了包含指示传给诊断的参数何处插入和如何格式化的标记的英文字符串。例如下面的一些简单格式字符串:

	"binary integer literals are an extension"
	"format string contains '\\0' within the string body"
	"more '%%' conversions than data arguments"
	"invalid operands to binary expression (%0 and %1)"
	"overloaded '%0' must be a %select{unary|binary|unary 	or binary}2 operator"
    	 " (has %1 parameter%s1)"

这些例子展示了格式化字符串的某些重要性。你可以使用普通的 ASCII 字符串，除了`%`，但是这些事 C 字符串，所以你要像上边第二个例子一样使用并且小心 escape sequences。如果你想生成一个`%`，使用`%%`escape sequence，像上例中第三个一样。最终，Clang 使用`%...[digit]`序列来指定诊断参数的位置和如何格式化。

