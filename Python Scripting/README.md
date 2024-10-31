# Hashlib and Binary Data

```python
import hashlib
def show_hash(): # Demonstration of Basic Hashing
    md5 = hashlib.sha256()
    md5.update(b'hello')
    print(md5.hexdigest())

def show_bytearray(): # Byte Array Data Type (Mutable as opposed to Byte Data Type)
	barr = bytearray(b"\x32\x42")
	print(len(barr))
	barr.append(65)
	print(barr)
def show_bitwise(): # BitWise Operation
	byte1 = int("01000001", 2)
	byte2 = int("00111000", 2)
	print(byte1 | byte2)
def show_bytes(): # Convert A Number to Bytes
	num = 512
	print(num.to_bytes(20, byteorder="big"))
	print(num.to_bytes(20, byteorder="little"))
```

# Itertools
```python
from itertools import *
counter = count(5, 5)

for i in range(10):
   print(next(counter)) # 5, 10, 15, ...
```

```python

from itertools import *
lst = ['red', 'green', 'blue']
cyclic = cycle(lst) 

for i in range(10):
   print(next(cyclic)) # Cycle Through Elements 10 times
```

```python

from itertools import *
lst1 = [1, 2]
lst2 = ['a', 'b', 'c']
zipped = list(zip_longest(lst1, lst2)) # Merges two lists and turns into tuple based on the longest list
print(zipped) # [(1, 'a'), (2, 'b'), (None, 'c')]
```

```python

from itertools import *
lst1 = [1, 2, 3, 4, 5, 6, 7, 8, 9]

combinations_list = list(combinations(lst1, 2)) # nC2
for comb in combinations_list:
   print(comb)
```

```python

from itertools import *
lst1 = [1, 2, 3, 4, 5, 6, 7, 8, 9]

permutations_list = list(permutations(lst1, 2)) # nP2

for perm in permutations_list:
   print(perm)
```

```python

from itertools import *
import string
cseq = string.ascii_letters + string.digits
# Bruteforce password of length 4
gen = permutations(cseq, 4)
for pw in gen:
   print("".join(pw))
```

```python

from itertools import *
import string
cseq = string.ascii_letters + string.digits
# Bruteforce password of length 4
gen = permutations(cseq, 4)
for pw in gen:
   pw_f = "".join(pw)
   if pw_f == 'bsdk':
       print("Success:", pw_f)
       break
   else:
       print("Failure:", pw_f)

```

# Default UTF-8 String to Byte Object
```python
s = "Hello World" # Encode it using UTF-8 into a byte object
print(s.encode()) #b'Hello World'
```

```python
print(bytes('a', 'utf-8')) # b'a'
print('a'.encode('utf-8')) # b'a'
```

# Byte Array
Array of Bytes (Just like a C string) But every byte is stored as an int value
```python
s = "Hello World"
b_arr = bytearray(s, 'utf-8') # bytearray(source, encoding)
print(b_arr) # bytearray(b'Hello World')
print(type(b_arr[0])) # <class 'int'>
```

Convert a list into a byte array
```python
lst = [1, 2, 3]
b_arr = bytearray(lst) 
print(b_arr) # bytearray(b'\x01\x02\x03')
```

# Byte Array Methods
```python
b_arr.append(48)
b_arr.extend(b'abc')
b_arr.extend('hello world'.encode())
```

# Find MD5 Hash of A String
```python
import hashlib

s = "Hello World"
b_arr = bytearray()
b_arr.extend(s.encode())
print(b_arr) # bytearray(b'Hello World')
m = hashlib.md5()
m.update(b_arr)
h_digest = m.hexdigest()
digest = m.digest()
print(h_digest) # b10a8db164e0754105b7a99be72e3fe5
print(digest) # b'\xb1\n\x8d\xb1d\xe0uA\x05\xb7\xa9\x9b\xe7.?\xe5'
```

# Byte Array to Byte Object

```python
a_arr = bytearray(b'\xb1\n\x8d\xb1d\xe0uA\x05\xb7\xa9\x9b\xe7.?\xe5')
a_obj = bytes(a_arr)
print(a_obj) # b'\xb1\n\x8d\xb1d\xe0uA\x05\xb7\xa9\x9b\xe7.?\xe5'
```

# Byte Object to Byte Array

```python
b_obj = b'\xb1\n\x8d\xb1d\xe0uA\x05\xb7\xa9\x9b\xe7.?\xe5'
b_arr = bytearray(b_obj) # bytearray(b'\xb1\n\x8d\xb1d\xe0uA\x05\xb7\xa9\x9b\xe7.?\xe5')
print(b_arr)
```

# Important Point
Neither bytes nor byte array are null terminated like C strings

# Reading A Binary File
```python
enc_flag_file = open("level4.flag.txt.enc", "rb") # A file pointer to the encrypted flag file
hashed_correct_pw_file = open("level4.hash.bin", "rb") # A file pointer to the hashed flag file

enc_flag = enc_flag_file.read() # Read the encrypted file and get bytes object
hashed_correct_pw = hashed_correct_pw_file.read() # Read the hashed password and get bytes object
```

# Different Ways of Seeing Bytes
```python
b_str = b'\n\x07\x04\x14\x0c\x15\x08\x06\x03\x07\x1b'
for c in b_str:
    print(f"\\x{hex(c)[2:]}", end="") # \xa\x7\x4\x14\xc\x15\x8\x6\x3\x7\x1b
print("\n")
for c in b_str:
    print(f"{hex(c)[2:]:02}", end=" ") # a0 70 40 14 c0 15 80 60 30 70 1b
```

# Basic XOR Encryption
```python
import sys
def str_xor(secret, key):
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i] # Pad/Expand the key to match the secret string
        i = (i + 1) % len(key)
    return "".join(chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret, new_key))


secret = 'humangasaur'
key = 'briu'
print(bytes(str_xor(secret, key), 'utf-8')) # b'\n\x07\x04\x14\x0c\x15\x08\x06\x03\x07\x1b'
```


