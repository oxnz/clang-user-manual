[LLVM Language Reference Manual](http://llvm.org/docs/LangRef.html)

#LLVM 语言参考手册
* 摘要
* 简介
	* 良好的形式
* 标识符
* 高层结构
	* 模块结构
	* 连接类型
	* 调用转换(Calling Convertions)
	* Visibility Styles
	* DLL Storage Classes
	* 线程本地存储模型(Thread Local Storage Models)
	* 结构类型
	* 全局变量
	* 函数
	* 别名
	* Comdats
	* Named Metadata
	* 参数属性
	* 垃圾回收器名
	* Prefix Data
	* Prologue Data
	* 属性组
	* 函数属性
	* 模块级别内联汇编
	* 数据布局(Data Layout)
	* Target Triple
	* Pointer Aliasing Rules
	* Volatile Memory Access
	* 并行操作的内存模型
	* Atomic Memory Ordering Constraints
	* Fast-Math Flags
	* Use-list Order Directives
* 类型系统
	* Void 类型
	* Function 类型
	* First Class Type
		* 单值类型(Single Value Types)
			* 整数类型
			* 浮点数类型
			* X86_mmx 类型
			* 指针类型
			* 向量(Vector)类型
		* 标签(Label)类型
		* Metadata 类型
		* 集合(Aggregate)类型
			* 数组类型
			* 结构(Structure)类型
			* Opaque Structure Types
* 常量(Constants)
	* 简单常量
	* Complex 常量
	* 全局变量和函数地址
	* Undefined Values
	* Poison Values
	* 基本块地址(Address of Basic Blocks)
	* 常量表达式(Constant Expressions)
* 其他值(Other Values)
	* 内联汇编表达式
		* Inline Asm Metadata
	* Metadata Nodes and Metadata Strings
		* tbaa
		* tbaa.struct
		* noalias and alias.scope
		* fpmath
		* range
		* llvm.loop
		* llvm.loop.vectorize and llvm.loop.interleave
		* llvm.loop.interleave.count
		* llvm.loop.vectorize.enable
		* llvm.loop.vectorize.width
		* llvm.loop.unroll
		* llvm.loop.unroll.count
		* llvm.loop.unroll.disable
		* llvm.loop.unroll.full
		* llvm.mem
		* llvm.mem.parallel_loop_access
* Module Flags Metadata
	* Objective-C Garbage Collection Module Flags Metadata
	* Automatic Linker Flags Module Flags Metadata
	* C type width Module Flags Metadata
* Intrinsic Global Variables
	* llvm.used
	* llvm.compiler.used
	* llvm.global_ctors
	* llvm.global_dtors
* 指令参考(Instruction Reference)
	* 终结符指令
	* 二元操作
		* add
		* fadd
		* sub
		* fsub
		* mul
		* fmul
		* udiv
		* sdiv
		* fdiv
		* urem
		* srem
		* frem
	* 位二元操作
		* shl
		* lshr
		* ashr
		* and
		* or
		* xor
	* 向量操作
		* extractelement
		* insertelement
		* shufflevector
	* 集合操作(Aggregate Operations)
		* extractvalue
		* insertvalue
	* 内存操作和地址操作
		* alloca
		* load
		* store
		* fence
		* cmpxchg
		* atomicrmw
		* getelementptr
	* 转换操作(Convertion Operations)
		* trunc ... to
		* zext ... to
		* sext ... to
		* fptrunc ... to
		* fpext ... to
		* fptosi ... to
		* uitofp ... to
		* sitofp ... to
		* ptrtoint ... to
		* inttoptr ... to
		* bitcase ... to
		* addrspacecast ... to
	* 其他操作
		* icmp
		* fcmp
		* phi
		* select
		* call
		* va_arg
		* landingpad
* Intrinsic Functions
	* Variable Argument Handling Intrinsics
		* llvm.va_start
		* llvm.va_end
		* llvm.va_copy
	* Accurate Garbage Collection Intrinsics
		* llvm.gcroot
		* llvm.gcread
		* llvm.gcwrite
	* Code Generator Intrinsics
		* llvm.returnaddress
		* llvm.frameaddress
		* llvm.read_register and llvm.write_register
		* llvm.stackrestore
		* llvm.prefetch
		* llvm.pcmarker
		* llvm.readcyclecounter
		* llvm.clear_cache
	* Standard C Library Intrinsics
		* llvm.memcpy
		* llvm.memmove
		* llvm.memset
		* llvm.sqrt.*
		* llvm.powi.*
		* llvm.sin.*
		* llvm.cos.*
		* llvm.pow.*
		* llvm.exp.*
		* llvm.exp2.*
		* llvm.log.*
		* llvm.log10.*
		* llvm.log2.*
		* llvm.fma.*
		* llvm.fabs.*
		* llvm.minnum.*
		* llvm.maxnum.*
		* llvm.copysign.*
		* llvm.floor.*
		* llvm.ceil.*
		* llvm.trunc.*
		* llvm.rint.*
		* llvm.nearbyint.*
		* llvm.round.*
	* Bit Manipulation Intrinsics
		* llvm.bswap.*
		* llvm.ctpop.*
		* llvm.ctlz.*
		* llvm.cttz.*
	* Arithmetic with Overflow Intrinsics
		* llvm.sadd.with.overflow.*
		* llvm.uadd.with.overflow.*
		* llvm.ssub.with.overflow.*
		* llvm.usub.with.overflow.*
		* llvm.smul.with.overflow.*
		* llvm.umul.with.overflow.*
	* Specialised Arithmetic Intrinsics
		* llvm.fmuladd
	* Half Precision Floating Point Intrinsics
		* llvm.convert.to.fp16
		* llvm.convert.from.fp16
	* Debugger Intrinsics
	* Exception Handling Intrinsics
	* Trampoline Intrinsics
		* llvm.init.trampoline
		* llvm.adjust.trampoline
	* 内存使用标识(Memory Use Marker)
		* llvm.lifetime.start
		* llvm.lifetime.end
		* llvm.invariant.start
		* llvm.invariant.end
	* General Intrinsics
		* llvm.var.annotation
		* llvm.ptr.annotation.*
		* llvm.annotation.*
		* llvm.trap
		* llvm.debugtrap
		* llvm.stackprotector
		* llvm.stackprotectorcheck
		* llvm.objectsize
		* llvm.expect
		* llvm.assume
		* llvm.donothing
	* Stack Map Intrinsics

##摘要(Abstract)
这份文档是LLVM汇编语言的一个参考手册。LLVM是一个基于静态单赋值(Static Single Assignment SSA)表示提供了类型安全、低级别操作、弹性和简洁的表示所有高级语言的能力。是在LLVM编译策略中一直使用的通用代码表示。

##介绍（Introduction)
LLVM代码表示被设计用作三种形式：内存中的编译器IR、磁盘上的字节码表示(适合Just－in－time编译器快速加载)、人类可读的汇编语言表示。这就允许LLVM提供一个强力的中间表示给高效编译器转换和分析，同时提供一种自然的方式来调试和可视化转化。这三种不同的表示是等价的。这份文档描述了人类可读的表示和符号。

LLVM表示旨在表达，输入和扩展同时做到轻量级和低级别。既旨在作为一个通用IR，又足够低级使得高级主意可以很简洁的映射到它(类似微处理器是通用IR，允许很多源语言映射到它们)。通过提供类型信息，LLVM可以被用作优化的目标，举例来说，可以通过指针分析，可以证明一个C自动变量永远不会在当前函数之外被访问，允许它作为一个简单的SSA值而不是一个内存位置。

###良好的形式

主意到本文档描述了良好格式化的LLVM汇编语言是很重要的。编译器接受的和所认为的好格式之间存在差异。举例如下，下面的指令是语法合格的，但不是两好格式的：

	%x = add i32 1, %x

因为%x的定义并没有覆盖它所有的使用。

