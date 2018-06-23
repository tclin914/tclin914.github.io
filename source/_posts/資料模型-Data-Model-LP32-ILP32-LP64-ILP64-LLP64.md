---
title: 資料模型 Data Model - LP32 ILP32 LP64 ILP64 LLP64
categories: Data Model
tags:
  - LP32
  - ILP32
  - LP64
  - ILP64
  - LLP64
abbrlink: d7ed614
date: 2018-05-24 05:31:31
updated: 2018-05-24 05:31:31
---


本文介紹一下資料模型 (Data Model)，下表格為 C語言型別對應不同資料模型的長度。

| Type      | LP32  | ILP32 | LP64  | ILP64 | LLP64 |
| --------- | ----- | ----- | ----- | ----- | ----- |
| CHAR      | 8     | 8     | 8     | 8     | 8     |
| SHORT     | 16    | 16    | 16    | 16    | 16    |
| INT       | 16    | 32    | 32    | 64    | 32    |
| LONG      | 32    | 32    | 64    | 64    | 32    |
| LONG LONG | 64    | 64    | 64    | 64    | 64    |
| POINTER   | 32    | 32    | 64    | 64    | 64    |

LP32 與 ILP32是在 32-bit平台，LP64、ILP64 與 LLP64則是在 64-bit平台。

LP32 指的是 LONG與 POINTER為 32-bit，ILP32 指的是 INT、LONG與 POINTER為 32-bit，LP64 指的是 LONG與 POINTER為 64-bit，ILP64 指的是 INT、LONG與POINTER為 64-bit，LLP64 指的是 LONG LONG與 POINTER為 64-bit，不管是在哪個資料模型，LONG LONG皆為是 64-bit。

資料模型規定了每個 C語言型別的長度，讓編譯器編譯出來的程式能使用正確的資料長度與平台溝通。
