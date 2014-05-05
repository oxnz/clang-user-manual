#打包 LLVM 的建议

##概览

LLVM 设置了特定的默认配置选项来确保开发者在受限的平台上不会搞破坏。这些设置对于桌面系统来说不是最佳的，所以我们希望打包者(packagers)（e.g., Redhat, Debian, MacPorts, etc.）来修改它们。此文列出了我们希望你改变的设置。

LLVM 的 API 在每一个版本中都有改变，所以用户可能会希望同时安装例如 LLVM-2.6 和 LLVM-2.7 来支持为各版本开发的应用。

##编译参数

在优化并去除断言之后，LLVM 运行的会更快。然而这样的编译与未定义 NDEBUG 的用户并不兼容，并且由于缺少断言使得调试用户代码中的问题更加困难。我们建议用户同时安装优化的和调试版本的 LLVM。下面给出相关的configure flags:

`--disable-assertions`

定义 NDEBUG 编译 LLVM。改变了 LLVM 的 ABI。也可是设置make环境变量 `DISABLE_ASSERTIONS=0|1` 。这默认会忽略优化设置，但是会降低速度。

`--enable-debug-symbols`

使用 `-g` 选项编译 LLVM。也可以通过改变 make 的环境变量 `DEBUG_SYMBOLS=0|1` 来改变。优化时默认是关闭的，所以你应当打开它来使得用户可以调试它们的程序。

`--enable-optimized`

（对于svn checkouts）使用 -O2 来编译 LLVM，并且默认关闭调试符号。也可以通过改变make的环境变量`也可以通过改变make的环境变量`来改变。在非 checkout 中默认是打开的。

##C++特性

###RTTI

LLVM 默认关闭了 RTTI。添加 `REQUIRES_RTTI=1` 到 make 的环境变量中来打开它。这将允许用户编译带有 RTTI 的并且从 LLVM 继承的类。

##共享库

使用 `--enable-shared` 来配置来编译 `libLLVM-<major>.<minor>.(so|dylib)` 并且给工具来链接。这将会用一些打开时间来换取二进制文件的大大减小。

##依赖关系

`--enable-libffi`

依赖于 [libffi](http://sources.redhat.com/libffi/) 来使得 LLVM 解释器可以调用外部函数。

`--with-oprofile`

依赖于 [libopagent](http://oprofile.sourceforge.net/doc/devel/index.html)(>= version 0.9.4) 来使得 LLVM JIT 告诉 oprofile 关于函数地址和行号。
