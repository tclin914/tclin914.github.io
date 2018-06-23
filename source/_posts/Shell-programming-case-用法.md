---
title: 'Shell programming: case 用法'
categories: Shell programming
tags: Shell programming
abbrlink: f8c5f0e1
date: 2018-05-16 13:21:42
---


在 Shell programming中如何使用 case。

#### case 語法

    case value in
        pattern1)
            command
            ...
            command
            ;;
        pattern2)
            command
            ...
            command
            ;;
        pattern3)
            command
            ...
            command
            ;;
        ...
    esac

#### 範例

```bash
if [ "$#" -ne 1 ]
then
  echo "Usage: $0 [0-9]"
  exit 1
fi

case "$1" in
    0) echo zero;;
    1) echo one;;
    2) echo two;;
    3) echo three;;
    4) echo four;;
    5) echo five;;
    6) echo six;;
    7) echo seven;;
    8) echo eight;;
    9) echo nine;;
    *) echo "The input is not between 0 and 9."
esac
```

#### 進階用法

1. 多個情況匹配的時候，類似 or的功能
    pattern1 | pattern2

2. 一個範圍匹配
    [0-9]
