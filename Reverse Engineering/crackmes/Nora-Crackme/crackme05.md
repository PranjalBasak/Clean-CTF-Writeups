# Download the Challenge
[Original GitHub Repo](https://github.com/NoraCodes/crackmes/tree/master)

All of the challenges are created by @NoraCodes. You can see the source codes if you want but I would not suggest it. Just compile them and attempt the challenge yourself.

# Ghidra Decompilation
## Main Function
```C
undefined8 main(int param_1,char **param_2)

{
  int check_with_mod_result;
  size_t strlen;
  char *usr_input;
  
  if (param_1 != 2) goto LAB_001012ab;
  usr_input = param_2[1];
  strlen = strnlen(usr_input,1000);
  if ((int)strlen == 16) {
    if (usr_input[2] != 'B') goto LAB_0010127b;
    if (usr_input[13] != 'Q') goto LAB_00101283;
    check_with_mod_result = check_with_mod(usr_input,4,3);
    if (check_with_mod_result == 0) goto LAB_0010128b;
    check_with_mod_result = check_with_mod(usr_input + 4,4,4);
    if (check_with_mod_result == 0) goto LAB_00101293;
    check_with_mod_result = check_with_mod(usr_input + 8,4,5);
    if (check_with_mod_result == 0) goto LAB_0010129b;
    check_with_mod_result = check_with_mod(usr_input + 12,4,4);
    if (check_with_mod_result == 0) {
      fail(usr_input);
      goto LAB_00101273;
    }
  }
  else {
LAB_00101273:
    fail(usr_input);
LAB_0010127b:
    fail(usr_input);
LAB_00101283:
    fail(usr_input);
LAB_0010128b:
    fail(usr_input);
LAB_00101293:
    fail(usr_input);
LAB_0010129b:
    fail(usr_input);
  }
  succeed(usr_input);
LAB_001012ab:
  puts("Need exactly one argument.");
  return 0xffffffff;
}
```

## check_with_mod() function
```C
bool check_with_mod(char *usr_input,int num_1,int num_2)

{
  int ret_status_code;
  char *char_at_num_1;
  
  if (num_1 < 1) {
    ret_status_code = 0;
  }
  else {
    char_at_num_1 = usr_input + num_1;
    ret_status_code = 0;
    do {
      ret_status_code = ret_status_code + *usr_input;
      usr_input = usr_input + 1;
    } while (usr_input != char_at_num_1);
  }
  return ret_status_code % num_2 == 0;
}
```

It is pretty obvious what we need to do. The program sends index 0,4,8 and 12 in four function calls to `check_with_mod()`. They are adding up the decimal values of 4 consecutive characters in each case and checking if the summation is divisible by `num_2`. We can easily write a keygen:

```python
def keygen(num_1, num_2, first_c):
    tmp = first_c * num_1
    if tmp % num_2 == 0:
        return [chr(first_c) for i in range(num_1)]
    else:
        return [chr(first_c) for i in range(num_1)] + [chr(first_c-(tmp % num_2))] # We can just subtract the remainder from the last character


usr_input = ['a' for i in range(16)]
usr_input[2] = 'B'
usr_input[13] = 'Q'

usr_input[:4] = keygen(4, 3, ord('B')) # Since usr_input[2] = 'B'
usr_input[4:8] = keygen(4, 4, ord('Z')) # We could pick anything
usr_input[8:12] = keygen(4, 5, ord('Z')) # Same here, anything goes
usr_input[12:16] = keygen(4, 4, ord('Q')) # Since usr_input[13] = 'Q'
print("".join(usr_input))
```