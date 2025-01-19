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
lastmod: 2025-01-19T18:30:32+03:00
---

## Introduction
A collection of notes I've taken while learning about malloc/heap internals.

You are more than welcome to point out any errors and contribute to improving these notes :)

## Malloc
The allocator has a set of functions and metadata where its goal is to manage a running program's dynamic memory.\
Malloc's metadata contains heaps, chunks and arenas.

Briefly:\
heap - A large memory block that can be divided into smaller chunks, when the program allocates memory, it is taken from the heap.\
arena - A structure that manages the allocation process in the heap.\
chunk - A block of memory with varying sizes (within the heap).

### Chunks
in-use chunk layout:
```
 
 +-------------+--------+-+-+-+-------------------------+
 |  prev_size  |  size  |A|M|P|           data          |
 +-------------+--------+-+-+-+-------------------------+
 ↑                            ↑
 malloc ptr                   user ptr

```
AMP - **A**llocated Arena, **M**map'd, **P**rev in use

prev_size - When freeing a chunk, it writes down the size of a previous chunk\
size - data + metadata size\
Allocated Arena - If the chunk belongs to the allocated arena or mmap\
Mmap'd - If it is mmapd\
Prev in use - if the previous chuck in use (1 - true)

                               
