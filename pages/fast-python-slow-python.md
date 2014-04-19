title: Fast Python, Slow Python


* Benchmarks are full of lies
* Performance is about specialization

Macro vs Micro benchmarks

What Python isn't

* Cython
* C
* Numba
* RPython

Optimizing dynamic languages is different from static languages, but you can optimize it

You can monkey patch anything in Python
Slow vs harder to optimize

###PyPy
* Specializes and tries to optimize code when it runs

How can we specialize our code?

Dictionaries are not a replacing for Classes

Dicts vs objects

* Dictionaries are not specialized
    * A dict can represent any format
* Classes are specialized
    * Used to represent specific types of objects

Why don't we care?
For 20 years, dicts and classes had similar performance.
Now, and specifically with PyPy, classes are much faster

###Zero buffer
* library that ports C type ideas to Python in a Pythonic way


###Myths
* function calls are really expensive
* Only use builtin data types to make your code fast
* Don't write Python in the style of Java or C

