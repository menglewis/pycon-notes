title: Lightning Talks 4-12

###NYC Subway Challenge

* Challenge to ride the NYC subway and visit all the stations as fast as possible

* Montreal metro challenge
* Used algorithms such as Depth-first search and Dykstra's algorithm to determine the optimal route through the Montreal metro system
* [Source at Github](github.com/leafstorm/montreal-metro)


###Scrape

* An interactive scraping tool using selenium and a command line to create web scraping scripts in an interactive environment
* Easy enough to be used by non-programmers (still have to be tech savvy)
* Used by grad students at University of Chicago to scrape data from journals
* [Source code at Github](https://github.com/yarko/scrape)
* [Docs at Readthedocs](http://scrape.readthedocs.org/en/latest)

###Pair Programming at Hackbright

Benefits of Pair Programming

* Helps keep focus
* People learn by teaching
    * Even if there is a skill gap in the 2 people, the one with a greater skill will solidify their skills by teaching and the other person will benefit from having a more experienced person helping them
* Driver/Navigator dynamic
    * Important to trade off frequently
* Helps improve communication skills
* Continuous learning

###Giving Logs Structure

* A good log entry
    * Contains all relevant data
    * Is easily comprehensible
* Does not necessarily have to be comprehensible to humans
    * If it is comprehensible to a computer, then it can output the data in more useful forms such as graphs and charts with the option to view data on specific logs at specific times
* structlog
    * Wraps existing log structure with a context
    * [Source at Github](https://github.com/hynek/structlog)
    * [Docs at Readthedocs](http://www.structlog.org/en/0.4.1/)

###Improving Django performance with one line of code

* If there are lots of URL rules on a Django project, it can cause slowdown if it surpasses the default cache that is set on Regular expressions
* The behavior in the re module is such that surpassing the cache causes it to clear the cache
    * This can cause a large slowdown on processing URL routes

Use the following snippet to set the cache to a larger value. It defaults to 100.

    #!python
    re._MAXCACHE = 1000 # Or larger if necessary


