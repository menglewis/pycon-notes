title: Lighting Talks 4-11

##Choose Pandas
Why pandas?
* Keep everything in Python
* Community support/resources
* Great for preprocessing, analysis and visualization

##Documenting History or How to Write Great Commit Messages

* Important reasons to use version control
    * memory
    * collaboration
* Assume the person who will be maintaining your code in 2 years from now is an axe wielding maniac who knows where you live
* Tell WHY you made the change

Rules

1. Tell me WHAT you did and WHY
2. Brevity is the soul of wit
    * But would rather have too much info than too little
3. Pick a style and stick with it
4. Pick a grammatical mode and stick with it
    * Present tense imperative - "Replace IOError with OSError" - YES
    * Past tense - "Replaced IO.."
    * Present tense with gerund - "Replacing IOError..." - NO
5. Spelling Counts
6. Teamwork counts

## dh-virtualenv

Mixes virtualenv with debian packages. 

## Machine Learning in Minutes

Identification Trees

* Which question to use at base of the tree?
* Pick questions that split up the data set the most

##Bleeding Edge packages: for users and developers alike

* Try dev version of a new package, it depends on the dev version of another package!
* bep - tool for managing bleeding edge packages
* Allows install, updates, remove and list installed packages
* Packages built and installed in user's home directory
* Can turn on/off packages
* specify -l or --language
* Can specify different repo types, git vs hg
* bep turn_off ipython

##Home brewing with Python
Why

* Home brewing is mainly manual
* Error prone, lots of little things can create off-flavors
* Lack of control
* Lack of visibility on progress

Architecture

* Raspberry PI
* Simple circuit boards for controlling/reading temp using GPIO pings
* interfaced with using python RPIO lib
* Metrics collection service on server
* Web interface for storing brew logs

* heats external water supply and pumps it through copper coils
* Pump has max execution times and lapse times to prevent overheating
* There is a critical range, outside of which alarms are sent to notify relevant people to fix the brew (Don't spam people)

##Distributing your Python Game
* Build a Windows Executable
* py2exe
* cx_freeze

What's wrong?

* copy-paste solution
* works only on specific paths
* hard to explain
* you have to own, install and run Windows

pyg.exe created to do a simple py2exe for pygame

##mpld3: Bridging Matplotlib and D3js
Used to create interactive figures with D3

* easy to embed in any static website
* Allows for plugins

##Server Security 101
* fail2ban
* Use IPTables to block IPs known to cause problems
* Disable password auth in sshd_config
* Always uses Private/Public keys to connect
* Disable SSHv1, only use SSHv2
* keep minimal set of packages installed
* Configure and customize PAM

* NEVER SSH with root
* always su -root
* Never have admins share the same accounts
* Keep users for separate processes separate
    * If a service is exploited, it is limited to that service
* Configure /etc/security/limits.conf
* Partition the hard drive if possible

##Google Crisis Map
Publishing maps for disasters and humanitarian aid

Can add layers from different sources to give data about a crisis that is placed on the map

##Seven Deadly Sins of Software Deployment
* Sloth 
    * No docs
    * No deployment scripts
* Greed
    * Cheap staging servers
    * not enough disk space
    * different OS release
* Gluttony
    * new OS versions
    * new libraries
    * new app code
* Pride
    * Who needs a rollback plan when your code is perfect?
* Lust
    * Bleeding edge, alpha versions
* Envy
    * Avoid communication
* Anger
    * Keep trying without rollback

