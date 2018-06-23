---
title: RISC-V 指令集架構介紹 - Integer Calling convention
categories: RISC-V
tags:
  - RISC-V
  - Calling convention
abbrlink: '77838749'
date: 2018-05-09 07:29:20
updated: 2018-05-09 07:29:20
---


### Integer Calling convention

有 32個 32-bit(RV32)或 64-bit(RV64)的暫存器(register)，為 x0-x31，下表為各個暫存器其 ABI Name與其用途：

| Register | ABI Name | Description                       | Saver  |
| -------- | -------- | -----------                       | ------ |
| x0       | zero     | Hard-wired zero                   |   -    |
| x1       | ra       | Return address                    | Caller |
| x2       | sp       | Stack pointer                     | Callee |
| x3       | gp       | Global pointer                    |   -    |
| x4       | tp       | Thread pointer                    |   -    |
| x5       | t0       | Temporary/alternate link register | Caller |
| x6-7     | t1-2     | Temporaries                       | Caller |
| x8       | s0/fp    | Saved regsiter/frame pointer      | Callee |
| x9       | s1       | Saved register                    | Callee |
| x10-11   | a0-1     | Function arguments/return values  | Caller |
| x12-17   | a2-7     | Function arguments                | Caller |
| x18-27   | s2-11    | Saved registers                   | Callee |
| x28-31   | t3-6     | Temporaries                       | Caller |

#### x0 (zero)
x0內容值恆為 0，在許多的指令運算中，使用到 0值的機會很多，因此將其中一個暫存器的值直接固定為 0可帶來許多方便。

#### x1 (ra)
x1(ra) 使用來放置 return address，當函數(function)要結束返回時，便會返回到此暫存器中所儲存的位址，此暫存器為 Caller save，意謂者在一函數(Caller)中要呼叫另一函數(Callee)之前必須先將放置在 x1中的 Caller返回位址儲存到 stack中，因為當一呼叫其他函式，此被呼叫的函式的返回位址會同樣放到 x1(ra)中，所以此暫存器的內容值保存是由 Caller負責。

#### x2 (sp)

x2(sp) 用來放置 stack指標的值，可使用此暫存器對 stack做儲存載入的操作，此暫存器為 Callee save，當一被呼叫的函式想修改此暫存器的內容值前，必須先將此內容給保存下來，並在函式返回前，恢復其原本的值，換句話說，此暫存器的內容值在函式被呼叫前與此函式結束後的值應該保持一致。

#### x3 (gp)

x3(gp) 用來放置 global指標的值，此指標通常指向整個執行檔中的某個位址，可利用此指標來讀取資料。

#### x4 (tp)

x4(tp) 用來放置 thread指標的值，指向 Thread local storage。

#### x5 (t0)

x5(t0) 可用來當做一般的暫存器使用或者是第二個用來放置 return address的暫存器，此暫存器為 Caller save，即 Caller若在呼叫完某一函式後對此暫存器的內容有需要，必須在呼叫此函數之前將其內容值給儲存起來，避免 Callee修改到內容值。

#### x6-7 (t1-2) & x28-31 (t3-6)

一般的暫存器使用，為 Caller save，呼叫函式前後並不保證其暫存器內容值一致，若 Caller對其暫存器內容值有所需要，必須在呼叫函式前，先儲存起來，待呼叫函式結束後，在恢復其暫存器原本的內容值。

#### x8 (s0)

x8 可被當做是 Saved register或 Frame pointer使用，此暫存器為 Callee save，若在某函式內，要使用到此暫存器時，需先將其內容值給儲存起來，才可以使用，並在函式返回之前恢復原先的內容值，因爲在此函數被呼叫前後必須維持此暫存器的內容一致，在程式碼的產生上，可設定是否需要 Frame pointer去指向當前函式所使用到 stack的底部，此非為必要，所以在優化上，以 GCC為例，下 -fomit-frame-pointer選項，可將此暫存器當作 Saved register使用，增加一個可用的暫存器，提升效能。

#### x9 (s1) & x18-27 (s2-11)

Saved register，為 Callee save，需由 Callee負責其內容值的保存。

#### x10-x11 (a0-1)

x10(a0) 與 x11(a1) 使用來傳遞參數或函式返回時會將其返回值放入此暫存器中，為 Caller save。

#### x12-17 (a2-7)

x12-17(a2-7) 使用來傳遞參數，為 Caller save。

### RV32E Calling Convention

RV32E 只有 16個 32-bit的暫存器，為 x0-x15，有 6個傳遞參數用暫存器 a0-a5，2個 Callee save暫存器 s0-s1，以及 3個 temporaries暫存器 t0-t2。

### 總結

每個暫存器皆可當做一般的暫存器使用，Calling convention 則是規定了一系列的函式與函式之間互動的慣例，例如：如何傳遞參數給函式，如何取得函式結束後回傳的返回值，哪些暫存器的內容值需在函式呼叫前後保持一致，哪些暫存器其值若在函式呼叫結束後仍有需要，則需在函式呼叫前先行儲存起來，若一函式編譯成 object file後，提供外部連結，則須嚴格遵守 Calling convention，才能與其他函式溝通。
