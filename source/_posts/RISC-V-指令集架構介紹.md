---
title: RISC-V 指令集架構介紹
categories: RISC-V
tags: RISC-V
abbrlink: 3ad6268d
date: 2018-04-01 15:40:37
---


RISC-V 是一個開放指令集架構，任何人可基於此架構實作，基本指令長度為32-bit，設計成容易擴充與客製化的指令集架構，基本指令集架構大約只有一百多道指令，有32-bit(RV32)，64-bit(RV64)和 128-bit(RV128)三種不同的暫存器長度，與模組化的擴充指令集。

#### RISC-V 指令集有以下四種基本指令集架構：
* **RV32I Base Integer Instruction Set** 
    32-bit 基本整數指令集，有 32個 32-bit暫存器。
* **RV32E Base Integer Instruction Set**
    32-bit 嵌入式基本整數指令集，與 RV32I的差異為暫存器減少成只有 16個 32-bit暫存器。
* **RV64I Base Integer Instruction Set**
    64-bit 基本整數指令集，有 32個 64-bit暫存器。
* **RV128I Base Integer Instruction Set**
    128-bit 基本整數指令集，有 32個 128-bit暫存器。

#### 標準擴充指令集(Standard Extension)：
* **"M" Standard Extension for Integer Multiplication and Divison**
    新增了整數乘法除法指令。
* **"A" Standard Extension for Atomic Instructions**
    新增了 Atomic相關的指令。
* **"F" Standard Extension for Single-Precision Floating-Point**
    新增了 32個 32-bit的 floating-point暫存器(f0-f31)與其相關的指令。
* **"D" Standard Extension for Double-Precision Floating-Point**
    將 32個 floating-point暫存器(f0-f31)擴大成 64-bit與新增其相關的指令。
* **"Q" Standard Extension for Quard-Precision Floating-point**
    將 32個 floating-point暫存器(f0-f31)擴大成 128-bit與新增其相關的指令。
* **"C" Standard Extension for Compressed Instruction**
    新增了 16-bit長度的指令，用來替換掉常用到的 32-bit長度的指令，可減少 code size。
* **"G"** 
    為 I，M，A，F和D 的組合(IMAFD)。

還有一些已保留其名字，但尚未詳細制定指令的標準擴充指令集，例："L"，"B"，"J"，"T"，"P"，"V" 和 "N"

可以把任一基本指令集加上一個或多個擴充指令集使用，例如：
* **RV32IMA**
    即為 32-bit基本整數指令集加上整數乘法除法指令與 atomic相關指令。
* **RV64IMAFD** 
    即為 64-bit基本整數指令集加上整數乘法除法指令，atomic相關指令，single-precision與 Double-precision其 floating-point暫存器與其相關指令。

RISC-V 可自行新增指令集，為非標準擴充指令集(Non-Standard Extension)，若想要在 RV32IMA之外新增某些指令稱為 "H"，則稱為 RV32IMAXH，接在 "X"之後的為非標準擴充指令集，RISC-V 提供了相當的彈性去選擇所需的標準擴充指令集，也可再加上自行客製化的非標準擴充指令集。

#### Reference
[1] [The RISC-V Instruction Set Manual](https://riscv.org/specifications/)
