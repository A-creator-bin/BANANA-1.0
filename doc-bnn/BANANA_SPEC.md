# 🍌 BANANA Language — Full Specification

**Version:** 1.0
**Compiler:** `bananac.py` (backend via GCC/LLVM-style pipeline)
**Extension:** `.bn`

---

## Philosophy

BANANA is a statically typed compiled language inspired by Rust, with a 4-level user safety system built into the language itself. The compiler translates BANANA code into intermediate C which is then compiled with GCC into native code.

---

## Compilation Pipeline

```
source.bn → [Lexer] → Tokens

→ [Parser] → AST

→ [Analyzer] → Verified AST + security

→ [Codegen] → Intermediate C

→ [GCC/LLVM] → Native binary
```

---

## User Security System

BANANA has 4 user levels, in descending order of privilege:

```
root > a > b > c
```

| User | Level | Access |

|---------|-------|--------|

| `root` | 0 | Accesses everything (variables of all users) |

| `a` | 1 | Accesses variables of `a`, `b`, `c` |

| `b` | 2 | Accesses variables of `b`, `c` |

| `c` | 3 | Accesses only variables of `c` |

Variables and functions are annotated with the owner user:

```banana
root var x: int = 10; @ root is the owner — anyone can read it
a var segredo: int = 42; @ 'a' is the owner — b and c cannot access it
b var local_b: int = 7; @ 'b' is the owner — c cannot access it
c var privado: int = 1; @ 'c' is the owner — only 'c' can access it

```

Without a prefix, the property belongs to the current context.

---

## Keywords

| Keyword | Description |

|---------|-----------|

| `func` | Defines a function |

| `ret` | Returns the function value |

| `var` | Declares a variable |

| `int` | Integer type (64-bit) |

float type | Floating-point type (double) |

char type | Character type |

arr type | Array type |

if type | Conditional |

else type | Alternative to if |

while loop | While loop |

for loop | For loop |

mud type | Changes user context (MUD = MUDar) |

root user | Root user |

a user | User a |

b user | User b |

c user | c |

---

## Types

```banana
int @ 64-bit integer
float @ double (64-bit float)
char @ character
arr[int][10] @ array of 10 integers
arr[float][5] @ array of 5 floats
arr[char] @ string (char pointer)

```

---

## Comments

```banana
@ This is a comment — it starts with @ and goes to the end of the line
```

---

## Variable Declaration

```banana
var name: type = value; @ with initialization
var name: type; @ without initialization (zero by default)

@ With user prefix:
a var secret: int = 99;
root var global: float = 3.14;
```

---

## Functions

```banana
@ Root function (default)
func name(param1: type, param2: type) : ret_type {
ret value;

}

@ User function 'a'
a func helper(x: int) : int {
ret x + 1;

}

@ Void function (no return type)
func print(n: int) {
print_int(n);
}
```

---

## Operators

### Arithmetic
```banana
a + b @ addition
a - b @ subtraction
a * b @ multiplication
a / b @ division
a ^ b @ exponentiation (e.g., 2.0 ^ 8.0 = 256.0)
```

### Comparison
```banana
a == b @ equal
a != b @ not equal
a < b @ less than
a > b @ greater than
a <= b @ less than or equal to
a >= b @ greater than or equal to
```

### Logical
```banana
a && b @ logical AND
a || b @ logical OR
!a @ logical NOT
```

### Others
```banana
&a @ address of (reference)
a ? b : c @ ternary
a[i] @ array indexing
a.field @ field access
```

---

## Control Structures

### If / Else
```banana
if (condition) {

@ code
} else if (other) {

@ code
} else {

@ code
}
```

### While
```banana
while (condition) {

@ body
}
```

### For
```banana
for (var i: int = 0; i < 10; i = i + 1) {

@ body
}
```

---

## mud — User Context Switching

`mud` allows executing a block as a different user (only for users of the same level or lower):

```banana
@ root can switch to any user
mud a {

a var local: int = 42; @ belongs to 'a'

print_int(local);

}

mud b {

b var temp: int = 7;

print_int(temp);

}

@ ERROR: user 'b' cannot scale to 'a'

@ (detected at compile time!)

```

---

## Arrays

```banana
@ Fixed-size declaration
var nums: arr[int][10]; @ array of 10 ints, initialized with 0
var nums: arr[int][10] = 0; @ equivalent

@ Access
nums[0] = 42;

var x: int = nums[5];

@ Arrays of floats
var dados: arr[float][4];
dados[0] = 3.14;
```

---

## Built-in Functions

| Function | Description |

|--------|-----------|

| `print_int(n)` | Prints integer with newline |

| `print_float(f)` | Prints float with newline |

| `print_char(c)` | Prints char with newline |

| `print_str(s)` | Prints string with newline |

| `int_to_float(n)` | Converts int to float |

| `float_to_int(f)` | Converts float to int (truncates) |

---

## Complete Examples

### Hello World
```banana
func main() : int {

print_str("Hello from BANANA!");
ret 0;

}
```

###
