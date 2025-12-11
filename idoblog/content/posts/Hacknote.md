---
title: Hacknote writeup
date: 2024-12-06T19:30:32+03:00
draft: false
description: Writeup to hacknote challenge in pwnable.tw
tags:
  - pwn
  - tw
  - heap
toc: false
lastmod: 2024-12-06T19:30:32+03:00
---
## Introduction
We are facing a 32 bit closed-source binary.
The binary follows a simple functionallity:
```
----------------------
       HackNote
----------------------
 1. Add note
 2. Delete note
 3. Print note
 4. Exit
----------------------
```
The binary lacks PIE and features Partial RELRO. 


## Reversing  
Let's now review the functions associated with each option.
```c AddNote

```