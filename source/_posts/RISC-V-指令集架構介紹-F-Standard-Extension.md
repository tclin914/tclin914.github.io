---
title: RISC-V 指令集架構介紹 - F Standard Extension
categories: RISC-V
tags: RISC-V
abbrlink: 3d45634e
date: 2018-06-23 17:05:03
---


F standard extension為單精度浮點數(single-precision floating-point)指令擴充，增加了 32個浮點數暫存器(f0-f31)，長度為 32-bit，以及一個浮點數控制與狀態暫存器(fcsr)，與 IEEE 754-2008相容。

下面所提到的一般暫存器(general register)指的是在 RV32I或 RV64I的 x0-x31，浮點數暫存器則為 f0-f31。

### 浮點數控制與狀態暫存器 (Floating-Point Control and Status Register)

{% asset_img fcsr.png %}

浮點數指令可設定成動態的去參照目前 fcsr中的 Rounding Mode bits來決定如何 rounding，fcsr的 Rounding Mode編碼則為下方表格。

{% asset_img rounding_mode_encoding.png %}

若發生 floating point exception，則會去設定相對應的 fcsr exception flags，下方表格為各個 flag與其 floating point exception的對照。

{% asset_img exception_flag_encoding.png %}

### 單精度載入與儲存指令 (Single-Precision Load and Store Instructions)

* **FLW**
    *flw rd, rs1, simm12*
    常數部分為 sign-extended 12-bit，載入位址則為 rs1一般暫存器加上 sign-extended 12-bit，將 single-precision浮點數值從記憶體中載入 rd浮點數暫存器。

* **FSW**
    *fsw rd, rs1, simm12*
    常數部分為 sign-extended 12-bit，儲存位址則為 rs1一般暫存器加上 sign-extended 12-bit，將 rs1浮點數暫存器的 single-precision浮點數值寫入記憶體。

### 單精度浮點數運算指令 (Single-Presicion Floating-Point Computational Instructions)

* **FADD.S/FSUB.S**
    *fadd.s/fsub.s rd, rs1, rs2*
    將 rs1浮點數暫存器與 rs2浮點數暫存器做加法/減法運算，結果寫入 rd浮點數暫存器。

* **FMUL.S/FDIV.S**
    *fmul.s/fdiv.s rd, rs1, rs2*
    將 rs1浮點數暫存器與 rs2浮點數暫存器做乘法/除法運算，結果寫入 rd浮點數暫存器。

* **FMIN.S/FMAX.S**
    *fmin.s/fmax.s rd, rs1, rs2*
    將 rs1浮點數暫存器與 rs2浮點數暫存器做最大值/最小值比較，將最大值/最小值寫入 rd浮點數暫存器。

* **FSQRT.S**
    *fsqrt.s rd, rs1*
    將 rs1浮點數暫存器做取平方根運算，結果寫入 rd浮點數暫存器。

* **F[N]MADD/F[N]MSUB**
    *f[n]madd/f[n]msub rd, rs1, rs2, rs3*
    將 rs1浮點數暫存器與 rs2浮點數暫存器做乘法運算後，再與 rs3浮點數暫存器做加法/乘法運算，結果寫入 rd浮點數暫存器，n版本則為做完乘法運算後取 negate，為乘法與加法指令一次完成，中間並沒有經過 rounding，所以精準度上會比一般乘法指令加上加法指令還高。

### 單精度浮點數轉換與移動指令 (Single-precision Floating-Point Conversion and Move Instructions)

將浮點數轉換為整數或整數轉換為浮點數指令，會根據指令中的 rm field來決定轉換的 rounding mode。

* **FCVT.W.S/FCVT.L.S**
    *fcvt.w.s/fcvt.l.s rd, rs1*
    將 rs1浮點數暫存器中的浮點數轉換為 signed 32-bit/64-bit，結果寫入 rd一般暫存器。

* **FCVT.S.W/FCVT.S.L**
    *fcvt.s.w/fcvt.s.l rd, rs1*
    將 rs1一般暫存器中的signed 整數轉換為浮點數，結果寫入 rd一般暫存器。

* **FCVT.WU.S/FCVT.LU.S**
    *fcvt.wu.s/fcvt.lu.s rd, rs1*
    將 rs1浮點數暫存器中的浮點數轉換為 unsigned 32-bit/64-bit，結果寫入 rd一般暫存器。

* **FCVT.S.WU/FCVT.S.LU**
    *fcvt.s.wu/fcvt.s.lu rd, rs1*
    將 rs1一般暫存器中的unsigned 整數轉換為浮點數，結果寫入 rd一般暫存器。

sign-injection instructions

* **FSGNJ.S**
    *fsgnj.s rd, rs1, rs2*
    rd浮點數暫存器中的數值來自 rs1浮點數暫存器，sign bit則為 rs2浮點數暫存器的 sign bit。

* **FSGNJN.S**
    *fsgnjn.s rd, rs1, rs2*
    rd浮點數暫存器中的數值來自 rs1浮點數暫存器，sign bit則為 rs2浮點數暫存器的 sign bit的相反。

* **FSGNJX.S**
    *fsgnjx.s rd, rs1, rs2*
    rd浮點數暫存器的數值來自 rs1浮點數暫存器，sign bit則為 rs1浮點數暫存器的 sign bit與 rs2浮點數暫存器的 sign bit做 xor運算。

單純 floating point與 integer之間的 move指令，與上面不同的是，只做 bit的移動，並不做浮點數與整數之間的轉換。

* **FMV.X.W**
    *fmv.x.w rd, rs1*
    將 rs1浮點數暫存器的 32-bit數值寫入 rd一般暫存器中，若在 RV64，則用 sign bit填滿最高 32-bit。

* **FMV.W.X**
    *fmv.w.x rd, rs1*
    將 rs1一般暫存器的最低 32-bit(已用IEEE 754-2008編碼好)數值寫入 rd浮點數暫存器。

### 單精度浮點數比較指令 (Single-Precision Floating-Point Compare Instructions)

* **FEQ.S/FLT.S/FLE.S**
    *feq.s/flt.s/fle.s rd, rs1, rs2*
    將 rs1浮點數暫存器與 rs2浮點數暫存器做 eq/lt/le比較，結果寫入 rd一般暫存器。

### 單精度浮點數分類指令 (Single-Precision Floating-Point Classify Instruction)

* **FCLASS.S**
    *fclass.s rd, rs1*
    根據下表分類 rs1浮點數暫存器的浮點數值，結果寫入 rd一般暫存器。

{% asset_img fclass.png %}

#### Reference
[1] [The RISC-V Instruction Set Manual](https://riscv.org/specifications/)
