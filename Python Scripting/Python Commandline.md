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