---
title: "Code Example"
date: 2019-02-19T11:07:33-05:00
---

![codedna](/images/dna.jpg)

# Rendering Code  

Let's see how Hugo and Markdown do with displaying some code text...  

#### some_file.py  
``` python  
"""
This is an example snippet of Python code.
"""

import this
from thisFile import functionA

Class HugoClass():
    pass  

Class SubHugoClass(HugoClass):
    """A docstring here explaining what this class does."""

    def __init__(self, name, age):
        """Initialize name age and age attributes."""
        # Make accessible attributes or variables:
        self.name = name
        self.age = age

    # Some method
    def method_a(self):
        # do something here,
        # and here

    # More methods can follow here

# Make an instance representing a specific SubHugoClass  

# Calling methods  

# Make multiple instances  
```  

#### bash-script  
```bash  
$ mkdir new-dir
$ cd new-dir
$ wget https://somethingheretowget.git
```  