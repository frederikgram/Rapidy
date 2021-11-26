# corree
Command-line arguments in an _**"Ask and you shall receive"**_ fashion.

Pythons standard [argparser](https://docs.python.org/3/library/argparse.html) isn't very pretty, nor is it short, compact or to the point.  This tool is meant to fill the gap between "quick-and-dirty" and overly complex. 


However simple it is, this parser _does_ support type validation. Combine this with an easy-to-use system for specifying the number and types of arguments expected for a flag and you get a suprisingly powerful tool.

---

:notebook_with_decorative_cover: note: _the notion of raising exceptions is not one that is used here.  The "success" flag returned by _parse_args_ is to be used to validate proper responses_


## Example
```python
""" corree Usecase Example """
import corree

# Declare wanted flags, args and types
declared_args = {
    "age": float,
    "name": str,            # A single argument of the specified type (place inside bracket for required arguments)
    "foods": [str],         # 0 .. n arguments of any type specified inside the brackets
    "numbers": [int],
    "domain-name": str, 
    "likes-cake": bool,     # bool, takes no arguments except the flag itself.
    "IP-PORT":  (str, int), # both the count and order of types are required for successful parsing
},



# This would be sys.argv[1:] usually, the parser handles
# both strings as well as lists of strings as input
given_input = """-name johndoe -age 42.2 --domain-name python.org --likes-cake /
-foods banana pineapple pizza oreos --IP-PORT 192.0.0.1 8080 -numbers 1 2 3 4"""

# Parse input
success, result = corree.parse_args(
    declared_args, given_input
)
# Output
>> success, result
True,
{
    'age': 42.2,
    'name': 'johndoe',
    'foods': ['banana', 'pineapple', 'pizza', 'oreos'],
    'numbers': [1, 2, 3, 4],
    'domain-name': 'python.org',
    'likes-cake': True,
    'IP-PORT': ("192.0.0.1", 8080)
}

@NOTE: Unspecified optional arguments won't appear in the result dictionary,
you should check if the key exists for non-required arguments.
