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