---
title: ssh 免密碼登入
categories: Shell
tags: Shell
abbrlink: 251befd7
date: 2018-06-08 14:54:08
updated: 2018-06-08 14:54:08
---


這邊紀錄一下如何 ssh連線其它機器可以免輸入密碼登入。

假設我們目前在 A機器，想要連線 B機器。

先在 A機器產生 id_rsa與 id_rsa.pub，若已產生可免去此步驟， key預設放在 ~/.ssh，按下 enter繼續。

    jim@A:~ $ ssh-keygen -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/users/jim/.ssh/id_rsa):


請勿輸入密碼，直接按下 enter繼續。

    Enter passphrase (empty for no passphrase):

一樣不用輸入密碼，直接按下 enter繼續。

    Enter same passphrase again:

建立好 key了。

    Your public key has been saved in /home/users/jim/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:Q14hkz7AJG0rcEv1kmffq6gE5C6GkqTb9J/CYWj8UHw jim@A

解說一下產生的一對 key的作用。

* id_rsa: private key
* id_rsa.pub: public key

將 id_rsa.pub的內容複製到遠端機器的 ~/.ssh/authorized_keys中，之後在登入時，便會用本地的 id_rsa與遠端機器的 id_rsa.pub做認證，便可免輸入密碼登入遠端機器。

接下來在 B機器建立目錄 ~/.ssh，如果已有可跳過此步驟。

    jim@A:~ $ ssh b@B mkdir -p .ssh
    jim@B's password:

把 A機器的 ~/.ssh/id_rsa.pub也就是 public key複製到 B機器的 .ssh/authorized_keys。

方法 1

    jim@A:~ $ cat .ssh/id_rsa.pub | ssh jim@B 'cat >> .ssh/authorized_keys'
    jim@B's password:

方法 2

    jim@A:~ $ ssh-copy-id -i .ssh/id_rsa.pub jim@B
    jim@B's password:

即可成功登入 B機器而不需要輸入密碼了。

    jim@A:~ $ ssh jim@B
