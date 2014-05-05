[LLVM Language Reference Manual](http://llvm.org/docs/LangRef.html)

#LLVM 语言参考手册
* 摘要
* 简介
	* 良好的形式
* 标识符
* 高层结构
* 类型系统
* 常量(Constants)
* 其他值(Other Values)
* Module Flags Metadata
* Intrinsic Global Variables
* 指令参考(Instruction Reference)
* Intrinsic Functions

##摘要(Abstract)
这份文档是LLVM汇编语言的一个参考手册。LLVM是一个基于静态单赋值(Static Single Assignment SSA)表示提供了类型安全、低级别操作、弹性和简洁的表示所有高级语言的能力。是在LLVM编译策略中一直使用的通用代码表示。

##介绍（Introduction)
LLVM代码表示被设计用作三种形式：内存中的编译器IR、磁盘上的字节码表示(适合Just－in－time编译器快速加载)、人类可读的汇编语言表示。这就允许LLVM提供一个强力的中间表示给高效编译器转换和分析，同时提供一种自然的方式来调试和可视化转化。这三种不同的表示是等价的。这份文档描述了人类可读的表示和符号。

LLVM表示旨在表达，输入和扩展同时做到轻量级和低级别。既旨在作为一个通用IR，又足够低级使得高级主意可以很简洁的映射到它(类似微处理器是通用IR，允许很多源语言映射到它们)。通过提供类型信息，LLVM可以被用作优化的目标，举例来说，可以通过指针分析，可以证明一个C自动变量永远不会在当前函数之外被访问，允许它作为一个简单的SSA值而不是一个内存位置。

###良好的形式

主意到本文档描述了良好格式化的LLVM汇编语言是很重要的。编译器接受的和所认为的好格式之间存在差异。举例如下，下面的指令是语法合格的，但不是两好格式的：

	%x = add i32 1, %x

因为%x的定义并没有覆盖它所有的使用。

