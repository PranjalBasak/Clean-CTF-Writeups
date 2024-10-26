# Link to the Crackme Challenge
[Challenge](https://crackmes.one/crackme/66c724b9b899a3b9dd02ad98)

# Level 4

---

# Ghidra Decompilation

# Initialized Variables
```C
  magic_yapping[0] = "Hunga";
  magic_yapping[1] = "Bunga";
  magic_yapping[2] = "Funga";
  magic_yapping[3] = "Lunga";
  magic_yapping[4] = "Skrunga";
```

```C
  symbol_rain[0] = '@';
  symbol_rain[1] = '$';
  symbol_rain[2] = '+';
  symbol_rain[3] = 'Z';
  symbol_rain[4] = 'Q';
  symbol_rain[5] = 'B';
  symbol_rain[6] = '*';
  symbol_rain[7] = 'J';
  symbol_rain[8] = '&';
  symbol_rain[9] = '%';
  symbol_rain[10] = '|';
  symbol_rain[11] = '{';
  symbol_rain[12] = ':';
  symbol_rain[13] = '?';
  symbol_rain[14] = '<';
  symbol_rain[15] = '>';
  symbol_rain[16] = '!';
  symbol_rain[17] = 'X';
```

## Sus Array
It is a randomly generated array
```C
  for (idx = 0; idx < 6; idx = idx + 1) {
    iVar4 = rand();
    magic_yapping[(long)idx + 6] = magic_yapping[(long)(iVar4 % 0x28) + 0xc];
    sus[idx] = iVar4 % 0x28;
  }
```

# Initial Portion
```C
  current_time = time((time_t *)0x0);
  srand((uint)current_time);
  counter = 0;
  rand_1 = rand();
  rand_1 = rand_1 % 4;
  rand_2 = rand();
  rand_2 = rand_2 % 3;
  rand_3 = rand();
  rand_3 = rand_3 % 5;
  symbol_dance[4] = "@";
  symbol_dance[5] = "$";
  symbol_dance[6] = "*";
  symbol_dance[7] = "+";
  symbol_dance[0] = "@";
  symbol_dance[1] = "\\";
  symbol_dance[2] = "&";
  magic_yapping[0] = "Hunga";
  magic_yapping[1] = "Bunga";
  magic_yapping[2] = "Funga";
  magic_yapping[3] = "Lunga";
  magic_yapping[4] = "Skrunga";
  for (idx = 0; idx < 6; idx = idx + 1) {
    rand_4 = rand();
    magic_yapping[(long)idx + 6] = magic_yapping[(long)(iVar1 % 0x28) + 0xc];
    sus[idx] = iVar1 % 0x28;
  }
```

# Important Portion
```C
  fgets(usr_input,100,stdin);
  strlen = strcspn(usr_input,"\n");
  usr_input[strlen] = '\0';
  for (i = 0; i < 6; i = i + 1) {
    if (sus[i] < 0x15) {
      counter = counter + 1;
    }
    else {
      counter = counter + -1;
    }
  }
  if (counter < 0) {
    single_bullet = symbol_rain[1];
    passed_to_func[0] = symbol_rain[3];
    func_first_gate("\n\n*LEVEL 4 [! COMPLETE !] (How did you not rip your hair out?)*\n\n       {Pr ess Any Key to Return to the Menu}\n"
                    ,"\n\n*LEVEL 4 [X FAILED X] (Give it another try, reverser :-])",rand_1,
                    symbol_dance[(long)rand_1 + 4],symbol_rain,passed_to_func,usr_input,rand_3,
                    magic_yapping + rand_3,rand_2,symbol_dance[rand_2]);
  }
  else {
    single_bullet = symbol_rain[6];
    passed_to_func[0] = symbol_rain[2];
    func_first_gate("\n\n*LEVEL 4 [! COMPLETE !] (How did you not rip your hair out?)*\n\n       {Pr ess Any Key to Return to the Menu}\n"
                    ,"\n\n*LEVEL 4 [X FAILED X] (Give it another try, reverser :-])",rand_1,
                    symbol_dance[(long)rand_1 + 4],symbol_rain,passed_to_func,usr_input,rand_3,
                    magic_yapping + rand_3,rand_2,symbol_dance[rand_2]);
  }
```

```C
func_first_gate(
    "\n\n*LEVEL 4 [! COMPLETE !] (How did you not rip your hair out?)*\n\n       {Pr ess Any Key to Return to the Menu}\n",
    "\n\n*LEVEL 4 [X FAILED X] (Give it another try, reverser :-])",
    rand_1,
    symbol_dance[(long)rand_1 + 4],
    symbol_rain,
    passed_to_func,
    usr_input,
    rand_3,
    magic_yapping + rand_3, // magic_yapping[rand_3]
    rand_2,
    symbol_dance[rand_2]);
```

# Some more Stuff
```C
char passed_to_func [4];
```