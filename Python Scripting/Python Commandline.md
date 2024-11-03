# Argparse
```python
import argparse

msg = "Adding Description" # Help Message
# Initialize Parser

parser = argparse.ArgumentParser(description = msg)

# Adding Optional Arugment
parser.add_argument("-o", "--Output", help="Show Output")

# Read Arguments From Command Line
args = parser.parse_args()

# Show Output in Terminal
if args.Output:
    print("Displaying Output as: % s" % args.Output)
```

# Types of Arguments
There are two types of arguments: Positional and Optional. 
Positional ones are those which do not need any specification to be invoked. Whereas, optional arguments need to be specified by their name first (which starts with ‘–‘ sign, ‘-‘ is also a shorthand.)


```python
# ---------------------------
#       add_num.py
# ---------------------------

import argparse

# Create A Parser Obejct
parser = argparse.ArgumentParser(description="An Addition Program")

# Add An Argument
parser.add_argument("add", nargs="*", metavar="num", type=int, help="All the numbers seperated bys spaces will be added.")

# Parse the arguments from standard intput
args = parser.parse_args()

# Check if add argument has any input data
# If it has, then print sum of the given numbers
if len(args.add) != 0:
    print(sum(args.add))
```

# Basic String to Hex Converter Tool
```python
import argparse
import os

def generate_hex_bytes(s):
    output = [hex(ord(c))[2:] for c in s]
    return " ".join(output)

def write_to_file(usr_inp, terminal_output_list, file_name):
    #print(f"{s}: Will write to file")
    print("Writing to file for: % s" % ", ".join(usr_inp))
    if os.path.isfile(file_name):
        os.remove(file_name)
    f = open(file_name, "a")
    f.write("Author: Pranjal Basak\n")
    f.write("--------------------------\n"*2)
    for (usr,line) in zip(usr_inp, terminal_output_list):
        f.write(f"{usr:{1+max(map(len, usr_inp))}}: {line}\n")
    f.close()
    f = open(file_name, "r")
    print("Wrote:", f.readlines())


def arg_handler():
    parser = argparse.ArgumentParser()
    parser.add_argument("hex", nargs="*", metavar="string", help="Converts your string into hex bytes")
    parser.add_argument("-o", nargs=1, metavar="output_file", help="Specify the output file name")
    args = parser.parse_args()
    #print(args.hex)
    if not args.o:
        for i in range(len(args.hex)):
            terminal_output = generate_hex_bytes(args.hex[i])
            print(f"{args.hex[i]}: {terminal_output}")
    else:
        terminal_output_list = map(generate_hex_bytes, args.hex)
        write_to_file(args.hex, terminal_output_list, args.o[0])



if __name__=="__main__":
    arg_handler()
```