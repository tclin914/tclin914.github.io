---
title: 編譯 GCC toolchain
categories: GCC
tags: GCC
abbrlink: cf36a3a6
date: 2017-08-08 21:48:12
updated: 2017-08-08 21:48:12
---

本文介紹如何編譯 GCC toolchain

1.  更新與升級系統
```
    $ sudo apt-get upgrade
    $ sudo apt-get update
```
2.  安裝一下預設的 GCC toolchain，待會用來編譯 GCC toolchain
```
    $ sudo apt-get build-essential
```
3.  下載與解壓縮 GCC toolchain source code，可直接改成所需版本號
```    
    $ wget http://gcc.parentingamerica.com/releases/gcc-7.1.0/gcc-7.1.0.tar.bz2
    $ tar xf gcc-7.1.0.tar.bz2
```
4.  進入 source code 目錄，並下載所需相關檔案
```
    $ cd gcc-7.1.0
    $ contrib/download_prerequisites
```
5.  建立建置用目錄，並進入此目錄，配置好 source code 目錄位置與相關設定
```
    $ mkdir build && cd build
    $ ../gcc-7.1.0/configure -v --build=x86_64-linux-gnu \
                                --host=x86_64-linux-gnu \
                                --target=x86_64-linux-gnu 
                                --prefix=/usr/local/gcc-7.1 \
                                --enable-checking=release \
                                --enable-languages=c,c++,fortran \
                                --disable-multilib \
                                --program-suffix=-7.1 
```
  設定說明
    *  --build

        指定編譯 GCC toolchain 的平台

    *  --host

        指定編譯後的 GCC toolchain 是執行在什麼平台

    *  --target

        指定編譯後的 GCC toolchain 是將 source code 編譯到什麼平台

    * --prefix

        指定編譯後的 GCC toolchain 的安裝位置

    * --program-suffix

        指定編譯後的 GCC toolchain 的執行檔結尾修飾名稱，例如：gcc-7.1，"-7.1" 即為結尾修飾名稱

6.  執行編譯流程  
```
    $ make -j 8 
```
7.  執行安裝流程
```
    $ make install
```

最後即可在安裝目錄底下使用 GCC toolchain
