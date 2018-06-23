---
title: 'Shell programming: if 條件式'
categories: Shell programming
tags: Shell programming
abbrlink: 2419b51b
date: 2018-05-06 11:23:11
---


在 Shell programming中如何使用 if條件式。

#### if 條件式語法

##### 使用 command exit status

    if command
    then
        command
        ...
    else
        command
        ...
    fi

##### 使用 [ ... ]

    if [ condition ]
    then
        command
        ...
    else
        command
        ...
    fi

##### 使用 test command

    if test condition
    then
        command
        ...
    else
        command
        ...
    fi

#### if 比較運算子

##### 字串比較運算子

| [ ... ]                   | test command                   | 用法(回傳 TRUE)         |
| ------------------------- | ------------------------------ | ----------------------- |
| if [ string1 = string2 ]  | if test string1 = string2      | string1與 string2相同   |
| if [ string1 != string2 ] | if test string1 != string2     | string1與 string2不相同 |
| if [ string ]             | if test string                 | string 不為空值         |
| if [ -n string ]          | if test -n string              | string 不為空值         |
| if [ -z string ]          | if test -z string              | string 為空值           |

##### 整數比較運算子

| [ ... ]              | test command          | 用法(回傳 True)        |
| -------------------- | --------------------- | ---------------------- |
| if [ int1 -eq int2 ] | if test int1 -eq int2 | int1等於 int2          |
| if [ int1 -ne int2 ] | if test int1 -ne int2 | int1不等於 int2        |
| if [ int1 -gt int2 ] | if test int1 -gt int2 | int1大於 int2          |
| if [ int1 -ge int2 ] | if test int1 -ge int2 | int1大於等於 int2      |
| if [ int1 -lt int2 ] | if test int1 -lt int2 | int1小於 int2          |
| if [ int1 -le int2 ] | if test int1 -le int2 | int1小於等於 int2      |

##### 檔案運算子

| [ ... ]                | test command            | 用法(回傳 True)                                             |
| ---------------------- | ----------------------- | ----------------------------------------------------------- |
| if [ -d file ]         | if test -d file         | file是個目錄                                                |
| if [ -e file ]         | if test -e file         | file存在                                                    |
| if [ -f file ]         | if test -f file         | file是個檔案                                                |
| if [ -r file ]         | if test -r file         | file可被讀取                                                |
| if [ -s file ]         | if test -s file         | file內容非空                                                |
| if [ -w file ]         | if test -w file         | file可被寫入                                                |
| if [ -x file ]         | if test -x file         | file可被執行                                                |
| if [ -L file ]         | if test -x file         | file是個符號連結                                            |
| if [ -b file ]         | if test -b file         | file is a block special file                                |
| if [ -c file ]         | if test -c file         | file is a character special file                            |
| if [ -g file ]         | if test -g file         | file's set group ID flag is set                             |
| if [ -h file ]         | if test -h file         | file is a symbolic link. This operator is retained for compatibility with previous versions of this program. Do not rely on its existence; use -L instead. |
| if [ -k file ]         | if test -k file         | file's sticky bit is set |
| if [ -p file ]         | if test -p file         | file is a named pipe (FIFO) |
| if [ -u file ]         | if test -u file         | file's set user ID flag is set |
| if [ -O file ]         | if test -O file         | file's owner matches the effective user id of this process  |
| if [ -G file ]         | if test -G file         | file's group matches the effective group id of this process |
| if [ -S file ]         | if test -S file         | file is a socket                                            |
| if [ file1 -nt file2 ] | if test file1 -nt file2 | file1比 file2新                             |
| if [ file1 -ot file2 ] | if test file1 -ot file2 | file2比 file2舊                             |
| if [ file1 -ef file2 ] | if test file1 -ef file2 | file1與 file2參照到相同檔案                 |

##### 邏輯負運算子 !

可使用 !來否定運算式的判斷，例如：**[ ! -f file ]**。

##### 邏輯 and運算子 -a

例如: **[ -f file1  -a  -f file2 ]**。

##### 邏輯 or運算子 -o

例如: **[ -f file1  -o  -f file2 ]**。

##### 括號的使用

例如: **[ \\( -f file1 \\)  -a  \\(  -f file2 \\) ]**。
