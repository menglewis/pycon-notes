title: All Your Ducks In a Row - Data Structures in the Standard Library and Beyond


###Addresses
* You can add and subtract to visit other addresses near one
* Records
    * Sequence of fields in an array
    * Retrieve a field from a record by adding the start address with the offset
    * Addition, heterogeneous

### Arrays
* GIven an array with items b bytes long, character i lives at (a + bi)
* Multiplication, homogeneous
* Array is useful for binary conversations with C libraries or I/O
    * Specialized arrays
        * str
        * unicode
        * StringIO
        * memoryview
        * bytearray
        * array
        * Accessing items in an array requires 
    * Numpy
        * Python array - for C or IO
        * Numpy array for math operations
        * Supports math operations without requiring intermediate python objects

###Python itself is a dynamic language, everything is an object

* main data strucutres are going to be general
* Can hold any type of object
* Array address math depends on every element being the same number of bytes long
* Tuple does not store the objects
    * Has an array of addresses that refer to the elements in the tuple
* All of the Python general data structures only store addresses to the obejects
* There is only one copy of an object regardless of how any tiems it appears in a tuple or list


###Tuple

* Supports bounds checking because a tuple knows its length

###List

* Like a tuple but it can grow
* The list might need to move if it’s hemmed in in memory
* But Python objects can never change address
* List contains length, size of storage and address of a block that stores references to list objects
* 2 levels of indirection
* Has additional address because otherwise every append would require reallocation which is expensive
* Number of extra spaces increases the more 
* Amortization
* To save time, we waste space with all of those unused list items
* Speed vs space tradeoff
* Python lists on average use 94% of their slots, consume 6% more space for much faster speed
* List is the most dangerous data structure
    * Some single item operations touch more items in the list
    * Popping from the start quickly after 

###Dict

* Behind each dict is an array, in which keys are stroed d indexes according to their hash value
* Each slot contains hash, key value, value value
* Resize cost is amortized
* Usually 1/3 to 2/3 full
* Speed vs space
* Given a key, the dict hashes it and can go straight to its record
* See 2010 talk on Dictionaries
* Forgets order because it hashes the keys
* Only maintains associations
* Defaultdict
* Counter class
* frozenset
    * If data in your keys is unordered
* OrderedDict
    * Used for dicts that remembers order that keys were added
    * Useful for email headers, JSON objects
* Class each attribute is a key in obj.__dict__
    * But classes that pre-sepcify __slots__ implement as a struct
* Fetching a range
    * Use bisect if your data is already sorted in a list or array
* Double ended list
    * Use deque
    * deque allows pop, appends on both ends efficientyly
    * Data structure behind Queue
* Popularity contest
    * heapq lets you fetch top t of n items in linear time
    * Can push and pop in any order
    * Has operations which get the objects without having to sort all the items in the structure
* PriorityQueue
    * Heapq with locks so threads can cooperate on a priotirtzed
* Scheduler
    * Tells you when the next task is due to run

###Traditional data structures don’t make sense in Python

* Because data structures in Python don’t contain data, only addresses
* Deque and ORdereddict have doubly linked lists on the back
* Trees used for tree shaped data
* Outsoourced trees to databases

###Goes toward functional programming styles that involves quick write once throw away data strucutres

* List comprehensions
* Dict comprehensions

