#LLVM 词典
##定义
**A**

__ADCE__

Aggressive Dead Code Elimination
积极的死代码消除

__AST__

Abstract Syntax Tree
抽象语法树

由于 Clang 的影响（大多是由于 C，尤其是 C++ 错综复杂的解析和语义分析），典型的 LLVM 社区中的 AST 的定义大概是“输入程序的编译器第一个完整的符号（与文本相反）表示”。因此，一个 “AST” 可能更像一个通用图而不是一棵“树”（考虑一个链表节点的符号表示）。这个工作的定义更接近一些作者所谓的“带注释的抽象语法树”。

使用你最喜欢的编译器书籍和搜索引擎来获取更多信息。
<hr>
**B**

__BB Vectorization__

Basic-Block Vectorization
基本块向量化

__BURS__

Bottom Up Rewriting System — A method of instruction selection for code generation.

自底向上重写系统－一种代码生成中的指令选择方法。一个例子是 [BURG](http://www.program-transformation.org/Transform/BURG) 工具。
<hr>

**C**

__CSE__

Common Subexpression Elimination.

共同子表达式消除。一种移除共同子表达式的优化。例如 `(a+b)*(a+b)` 有两个相同的子表达式: `(a+b)`。这个优化使得只做一次加法然后执行乘法（但是只针对正确且安全的计算）。
<hr>

**D**

__DAG__

Directed Acyclic Graph

有向不循环图

__Derived Pointer__

一个指向对象内部的指针，使得垃圾回收器不能使用此指针进行可达性分析。只要一个 derived 指针存在，对应的对象的指针必须保留在 root 内，否则垃圾回收器可能会释放引用的对象。对于拷贝收集器，derived 指针具有额外的风险使得它们可能在任何安全点处无效化。这个机制被用来对抗对象指针。

__DSA__

数据结构分析

__DSE__

Dead Store Elimination

可不达存储消除

<hr>

**F**

__FCA__

First Class Aggregate

第一类集合
<hr>

**G**

__GC__

垃圾回收。使用可达性分析替代显式的内存管理来回首不再使用的内存。

<hr>

**H**

__Heap__

在垃圾回收中，使用可达性分析管理的内存区域

<hr>

**I**

__IPA__

Inter-Procedural Analysis.

过程间分析。引用到过程、函数和编译单元（模块）之间发生的大量代码分析。

__IPO__

过程间优化。引用到过程、函数和编译单元（模块）之间发生的大量代码优化。

__ISel__

指令选择

<hr>

**L**

__LCSSA__

Loop-Closed Static Single Assignment Form

闭环静态单赋值形式

__LICM__

Loop Invariant Code Motion

循环不变量


__Load-VN__

负荷值编号

__LTO__

链接时优化
<hr>

**M**

__MC__

机器码

<hr>

**O**

__Object Pointer__

指向对象的指针，使得垃圾回收器可以追踪对象内的引用。这个机制用来对抗derived 指针。
<hr>

**P**

__PRE__

Partial Redundancy Elimination

部分冗余消除

<hr>

**R**

__RAUW__

Replace All Uses With. The functions User::replaceUsesOfWith(), Value::replaceAllUsesWith(), and Constant::replaceUsesOfWithOnConstant() implement the replacement of one Value with another by iterating over its def/use chain and fixing up all of the pointers to point to the new value. See also def/use chains.

__Reassociation__

Rearranging associative expressions to promote better redundancy elimination and other optimization. For example, changing (A+B-A) into (B+A-A), permitting it to be optimized into (B+0) then (B).

__Root__

在垃圾回收中，堆中的一个指针变量，垃圾回收器从这个指针开始可达性分析。在代码生成的上下文中，“root”几乎总是指向“stack root”-一个函数调用过程中的局部或临时变量。

__RPO__

Reverse postorder

<hr>

**S**

__Safe Point__

在垃圾回收中，必须标识栈根使得可达性分析能够执行。对于每条指令提供这个信息可能变得不可能，所以这个信息只在指定的安全点计算。使用拷贝收集器，derived 指针绝对不可以跨安全点并且对象指针必须从栈根重新加载。

__SDISel__

Selection DAG Instruction Selection.

DAG 指令选择

__SCC__

Strongly Connected Component

强链接组件

__SCCP__

Sparse Conditional Constant Propagation

稀疏的条件常量传播

__SLP__

Superword-Level Parallelism, 类似 [Basic-Block Vectorization](#).

__SRoA__

Scalar Replacement of Aggregates

聚合标量替换

__SSA__

Static Single Assignment

静态单赋值

__Stack Map__

在垃圾回收中，由代码生成器发出的元数据在一个函数调用的栈帧中标识了roots。

<hr>

**T**

__TBAA__

Type-Based Alias Analysis

基于类型的别名分析