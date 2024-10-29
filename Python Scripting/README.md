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