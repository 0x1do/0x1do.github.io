---
weight: 1
title: Glibc malloc notes
date: 2025-01-19T18:22:32+03:00
draft: false
author: ido
authorLink: https://0x1do.github.io
description: 
tags:
  - heap
lightgallery: true
toc:
  enable: true
lastmod: 2025-01-19T18:22:32+03:00
---

## Introduction
A collection of notes I've taken while learning about malloc/heap internals.

You are more than welcome to point out any errors and contribute to improving these notes :)

### Malloc
The allocator has a set of functions and metadata where its goal is to manage a running program's dynamic memory.\
Malloc's metadata contains heaps, chunks and arenas.

Briefly:\
heap - A big block of memory that can be broken down into chunks, when the program allocates memory it's taken from the heap.\
arena - A structure that manages the allocation process in the heap.\
chunk - A block of memory with varying sizes (within the heap).\