---
title: C++11 - Move語意和 Rvalue References
abbrlink: 2aaeb850
tags:
---

雖然已經2018，但筆者對於 C++11的許多新功能還感到十分陌生，本文要介紹 C++11新功能，Move語意和 Rvalue References，這也是筆者感到較困難的，因此撰寫此筆記。

## rvalue 和 lvalue 的定義

首先定義何謂是 rvalue和 lvalue，lvalue 在記憶體中佔有實際位址，且撰寫程式的人可透過識別子(identifier)來讀取其內容，不屬於 lvalue的即為 rvalue，意即 rvalue沒有在記憶體中佔有實際位址，只為運算過程中所產生的暫時物件，當然也無法透過識別子來讀取其內容。

## 簡單範例

```c
int value;
value = 5;
```

### Reference
[1] [https://www.artima.com/cppsource/rvalue.html]
[2] [https://eli.thegreenplace.net/2011/12/15/understanding-lvalues-and-rvalues-in-c-and-c/]
