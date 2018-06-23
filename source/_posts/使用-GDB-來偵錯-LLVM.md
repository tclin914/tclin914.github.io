---
title: 使用 GDB 來偵錯 LLVM
categories: GDB
tags:
  - GDB
  - Clang
abbrlink: 8eaa3f8e
date: 2018-04-20 07:43:51
updated: 2018-04-20 07:43:51
---


這邊紀錄一下如何使用 GDB 對 LLVM(Low Level Virtual Machine) 進行偵錯，LLVM 前端為 Clang，若我們在下編譯參數時，希望 Clang 直接產生 object file 或是 executable file，Clang 在處理完前端資訊後，會接著驅動 opt(LLVM Optimizer) 進行優化，驅動 llc(LLVM static compiler) 產生目的碼，opt 與 llc 皆為獨立的程式，Clang 會呼叫起此兩個程式來進行剩餘的編譯工作，因此若將 break point 設在 opt 或 llc 所屬的程式碼時，會遇到無法停止的情況，本文紀錄如何解決此情況。

首先記得在編譯 LLVM 時，在 configure 加上 `-DCMAKE_C_FLAGS="-O0 -g3"` 和 `-DCMAKE_CXX_FLAGS="-O0 -g3"`，讓在編譯 LLVM 過程能夠加上 Debug information，可參考此篇如何編譯 LLVM 和 Clang [編譯 LLVM 與 Clang](https://tclin914.github.io/%E7%B7%A8%E8%AD%AF-LLVM-%E8%88%87-Clang.html)。

寫個範例程式，讓我們能夠在編譯此範例程式的過程中，對編譯器偵錯。
``` c
int shift_and(int a) {
  int b = a >> 12;
  return b & 0xfff;
}
```

編譯此程式，並在過程中使用 GDB 來偵錯。

    $ gdb --args ./llvm-build/bin/clang -O2 -c -o shift_and.o shift_and.c
    GNU gdb (GDB) Fedora 7.12.1-48.fc25
    Copyright (C) 2017 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
    and "show warranty" for details.
    This GDB was configured as "x86_64-redhat-linux-gnu".
    Type "show configuration" for configuration details.
    For bug reporting instructions, please see:
    <http://www.gnu.org/software/gdb/bugs/>.
    Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.
    For help, type "help".
    Type "apropos word" to search for commands related to "word"...
    Reading symbols from ./llvm-build/bin/clang...done.
    (gdb)

假設我們想要停在 lib/Target/AArch64/AArch64ISelDAGToDAG.cpp的2707行。

    (gdb) b AArch64ISelDAGToDAG.cpp:2707
    Breakpoint 1 at 0x1940ecd: file /home/users/jim/llvm/llvm-6.0.0.src/lib/Target/AArch64/AArch64ISelDAGToDAG.cpp, line 2707.

讓程式開始執行，會發現因為使用 vfork 產生 child process的關係，gdb 此時無法針對產生出的 child process偵錯，會直接結束離開。

    (gdb) run
    Starting program: /home/users/jim/llvm/llvm-build/bin/clang -O2 -c -o shift_and.o shift_and.c
    Missing separate debuginfos, use: dnf debuginfo-install glibc-2.24-10.fc25.x86_64
    [Thread debugging using libthread_db enabled]
    Using host libthread_db library "/lib64/libthread_db.so.1".
    Detaching after vfork from child process 16302.
    [Inferior 1 (process 16298) exited normally]
    Missing separate debuginfos, use: dnf debuginfo-install libgcc-6.4.1-1.fc25.x86_64 libstdc++-6.4.1-1.fc25.x86_64 ncurses-libs-6.0-6.20160709.fc25.x86_64 zlib-1.2.8-10.fc24.x86_64

解決方法即為在執行程式之前，先設定好 `set follow-fork-mode child`，可看到順利地停在 AArch64ISelDAGToDAG.cpp:2707了。

    $ gdb --args ./llvm-build/bin/clang -O2 -c -o shift_and.o shift_and.c
    GNU gdb (GDB) Fedora 7.12.1-48.fc25
    Copyright (C) 2017 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
    and "show warranty" for details.
    This GDB was configured as "x86_64-redhat-linux-gnu".
    Type "show configuration" for configuration details.
    For bug reporting instructions, please see:
    <http://www.gnu.org/software/gdb/bugs/>.
    Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.
    For help, type "help".
    Type "apropos word" to search for commands related to "word"...
    Reading symbols from ./llvm-build/bin/clang...done.
    (gdb) b AArch64ISelDAGToDAG.cpp:2707
    Breakpoint 1 at 0x1940ecd: file /home/users/jim/llvm/llvm-6.0.0.src/lib/Target/AArch64/AArch64ISelDAGToDAG.cpp, line 2707.
    (gdb) set follow-fork-mode child
    (gdb) run
    Starting program: /home/users/jim/llvm/llvm-build/bin/clang -O2 -c -o shift_and.o shift_and.c
    Missing separate debuginfos, use: dnf debuginfo-install glibc-2.24-10.fc25.x86_64
    [Thread debugging using libthread_db enabled]
    Using host libthread_db library "/lib64/libthread_db.so.1".
    [New process 16732]
    [Thread debugging using libthread_db enabled]
    Using host libthread_db library "/lib64/libthread_db.so.1".
    process 16732 is executing new program: /home/users/jim/llvm/llvm-build/bin/clang-6.0
    Missing separate debuginfos, use: dnf debuginfo-install libgcc-6.4.1-1.fc25.x86_64 libstdc++-6.4.1-1.fc25.x86_64 ncurses-libs-6.0-6.20160709.fc25.x86_64 zlib-1.2.8-10.fc24.x86_64
    [Thread debugging using libthread_db enabled]
    Using host libthread_db library "/lib64/libthread_db.so.1".
    [Switching to Thread 0x7ffff7fc5600 (LWP 16732)]

    Thread 2.1 "clang-6.0" hit Breakpoint 1, (anonymous namespace)::AArch64DAGToDAGISel::Select (this=0x9fe0190, Node=0xa0923a0)
    at /home/users/jim/llvm/llvm-6.0.0.src/lib/Target/AArch64/AArch64ISelDAGToDAG.cpp:2707
    2707        if (tryBitfieldExtractOp(Node))
    Missing separate debuginfos, use: dnf debuginfo-install glibc-2.24-10.fc25.x86_64 libgcc-6.4.1-1.fc25.x86_64 libstdc++-6.4.1-1.fc25.x86_64 ncurses-libs-6.0-6.20160709.fc25.x86_64 zlib-1.2.8-10.fc24.x86_64
    (gdb)
