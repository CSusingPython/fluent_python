1.The Python Data Model
========================
### One of the best qualities of Python is its consistency
### The special method names are always spelled with leading and trailing double underscores, i.e. __getitem__. 

#### The special method names allow your objects to implement, support and interact with basic language constructs such as:
- iteration;
- collections;
- attribute access;
- operator overloading;
- function and method invocation;
- object creation and destruction;
- string representation and formatting;
- managed contexts (i.e. with blocks);

-------------------------------------------------------------------
##### A deck as a sequence of cards

```python
import collections

Card = collections.namedtuple('card',['rank','suit'])

class FrenchDeck:
  ranks = [str(n) for n in range(2, 11)] + list('JQKA')
  suits = 'spades diamonds clubs hearts'.split()
  def __init__(self):
    self._cards = [Card(rank, suit) for suit in self.suits
                                    for rank in self.ranks]
  def __len__(self):
    return len(self._cards)
  def __getitem__(self, position):
    return self._cards[position]
```

####
We’ve just seen two advantages of using special methods to leverage the Python Data
Model:
1.   The users of your classes don’t have to memorize arbitrary method names for stan‐
dard operations (“How to get the number of items? Is it .size() .length() or what?”)
2.   It’s easier to benefit from the rich Python standard library and avoid reinventing
the wheel, like the random.choice function.


----------------------------------------------------
## How special methods are used
### 
1.  special methods is that they are meant to be called by the Python interpreter
2. But for built-in types like list, str, bytearray etc., the interpreter takes a shortcut
3. Normally, your code should not have many direct calls to special methods.
4. The only special method that is frequently called by user code directly is __init__, to invoke the initializer of the superclass in your own __init__ implementation.


## Emulating numeric types
###  Several special methods allow user objects to respond to operators such as +
###   more detail in Chapter 13 ,N*N will in Chapter 14

```
v = Vector(3,4)
abs(v)
v*3
abs(v*3)
```

### Next code is a Vector class implementing the operations just described, through the use of the special methods __repr__, __abs__, __add__ and __mul__:

```python
from math import hypot
class Vector:

   def __init__(self, x=0, y=0):
    self.x = x
    self.y = y
  
  def __repr__(self):
    return 'Vector(%r, %r)' % (self.x, self.y)
  
  def __abs__(self):
    return hypot(self.x, self.y)
  
  def __bool__(self):
    return bool(abs(self))
  
  def __add__(self, other):
    x = self.x + other.x
    y = self.y + other.y
    return Vector(x, y)
  
  def __mul__(self, scalar):
    return Vector(self.x * scalar, self.y * scalar)
    
 ```
 #### four special methods (apart from __init__), none
of them is directly called within the class or in the typical usage of the class illustrated
by the console listings.the Python interpreter is the only frequent caller of most special methods


### String representation
 추가 필요 
 

##  Arithmetic operators
###  __add__ and__mul__.  both cases, the methods create and return a new instance of Vector, and do not modify either operand — self or other are merely read.
 at Chapter13 ^^
  
##  Boolean value of a custom type


### By default, instances of user-defined classes are considered truthy, unless either __bool__ or __len__ is implemented. 
###  Basically, bool(x) calls x.__bool__() and uses the result. If __bool__ is not implemented, Python tries to invoke x.__len__(), and if
that returns zero, bool returns False. Otherwise bool returns True.
 

## Overview of special methods

###  The Data Model page of the Python Language Reference lists 83 special method names,
47 of which are used to implement arithmetic, bitwise and comparison operators. As an overview of what is available, seethe table in the book

### Why len is not a method?
 #### No method is called for the built-in objects of CPython: the length is simply read from a field in a C struct. 
 len is not called as a method because it gets special treatment as part of the Python Data Model, just like abs. But thanks to the special method __len__ you can also make len work with your own custom objects.
 
 ## summary
 ####
 By implementing special methods, your objects can behave like the built-in types--Pythonic
 A basic requirement for a Python object is to provide usable string representations of itself, one used for debugging and logging, another for presentation to end users. That is why the special methods __repr__ and __str__ exist in the Data Model.
 
 1. sequence types is thesubject of Chapter 2
 2. in Chapter 10 we will create a multi dimensional extension of the Vector class
 3. including reversed operators and augmented assignment will be shown in Chapter 13 via enhancements of the Vector example
