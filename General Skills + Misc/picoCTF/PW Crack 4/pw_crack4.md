# Intro
We get three files
- A Python Flag Verification File
- An Encrypted Flag File
- An MD5 Hashed Password File Required to Decrypt The Flag

# Python Code
```python
import hashlib

### THIS FUNCTION WILL NOT HELP YOU FIND THE FLAG --LT ########################
def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])
###############################################################################

flag_enc = open('level4.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level4.hash.bin', 'rb').read()


def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()


def level_4_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    user_pw_hash = hash_pw(user_pw)
    
    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")



level_4_pw_check()



# The strings below are 100 possibilities for the correct password. 
#   (Only 1 is correct)
pos_pw_list = ["6288", "6152", "4c7a", "b722", "9a6e", "6717", "4389", "1a28", "37ac", "de4f", "eb28", "351b", "3d58", "948b", "231b", "973a", "a087", "384a", "6d3c", "9065", "725c", "fd60", "4d4f", "6a60", "7213", "93e6", "8c54", "537d", "a1da", "c718", "9de8", "ebe3", "f1c5", "a0bf", "ccab", "4938", "8f97", "3327", "8029", "41f2", "a04f", "c7f9", "b453", "90a5", "25dc", "26b0", "cb42", "de89", "2451", "1dd3", "7f2c", "8919", "f3a9", "b88f", "eaa8", "776a", "6236", "98f5", "492b", "507d", "18e8", "cfb5", "76fd", "6017", "30de", "bbae", "354e", "4013", "3153", "e9cc", "cba9", "25ea", "c06c", "a166", "faf1", "2264", "2179", "cf30", "4b47", "3446", "b213", "88a3", "6253", "db88", "c38c", "a48c", "3e4f", "7208", "9dcb", "fc77", "e2cf", "8552", "f6f8", "7079", "42ef", "391e", "8a6d", "2154", "d964", "49ec"]
```

# Insight
Let's understand what the code does step by step

1. level_4_pw_check() is the "main" function
2. We find the MD5 hash of user provided password (might be wrong) using `hash_pw()`
3. If the hashed value is correct, we run an XOR decryption using `str_xor()` and check if it is correct

# Step 1: Find Out The Correct Password
```python
import hashlib
def hash_pw(user_pw):
    byte_pw = bytearray()
    byte_pw.extend(bytes(user_pw, 'utf-8'))
    m = hashlib.md5()
    m.update(byte_pw)
    return m.digest()

pos_pw_list = ["6288", "6152", "4c7a", "b722", "9a6e", "6717", "4389", "1a28", "37ac", "de4f", "eb28", "351b", "3d58", "948b", "231b", "973a", "a087", "384a", "6d3c", "9065", "725c", "fd60", "4d4f", "6a60", "7213", "93e6", "8c54", "537d", "a1da", "c718", "9de8", "ebe3", "f1c5", "a0bf", "ccab", "4938", "8f97", "3327", "8029", "41f2", "a04f", "c7f9", "b453", "90a5", "25dc", "26b0", "cb42", "de89", "2451", "1dd3", "7f2c", "8919", "f3a9", "b88f", "eaa8", "776a", "6236", "98f5", "492b", "507d", "18e8", "cfb5", "76fd", "6017", "30de", "bbae", "354e", "4013", "3153", "e9cc", "cba9", "25ea", "c06c", "a166", "faf1", "2264", "2179", "cf30", "4b47", "3446", "b213", "88a3", "6253", "db88", "c38c", "a48c", "3e4f", "7208", "9dcb", "fc77", "e2cf", "8552", "f6f8", "7079", "42ef", "391e", "8a6d", "2154", "d964", "49ec"]

hashed_correct_pw = open("level4.hash.bin", "rb").read() # Correct Hash Value in bytes object format

for pw in pos_pw_list:
    hashed_attempted_pw = hash_pw(pw) # Attempted Password's Hashed Value in bytes object format
    if hashed_attempted_pw == hashed_correct_pw:
        print("Successful:", pw)
        print("---------------")
        break
    else:
        print("Failure:", pw)
```

## Output
```
Failure: 6288
Failure: 6152
Failure: 4c7a
Failure: b722
Failure: 9a6e
Failure: 6717
Failure: 4389
Failure: 1a28
Failure: 37ac
Failure: de4f
Failure: eb28
Failure: 351b
Failure: 3d58
Failure: 948b
Failure: 231b
Successful: 973a
---------------
```

# Step 2: XOR Decryption
```python
def str_xor(secret, key):
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i] # Pad/Expand the key to match the secret string
        i = (i + 1) % len(key)
    return "".join(chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret, new_key))


key = "973a" # Correct Password
secret = open("level4.flag.txt.enc", "rb").read() # Encrypted Flag in Bytes Object Format
flag = str_xor(secret.decode(), key) # Must Decode Bytes Object Flag into UTF-8 String (Default Python String) before XOR Decryption
print(flag)
```

## Output
```
picoCTF{fl45h_5pr1ng1ng_ae0fb77c}
```