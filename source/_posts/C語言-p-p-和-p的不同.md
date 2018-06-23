---
title: 'C語言: ++*p, *p++和 *++p的不同'
categories: C
tags: C
abbrlink: e9206a47
date: 2018-05-07 10:37:54
---


介紹一下在 C語言中 ++\*p, \*p++和 \*++p的不同。

#### ++\*p

```c
#include <stdio.h>

int main() {
  int array[] = { 10, 20};
  int *p = array;
  int v = ++*p;
  printf("v = %d, array[0] = %d, array[1] = %d, *p = %d\n", v, array[0], array[1], *p);
  return 0;
}
```

#### \*p++

```c
#include <stdio.h>

int main() {
  int array[] = { 10, 20};
  int *p = array;
  int v = *p++;
  printf("v = %d, array[0] = %d, array[1] = %d, *p = %d\n", v, array[0], array[1], *p);
  return 0;
}
```

#### \*++p

```c
#include <stdio.h>

int main() {
  int array[] = { 10, 20};
  int *p = array;
  int v = *++p;
  printf("v = %d, array[0] = %d, array[1] = %d, *p = %d\n", v, array[0], array[1], *p);
  return 0;
}
```

**++\*p**、**\*p++**和 **\*++p**是由 postfix ++(++在後)、prefix ++(++在前)和 \*(deference)運算子組合而成，其運算優先權與關聯性為:
1. prefix ++與 \*的運算優先權相同，關聯性為從右到左。
2. postfix ++的運算優先權高於 prefix ++與 \*，關聯性為從左到右。


#### ++\*p

因 prefix ++與 \*的運算優先權相同，因此從右到左，可視為是 ++(\*p)，將指標 p取值後加 1，結果為 v = 11, array[0] = 11, array[1] = 20, *p = 11。

#### \*p++

因 postfix ++的運算優先權高於 \*，可視為是 \*(p++)，將指標 p加 1，因為是 postfix，所以取的值為指標 p加 1之前所指向的值，結果為 v = 10, array[0] = 10, array[1] = 20, *p = 20。

#### \*++p

因 prefix ++與 \*的運算優先權相同，因此從右到左，可視為是 \*(++p)，將指標 p加 1後取值，結果為 v = 20, array[0] = 10, array[1] = 20, *p = 20。

#### Note

遇到 postfix ++的情況，可視為在執行完 **;** 之後，才會執行 ++的運算。

#### Reference
[1] [C Operator Precedence Table](http://www.difranco.net/compsci/C_Operator_Precedence_Table.htm)
