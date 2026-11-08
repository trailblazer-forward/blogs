---
title: Linux 使用经验
tags: technology linux
date: 26-11-08 18:41
---

下列举一些 Linux 下的使用经验。

## 终端

快捷键：

- Ctrl+L 清屏
- Ctrl+U 清空输入缓冲区
- Ctrl+W 删除末尾的一个单词

注：一个单词定义为由正整数个字符（汉字、字母和数字）或正整数个符号组成的字符串。对于中文而言，是否进一步地依照词语进行划分在不同软件中表现不同（浏览器通常以词语为单位划分，而 Vim 等将连续的中文字符视作一个单词）。

Shell 是一种很强大的工具，多翻翻手册学习能够让很多任务事半功倍。

## 资料

ArchWiki：[英文版](https://wiki.archlinux.org) [中文版](https://wiki.archlinuxcn.org)

## Vim

清屏：`:%d`（结束后为 Normal 模式）、`ggcG`（结束后为 Insert 模式）
清空一行并进入 Insert 模式：`S` 或 'cc'
善用寄存器，所有删除的内容都会进入寄存器，很方便进行转移。

## 键盘

Fn 键失效：无论你是否使用苹果键盘，参见 [archwikicn](https://wiki.archlinux.org/title/Apple_Keyboard#Function_keys_do_not_work)

## 下载

换源：使用中国/靠近中国的软件镜像，例如 TUNA、UTSC、Aliyun 等等。

## 字体

中文显示为异体字：参见 [archwikicn](https://wiki.archlinuxcn.org/wiki/%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%E6%9C%AC%E5%9C%B0%E5%8C%96#%E4%BF%AE%E6%AD%A3%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%E6%98%BE%E7%A4%BA%E4%B8%BA%E5%BC%82%E4%BD%93%EF%BC%88%E6%97%A5%E6%96%87%EF%BC%89%E5%AD%97%E5%BD%A2)
