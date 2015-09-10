---
layout: page
title: Prima Adi P
---
## About
I'm Prima Adi Pradana 

``` python
@requires_authorization
def somefunc(param1='', param2=0):
    r'''A docstring'''
    if param1 > param2: # interesting
        print 'Gre\'ater'
    return (param2 - param1 + 1 + 0b10l) or None

class SomeClass:
    pass

>>> message = '''interpreter
... prompt'''
```

For further details on Emerald, please visit the [repository on Github](https://github.com/KingFelix/emerald/).

![Emerald](img/Emerald01.png "Emerald")

### Setup
Emerald may be installed by simply downloading the .zip folder from the [repository on Github](https://github.com/KingFelix/emerald/archive/master.zip).

After extracting the content from the folder into the selected directory, you can type ``jekyll serve`` from the terminal, than open your browser to ``0.0.0.0:4000/emerald/`` and you will find it there.

Additionally it is possible to fork the repository and use Github Pages as hosting. By following this way it will be enough to change the ``baseurl`` value into the ``_config.yml`` file, with the directory name of your project (for example /blog) or simply with a "/" (slash) if you want install Emerald in the root. 

Last but not least: the [Jekyll documentation](http://jekyllrb.com) is the best starting point!

Enjoy!