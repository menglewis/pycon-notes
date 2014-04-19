title: Postgres Performance for Humans

####OLTP vs OLAP

* OLTP used for web apps
* OLAP used for BI/Reporting

####Setup/Config

* On Amazon, use Heroku or ‘postgres when its not your dayjob’
* Other clouds, ‘postgres when its not your dayjob’
* Real hardware, high performance Postgres

####Cache rules everything

Cache Hit Rate - query to get caching ratio, aim for 99% or higher

* if not, try giving more memory

Index Hit Rate

* Should aim for 95% or higher

###.psqlrc
Like bashrc but for psql

Sequential Scanning

* Scan on individual records
* Good for large reports
* Computing over lots of data (1k+ rows)

Index Scans

* Scan on indexes
* Good for small results
* Most common queries in app

####Understanding Query Performance
* EXPLAIN
* ANALYZE

###Rough Guidelines
* Page response times < 100ms
* Common queries < 10ms
* Rare queries < 100ms

pg_stat-statements

* Useful for profiling query performance

###Indexes
* Btree - usually what you want
* Generalized Inverted Index (GIN)
* Generalized Serach Tree (GIST)
     * Full text search
* KNN () - good for similarity
* SPace partioned GIST (SP-GIST) - good for phone numbers
* VODKA (coming soon)

Conditional 

* index on conditions like large population >10000
Functional
* Index on a JSON object in the field

Create index concurrently

* roughly 2-3x slower
* Doesn’t lock table (technically does for a few ms)

###hstore/JSON
* hstore is a key-value store
     * GIN/GIST indexes work
     * comparable to Mongodb
     * Not a full document store
* JSON is a full document store
     * Needs functional indexes to get performance up
* JSONb (coming)
     * Binary representation of JSON on disk

###Connection Pooling
Options

* Application/Framework layer
     * Automatic in Django 1.6+, SQLAlchemy
* Stand alone daemon


Backups

* Logical
* pg_dump
    * can be human readable, is portable

Physical

* The bytes on disk
* Base backup
* Used in replication

Logical

* Good across architectures
* Good for portability
* Has load on DB
* Works < 50 GB

Physical

* More initial setup
* Less portability
* Limited load on system
* Use above 50 GB