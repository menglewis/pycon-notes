title: Lightning Talks 4-13

####Django settings with django-configurations

Bridge between Django's module settings system and class based views

    #!python
    from configurations import Configuarion

    class Dev(Configuration):
        Debug = True

Setup environment

    #!bash
    export DJANGO_SETTINGS_MODULE = 'mysite.settings'
    export DJANGO_CONFIGUARTION = 'dev'


Applies the class content to the containing module before Django reads it.

Can use

* Mixins
* Facades 
* Factories
* Adapters

###Architecture of Open Source Applications
http://aosabook.org/en/index.html


###Lightning Python
Native Python on Windows without Visual studio

###Falcon
* WSGI framework for building APIs
* Very low latency
* Extremely high throughput
* Maintainable at scale

