---
title: RISC-V 指令集架構介紹 - RV64I
categories: RISC-V
tags: RISC-V
abbrlink: 464e7cd9
date: 2018-05-21 10:49:06
updated: 2018-05-21 10:49:06
---


RV64I是基於 RV32I的指令集架構，本文只會說明與 RV32I不同之處，RV64I將在 RV32I的 32個 32-bit暫存器給擴大成 64-bit，所有的指令也轉換成是操作在 64-bit暫存器上，也額外增加一些指令能夠操作 64-bit暫存器中的最低 32-bit，這些指令會以 **W** 為結尾，以下介紹各個指令的用途與格式。

### 整數運算指令 (Integer Computational Instructions)

#### 整數暫存器與常數指令 (Integer Register-Immediate Instructions)

指令為暫存器與常數之間的運算

* **ADDIW**
    *addiw rd, rs1, simm12*
    常數部分為 sign-extended 12-bit，會將 12-bit做 sign-extension成 32-bit後，再與 rs1暫存器的最低 32-bit做加法運算，並將此結果 sign-extension成 64-bit寫入 rd暫存器。*addiw rd, rs1, 0* 是將 rs1暫存器的最低 32-bit做 sign-extension成 64-bit寫入 rd暫存器，可寫成 sext.w rd, rs1

* **SLLI/SRLI/SRAI**
    *slli/srli/srai rd, rs1, uimm6*
    常數部分為 unsigned 6-bit，範圍為 0~63，為 shift amount，將 rs1暫存器做 shift運算，結果寫入 rd暫存器，SLLI為 logical左移，會補 0到最低位元，SRLI為 logical右移，會補 0到最高位元，SRAI為 arithmetic右移，會將原本的 sign bit複製到最高位元。

* **SLLIW/SRLIW/SRAIW**
    *slliw/srliw/sraiw rd, rs1, uimm5*
    常數部分為 unsigned 5-bit，範圍為 0~31，為 shift amount，將 rs1暫存器的最低 32-bit做 shift運算，並將此結果 sign-extension成 64-bit寫入 rd暫存器，SLLIW為 logical左移，會補 0到最低位元，SRLIW為 logical右移，會補 0到最高位元，SRAIW為 arithmetic右移，會將原本的 sign bit複製到最高位元。

* **LUI (Load upper immediate)**
    *lui rd, uimm20*
    將 unsigned 20-bit放到 rd暫存器的 31-12 bits，並將最低的 12-bit補 0。

* **AUIPC(add upper immediate to pc)**
    *auipc rd, uimm20*
    unsigned 20-bit放到最高 20位元，剩餘 12位元補0，將此 32-bit數值 sign-extension成 64-bit，與 pc相加寫入 rd暫存器。

#### 整數暫存器與暫存器指令 (Integer Register-Register Insructions)

指令為暫存器與暫存器之間的運算

* **ADDW/SUBW**
    *addw/subw rd, rs1, rs2*
    將 rs1暫存器的最低 32-bit與 rs2暫存器的最低 32-bit做加法/減法運算，並將此結果 sign-extension成 64-bit寫入 rd暫存器。

* **SLL/SRL/SRA**
    *sll/srl/sra rd, rs1,, rs2*
    將 rs1暫存器做 shift運算，結果寫入 rd暫存器，rs2暫存器的最低 6-bit為 shift amount。

* **SLLW/SRLW/SRAW**
    *sllw/srlw/sraw rd, rs1,, rs2*
    將 rs1暫存器的最低 32-bit做 shift運算，並將此結果 sign-extension成 64-bit寫入 rd暫存器，rs2暫存器的最低 5-bit為 shift amount。

### 載入與儲存指令 (Load and Store Instructions)

* **LD/LW/LWU**
    *ld/lw/lwu rd, rs1, simm12*
    常數部分為 sign-extended 12-bit，載入位址則為 rs1暫存器加上 sign-extended 12-bit，LD為載入 64-bit資料寫入 rd暫存器，LW/LWU為載入 32-bit資料分別做 unsigned/signed extension成 64-bit後寫入 rd暫存器。

* **SD**
    *sd rs2, rs1, simm12*
    常數部分為 sign-extended 12-bit，儲存位址則為 rs1暫存器加上 sign-extended 12-bit，SD為將 rs2暫存器完整 64-bit資料寫入記憶體。

RV64I指令集也包含本文沒有提到的 RV32I指令，功能一樣，只是將操作的暫存器寬度從 32-bit擴大為 64-bit。

#### Reference
[1] [The RISC-V Instruction Set Manual](https://riscv.org/specifications/)
