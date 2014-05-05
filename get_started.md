#入门：编译和运行 Clang
此页给你最快捷径来编译和运行一些 demo 选项。可以让你运行最基本的功能。如果你喜欢你看到的，请考虑参与到 Clang 社区中来。如果遇到了问题，请提交 bug 到 LLVM Bugzilla.

## Clang 发行版本
Clang 作为常规 LLVM 的一部分来发行。你可以从 [http://llvm.org/releases/](http://llvm.org/releases/) 下载。

Clang 也随主流 BSD 或者 GNU/Linux 发行版发行，作为各自包管理系统的一部风。从 Xcode 4.2 开始，Clang 成为了 Mac OS X 的默认编译器。

## 编译 Clang and Working with the Code

### 在类 Unix 的系统上
1. 获取必须的工具
    * 参考 [Getting Started with the LLVM System - Requirements](http://llvm.org/docs/GettingStarted.html#requirements)
    * 注意运行测试套件需要Python支持。可以从 [http://www.python.org/download](http://www.python.org/download) 下载。
2. Checkout LLVM:
    * `cd` 到你想要保存 llvm 的目录
    * `svn co http://llvm.org/svn/llvm-project/llvm/trunk llvm`
3. Checkout Clang:
    * `cd llvm/tools`
    * `svn co http://llvm.org/svn/llvm-project/cfe/trunk clang`
    * `cd ../..`
4. Checkout 额外的 Clang 工具:(可选)
    * `cd llvm/tools/clang/tools`
    * `svn co http://llvm.org/svn/llvm-project/clang-tools-extra/trunk extra`
    * `cd ../../../..`
5. Checkout Compiler-RT:
    * `cd llvm/projects`
    * `svn co http://llvm.org/svn/llvm-project/compiler-rt/trunk compiler-rt`
    * `cd ../..`
6. 编译 LLVM 和 Clang:
    * `mkdri build` (为了编译的时候不污染源代码目录)
    * `cd build`
    * `../llvm/configure`
    * `make`
    * 这将编译 LLVM 和 Clang 的 Debug 版本
    * 注意:为了后续的 Clang 开发，你可以只在 clang 目录层 make
    * 也可以使用 CMake 代替 makefiles。使用 CMake 可以为不同的IDE 生成工程文件：Xcode, Eclipse CDT4, CodeBlocks, Qt-Creator(使用 CodeBlocks 产生器), KDevelop3。
7. 如果你想要使用 Clang 的 C++ 支持，你需要告诉它如何找到你的 C++ 标准库头文件。一般地，Clang 会探测 libstdc++ 可用的最好版本并使用它们-它既会找系统安装的 libstdc++，也会找随 Clang 安装的。如果你的配置不满足，你可以使用 `--with-gcc-toolchain` 配置选项来告诉 Clang gcc 包含的 libstdc++ 安装位置。
8. 尝试这些(假定你添加了 llvm/Debug+Asserts/bin 到你的 path 中):
    * `clang --help`
    * `clang file.c -fsyntax-only` (检查正确性)
    * `clang file.c -S -emit-llvm -o -` (打印未做优化的 llvm 代码)
    * `clang file.c -S -emit-llvm -o - -O3`
    * `clang file.c -S -O3 -o -` (打印出本地机器代码)

注意 C 前端 使用的是 LLVM，但是并不依赖 llvm-gcc。如果你编译 Clang 的过程中遇到了问题，确保你具有最新的 SVN 版本的 LLVM。LLVM 包含了 Clang 的支持库。

### 同时编译 Clang 和 LLVM
一旦你将 Clang 迁出到 llvm 的代码树中，它将会随着剩余的 llvm 编译。为了一次一起全部编译 Clang 和 llvm，只需要在 llvm 根目录执行 make。

注意：观察 Clang 在技术上是一个单独的 Subversion 源的一部分。如上所述，最新的 Clang 源代码绑定到了最新的 LLVM 代码树中。你可以使用 `make update` 更新顶层的 LLVM 工程(可能是无关的)和所有包含在内的工程。这将会执行 `svn update` 在所有相关联的 subversion。

### 使用 Visual Studio

## Clang 编译器驱动(Drop-in Substitute for GCC)
## Clang 使用示例

	$ cat ~/t.c
	typedef float V __attribute__((vector_size(16)));
	V foo(V a, V b) { return a+b*a; }

###预处理:

	$ clang ~/t.c -E
	# 1 "/Users/oxnz/t.c" 1
	
	typedef float V __attribute__((vector_size(16)));
	
	V foo(V a, V b) { return a+b*a; }

###类型检查:

	$ clang -fsyntax-only ~/t.c

###GCC 选项:

	$ clang -fsyntax-only ~/t.c -pedantic
	/Users/oxnz/t.c:2:17: warning: extension used
	typedef float V __attribute__((vector_size(16)));
	                ^
	1 diagnostic generated.

###Pretty printing from the AST:
注意：`-cc` 选项指示应该运行编译器前端，而不是驱动。编译器前端具有一些 Clang 特有的特性，而没有通过 GCC 兼容驱动接口导出。

	$ clang -cc1 ~/t.c -ast-print
	typedef float V __attribute__((vector_size(16)));
	V foo(V a, V b) {
		return a + b * a;
	}
	
### LLVM 代码生成：

	$ clang ~/t.c -S -emit-llvm -o -
	define <4 x float> @foo(<4 x float> %a, <4 x float> %b) {
	entry:
         %mul = mul <4 x float> %b, %a
         %add = add <4 x float> %mul, %a
         ret <4 x float> %add
	}
	$ clang -fomit-frame-pointer -O3 -S -o - t.c # On x86_64
	...
	_foo:
	Leh_func_begin1:
		mulps	%xmm0, %xmm1
		addps	%xmm1, %xmm0
		ret
	Leh_func_end1: