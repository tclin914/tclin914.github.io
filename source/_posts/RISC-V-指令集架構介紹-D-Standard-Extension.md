---
title: RISC-V 指令集架構介紹 - D Standard Extension
categories: RISC-V
tags: RISC-V
abbrlink: 14d8f9aa
date: 2018-08-29 17:34:53
updated: 2018-08-29 17:34:53
---


D standard extension為雙精度浮點數(single-precision floating-point)指令擴充，將 32個浮點數暫存器(f0-f31)擴充成為 64-bit。

下面所提到的一般暫存器(general register)指的是在 RV32I或 RV64I的 x0-x31，浮點數暫存器則為 f0-f31。

### 雙精度載入與儲存指令 (Double-Precision Load and Store Instructions)

* **FLD**
    *fld rd, rs1, simm12*
    立即值為 sign-extended 12-bit，載入位址則為 rs1一般暫存器加上 sign-extended 12-bit，將 double-precision浮點數從記憶體中載入 rd浮點數暫存器。

* **FSW**
    *fsd rs2, rs1, simm12*
    立即值為 sign-extended 12-bit，儲存位址則為 rs1一般暫存器加上 sign-extended 12-bit，將 rs2浮點數暫存器的 double-precision浮點數值寫入記憶體。

### 雙精度浮點數運算指令 (Double-Precision Floating-Point Computational Instructions)

* **FADD.D/FSUB.D**
    *fadd.d/fsub.d rd, rs1, rs2*
    將 rs1浮點數暫存器與 rs2浮點數暫存器做加法/減法運算，結果寫入 rd浮點數暫存器。

* **FMUL.D/FDIV.D**
    *fmul.d/fdiv.d rd, rs1, rs2*
    將 rs1浮點數暫存器與 rs2浮點數暫存器做乘法/除法運算，結果寫入 rd浮點數暫存器。

* **FMIN.D/FMAX.D**
    *fmin.d/fmax.d rd, rs1, rs2*
    將 rs1浮點數暫存器與 rs2浮點數暫存器做最大值/最小值比較，將最大值/最小值寫入 rd浮點數暫存器。

* **FSQRT.D**
    *fsqrt.d rd, rs1*
    將 rs1浮點數暫存器做取平方根運算，結果寫入 rd浮點數暫存器。

* **F[N]MADD/F[N]MSUB**
    *f[n]madd/f[n]msub rd, rs1, rs2, rs3*
    將 rs1浮點數暫存器與 rs2浮點數暫存器做乘法運算後，再與 rs3浮點數暫存器做加法/乘法運算，結果寫入 rd浮點數暫存器，n版本則為做完乘法運算後取 negate，為乘法與加法指令一次完成，中間並沒有經過 rounding，所以精準度上會比一般乘法指令加上加法指令還高。

### 雙精度浮點數轉換與移動指令 (Double-precision Floating-Point Conversion and Move Instructions)

將浮點數轉換為整數或整數轉換為浮點數指令，會根據指令中的 rm field來決定轉換的 rounding mode，以及單精度與雙精度之間的轉換。

* **FCVT.W.D/FCVT.L.D**
    *fcvt.w.d/fcvt.l.d rd, rs1*
    將 rs1浮點數暫存器中的浮點數轉換為 signed 32-bit/64-bit，結果寫入 rd一般暫存器。

* **FCVT.D.W/FCVT.D.L**
    *fcvt.d.w/fcvt.d.l rd, rs1*
    將 rs1一般暫存器中的signed 整數轉換為浮點數，結果寫入 rd一般暫存器。

* **FCVT.WU.D/FCVT.LU.D**
    *fcvt.wu.d/fcvt.lu.d rd, rs1*
    將 rs1浮點數暫存器中的浮點數轉換為 unsigned 32-bit/64-bit，結果寫入 rd一般暫存器。

* **FCVT.D.WU/FCVT.D.LU**
    *fcvt.d.wu/fcvt.d.lu rd, rs1*
    將 rs1一般暫存器中的unsigned 整數轉換為浮點數，結果寫入 rd一般暫存器。

* **FCVT.S.D/FCVT.D.S**
    *fcvt.s.d/fcvt.d.s rd, rs1*
    為單精度與雙精度之間的轉換。

sign-injection Instructions

* **FSGNJ.D**
    *fsgnj.d rd, rs1, rs2*
    rd浮點數暫存器中的數值來自 rs1浮點數暫存器，sign bit則為 rs2浮點數暫存器的 sign bit。

* **FSGNJN.D**
    *fsgnjn.d rd, rs1, rs2*
    rd浮點數暫存器中的數值來自 rs1浮點數暫存器，sign bit則為 rs2浮點數暫存器的 sign bit的相反。

* **FSGNJX.D**
    *fsgnjx.d rd, rs1, rs2*
    rd浮點數暫存器的數值來自 rs1浮點數暫存器，sign bit則為 rs1浮點數暫存器的 sign bit與 rs2浮點數暫存器的 sign bit做 xor運算。

只在 RV64有效，單純 floating point與 integer之間的移動指令，與上面不同的是，只做 bit的移動，並不做浮點數與整數之間的轉換。

* **FMV.X.D**
    *fmv.x.w rd, rs1*
    將 rs1浮點數暫存器的 64-bit數值寫入 rd一般暫存器中。

* **FMV.D.X**
    *fmv.w.x rd, rs1*
    將 rs1一般暫存器的 64-bit數值寫入 rd浮點數暫存器。

### 雙精度浮點數比較指令 (Double-Precision Floating-Point Compare Instructions)

* **FEQ.D/FLT.D/FLE.D**
    *feq.d/flt.d/fle.d rd, rs1, rs2*
    將 rs1浮點數暫存器與 rs2浮點數暫存器做 eq/lt/le比較，結果寫入 rd一般暫存器。

### 雙精度浮點數分類指令 (Double-Precision Floating-Point Classify Instruction)

* **FCLASS.D**
    *fclass.d rd, rs1*
    功能與 FCLASS.S類似。

#### Reference
[1] [The RISC-V Instruction Set Manual](https://riscv.org/specifications/)
