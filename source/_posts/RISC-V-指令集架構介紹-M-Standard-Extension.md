---
title: RISC-V 指令集架構介紹 - M Standard Extension
categories: RISC-V
tags: RISC-V
abbrlink: f37f836
date: 2018-05-22 05:44:36
updated: 2018-05-22 05:44:36
---


M standard extension為整數乘法與除法指令擴充，RV32IM 即為RV32I 指令集加上 M standard extension，RV64IM 即為RV64I 指令集加上 M standard extension，以下所提到的 XLEN-bit在 RV32或 RV64分別是 32-bit與 64-bit，介紹各個指令的用途與格式。

### 乘法運算 (Multiplication Operations)

* **MUL**
    *MUL rd, rs1, rs2*
    將 rs1暫存器與 rs2暫存器做乘法運算，會產生 2 x XLEN-bit 的運算結果，將此結果的最低 XLEN-bit寫入 rd暫存器。

* **MULH/MULHU/MULHSU**
    *mulh/mulhu/mulhsu rd, rs1, rs2*
    將 rs1暫存器與 rs2暫存器做乘法運算，會產生 2 x XLEN-bit 的運算結果，將此結果的最高 XLEN-bit寫入 rd暫存器。MULH 為 signed x signed，MULHU 為 unsigned x unsigned，MULHSU 為 signed x unsigned。

* **MULW**
    *mulw rd, rs1, rs2*
    RV64 才會有此道指令，將 rs1暫存器的最低 32-bit與 rs2暫存器的最低 32-bit做乘法運算，並將此結果的最低 32-bit做 sign-extension成 64-bit寫入 rd暫存器。


### 除法運算 (Division Operations)

* **DIV/DIVU**
    *div/divu rd, rs1, rs2*
    將 rs1暫存器與 rs2暫存器做 signed/unsigned 除法運算，結果寫入 rd暫存器。

* **REM/REMU**
    *rem/remu rd, rs1, rs2*
    將 rs1暫存器與 rs2暫存器做 signed/unsigned 餘數運算，結果寫入 rd暫存器。

* **DIVW/DIVUW**
    *divw/divuw rd, rs1, rs2*
    RV64 才會有此道指令，將 rs1暫存器的最低 32-bit與 rs2暫存器的最低 32-bit做 signed/unsigned 除法運算，並將此結果做 sign-extension成 64-bit寫入 rd暫存器。

* **REMW/REMUW**
    *remw/remuw rd, rs1, rs2*
    RV64 才會有此道指令，將 rs1暫存器的最低 32-bit與 rs2暫存器的最低 32-bit做 signed/unsigned 餘數運算，並將此結果做 sign-extension成 64-bit寫入 rd暫存器。

#### Reference
[1] [The RISC-V Instruction Set Manual](https://riscv.org/specifications/)
