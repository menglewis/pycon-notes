title: "Django: The Good Parts"

Aren't other frameworks smaller/lighter and thus better?

###The Good Parts
* HTTP Abstraction + request/response cycle
* Convervative approach to adding features
* Secret weapon: apps

###HTTP sucks
* 9 different request methods, some safe, some idempotent
* Unicode unaware
* Persistence, auth, streaming, scurity all tacked on/afterthoughts

CGI isn't much fun either

###How to CGI (in theory)
* Script invoked by web server, request info stored in env variables
* submitted data reieved via standard input
* script does processing
* Response headers and response body sent via standard output

###How to Actually CGI
* Script invoked by web server, you hope if everything's configured correctly
* chmod 777 ALL the things
* Environment is full of lies and sadness
* Give up figureing what the actual request URL was and fork() bomb the server
* Drink. A lot.

###WSGI is basically CGI with a bit of Python

###WSGI pain points
* CGI style environ: have to pass it around, parse it, re-parse it, etc.
* Signaling up and down the stack requires inventing ad-hoc protocols
* Return/Response API sucks.
* Inherits HTTP's awful approach to character encoding

##Let's do better
* HTTP Request
    * Already parsed and normalized for you
    * Sensible attribute and dictionary based access
* HTTP REsponse
    * Easy to construct and manipulate
    * Concveneint subclasses for comon specialized responses (404, redirects)
* Write a callable that takes and HTTPRequest and either returns an HTTPREsponse or raises an exception
* URL parsing done for you


###Life Cycle
* Middleware system bakend in (all middleware systems suck, though)
* Signals
* Decorators
* Call other stuff - callables all the way down

###Sane HTTP/WSGI is worth importing some stuff

## Conservative approach to adding new features

* Migrations coming in 1.7 (took 6 years)
* Things get into Django slowly
* Now they're getting out
    * Unbundling/removing many things in contrib and other parts
* Things are difficult to add, easier to remove
* This is usually described as a bad thing
    * Not necessarily a bad thing
    * Strong preference for community solutions
    * Core framework more about providing API support
    * More swappable components than expected
    * More stability over time
    * Features land when ready
    * Competition often produces better solutions (South a good example of that)

## Encapsulation
* A Django project is a WSGI application.
* Django applications are encapsulated, pluggable functionality
* Do one thing and do it well

###The Bad parts
* Internals are messy
* Too many meta classes
* ORM is a minefield
* Needs a good cleaning up of the internal
* App loading refactor in 1.7 is good
