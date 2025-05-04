---
title: 編譯 LLVM 與 Clang
categories: LLVM
tags:
  - LLVM
  - Clang
abbrlink: a0c0fe90
date: 2018-04-19 02:33:29
updated: 2018-04-19 02:33:29
---

本文介紹如何編譯 LLVM 與 Clang

先從 LLVM 官網(http://releases.llvm.org/download.html#6.0.0) 下載 Clang 與 LLVM source code，使用 LLVM 6.0.0

    $ wget http://releases.llvm.org/6.0.0/llvm-6.0.0.src.tar.xz
    $ wget http://releases.llvm.org/6.0.0/cfe-6.0.0.src.tar.xz

解壓縮

    $ tar Jxvf llvm-6.0.0.src.tar.xz
    $ tar Jxvf cfe-6.0.0.src.tar.xz

開一個新資料夾用來放編譯的東西

    $ mkdir llvm-build && cd llvm-build

下 configure 指令

    $ cmake -DCMAKE_INSTALL_PREFIX="../llvm-install" \
      -DLLVM_EXTERNAL_CLANG_SOURCE_DIR="../cfe-6.0.0.src" -DCMAKE_C_COMPILER=/bin/gcc \
      -DCMAKE_CXX_COMPILER=/bin/g++ -DLLVM_BUILD_EXAMPLES=False -DLLVM_PARALLEL_LINK_JOBS=2 \
      -DLLVM_DEFAULT_TARGET_TRIPLE=aarch64-unknown-elf -DLLVM_TARGETS_TO_BUILD=AArch64 -G \
      Ninja -DLLVM_ENABLE_ASSERTIONS=Yes -DCMAKE_BUILD_TYPE=Debug -DCMAKE_C_FLAGS="-O0 -g3" \
      -DCMAKE_CXX_FLAGS="-O0 -g3" ../llvm-6.0.0.src

說明一下每個 configure 的功能

** DCMAKE_INSTALL_PREFIX **
  指定 LLVM 安裝路徑
** DLLVM_EXTERNAL_CLANG_SOURCE_DIR **
  指定 Clang source code 路徑
** DCMAKE_C_COMPILER **
  指定使用到的 C compiler 路徑
** DCMAKE_CXX_COMPILER **
  指定使用到的 C++ compiler 路徑
** DCMAKE_LLVM_EXAMPLES **
  是否要編譯 example
** DLLVM_PARALLEL_LINK_JOBS **
  設定平行連結的工作數量
** DLLVM_DEFAULT_TARGET_TRIPLE **
  指定預設的 target triple，以 aarch64-unknown-elf來說，指定指令集架構為 aarch64，vendor 為 unknown，object file format 為 elf，當使用 clang 或 llc 時，沒有給予 -mtriple 此 option時，將會用此預設 target triple
** DLLVM_TARGETS_TO_BUILD **
  指定要編譯哪一個 target，這邊指定要編譯 AArch64 此指令集架構，若沒有指定，則會將所有 target 給編譯起來，會相當耗時間
** -G Ninja **
  指定編譯工具，這邊使用 ninja 編譯工具
** DLLVM_ENABLE_ASSERTIONS **
  啟用 code assertion
** DCMAKE_C_FLAGS **
  設定編譯 LLVM c code 的額外 flags，因之後有開發 LLVM 的偵錯需求，所以優化設定為 O0，即為不做任何優化，另外使用 g3，讓編譯出來的 LLVM 有 debug information
** DCMAKE_CXX_FLAGS **
  設定編譯 LLVM c++ code 的額外 flags

最後指定 llvm source code 的路徑

開始編譯，-j30 為設定平行編譯個數，若所處硬體環境不強，可能需要耗時一個多小時以上才能夠編譯完成

    $ ninja -j30

安裝到先前設定的安裝路徑

    $ ninja install

順利的話應該可以在 llvm-install/bin 底下看到許多編好的工具

    $ cd ..
    $ ls llvm-install/bin/
    $ bugpoint      clang-cpp              git-clang-format  llvm-cfi-verify  llvm-diff       llvm-lib         llvm-mt          llvm-ranlib   llvm-stress      sancov
      c-index-test  clang-format           llc               llvm-config      llvm-dis        llvm-link        llvm-nm          llvm-rc       llvm-strings     sanstats
      clang         clang-func-mapping     lli               llvm-cov         llvm-dlltool    llvm-lto         llvm-objcopy     llvm-readelf  llvm-symbolizer  scan-build
      clang++       clang-import-test      llvm-ar           llvm-c-test      llvm-dsymutil   llvm-lto2        llvm-objdump     llvm-readobj  llvm-tblgen      scan-view
      clang-6.0     clang-offload-bundler  llvm-as           llvm-cvtres      llvm-dwarfdump  llvm-mc          llvm-opt-report  llvm-rtdyld   llvm-xray        verify-uselistorder
      clang-check   clang-refactor         llvm-bcanalyzer   llvm-cxxdump     llvm-dwp        llvm-mcmarkup    llvm-pdbutil     llvm-size     obj2yaml         yaml2obj
      clang-cl      clang-rename           llvm-cat          llvm-cxxfilt     llvm-extract    llvm-modextract  llvm-profdata    llvm-split    opt

寫個範例程式，來測試一下編好的編譯器
``` c
int add(int a, int b) {
  return a + b;
}  
```

編譯程式

    $ ./llvm-install/bin/clang -O2 -c -o add.o add.c

將程式反組譯，可看到 aarch64 的 assembly code

    $ ./llvm-install/bin/llvm-objdump -d add.o
    add.o:  file format ELF64-aarch64-little
    Disassembly of section .text:
    add:
        0:       20 00 00 0b     add     w0, w1, w0
        4:       c0 03 5f d6     ret

#### Reference
[1] [Getting Started: Building and Running Clang](https://clang.llvm.org/get_started.html)
[2] [Building LLVM with CMake](https://llvm.org/docs/CMake.html)
