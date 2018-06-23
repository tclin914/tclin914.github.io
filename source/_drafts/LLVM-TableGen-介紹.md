---
title: LLVM TableGen 介紹
abbrlink: df61e88c
date: 2017-08-09 15:15:43
tags:
---

本文介紹 LLVM TableGen的語法。

## TableGen 介紹

TableGen 使用來記錄一些固定或重複的結構，減少撰寫程式的工作量，它定義了一套描述的語法規範，TableGen 只在乎語法是否正確，並不在乎其語義。

在 LLVM 中，TableGen 主要是用在後端 code generation 時，描述目標機器的 registers，instructions 與 calling convention 等等特性。

## TableGen 語法

### TableGen comments

支援 C++ 的 "//" 型式，同時也支援 "/\* \*/" 型式。

### TableGen type system

TableGen 擁有強型別的型別系統，支援低階的型別（例如：bit）或高階的型別（例如：dag）。

##### bit 
  boolean 值，可為 0 或 1。

* int
  32 位元整數值。

* string
  表示字串。

* code
  The code type represents a code fragment, which can be single/multi-line string literal.

* bits<n>
  固定 n bits 的值。

* list<ty> 
  其內容型別要相同，可為其它 list。

* Class Type
  自定義型別。

* dag
  表示 directed acyclic graph。

### TableGen values and expressions

* ?
  未初始化。

* 0b1001011
  二進位整數值，其位元數為所其表示，並不會遭到 extended / truncated。

* 7
  十進位整數值。

* 0x7F
  十六進位整數值。

* "foo"
  一字串。

* [{ ... }]
  code fragment，一多行的字串。

* [ X, Y, Z ]&lt;type&gt;
  list 值，type 可以不用指定。

* { a, b, 0b10 }
  初始化一個 bits<4> 的值，其中一個 bit 值來自 a，一個 bit 值來自 b，兩個 bits 值來自 0b10。

* value
  變數。

* value{17}
  存取此變數的某個 bit。

* value{15-17}
  存取此變數的一串 bits。

* DEF
  reference to a record definition.

* CLASS&lt;val list&gt;
  reference to a new anonymous definition of CLASS with the specified template arguments.

* X.Y
  reference to the subfield of a value.

* list[4-7, 17, 2-3]
  將一 list 切片。

* foreach <var> = [ &lt;list&gt; ] in { &lt;body&gt; }
* foreach <var> = [ &lt;list&gt; ] in &lt;def&gt;
  循環 list 裡頭的每個 element。

* foreach <var> = 0-15 in ...
* foreach <var> = {0-15, 32-47} in ...
  循環一或多個整數範圍。

* (DEF a, b)
  一個 dag 值，第一個必須為 record definition。

* !listconcat(a, b, ...)
  將多個 list 串接起來。

* !strconcat(a, b, ...)
  將多個字串給串接起來。

* str1#str2
  將兩個字串給串接起來。

* !cast<type>(a)
  A symbol of type type obtained by looking up the string ‘a’ in the symbol table. If the type of ‘a’ does not match type, TableGen aborts with an error. !cast<string> is a special case in that the argument must be an object defined by a ‘def’ construct.

* !subst(a, b, c)
  If ‘a’ and ‘b’ are of string type or are symbol references, substitute ‘b’ for ‘a’ in ‘c.’ This operation is analogous to $(subst) in GNU make.

* !foreach(a, b, c)
  For each member of dag or list ‘b’ apply operator ‘c.’ ‘a’ is a dummy variable that should be declared as a member variable of an instantiated class. This operation is analogous to $(foreach) in GNU make.

* !head(a)
  取出 list 中的第一個 element。

* !tail(a)
  The 2nd-N elements of list 'a.' （第二個 element？ v.s. 最後一個 element？）

* !empty(a)
  list 是否為空，回傳值為 0 或 1。

* !if(a, b, c)
  若 a 不為零，回傳 b，否則回傳 c。

* !eq(a, b)
  字串比較，相同回傳 1，不同回傳 0。

* !shl(a, b) !srl(a, b) !sra(a, b) !add(a, b) !and(a, b)
  二元或數值運算。

### TableGen classes and definitions






