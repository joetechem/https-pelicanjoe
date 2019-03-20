---
title: "Python Lists"
date: 2019-03-20T16:28:01-04:00
draft: true
---

## Looping Lists  

### Simple Statistics with a List of Numbers  

A few Python functions are specific to lists of numbers. For example, you can easily find the min, max, and sum of a list of numbers:  

```python  
digits = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
min(digits)
max(digits)
sum(digits)
```  

### List Comprehensions  

When we generated the list `squares`, this method consisted of using a few lines of code. A **list comprehension** allows you to generate this same list is only ONE line of code. List comprehension combines the `for` loop and the creation of new elements into one line, and automatically appends each new element.  

Let's take our square numbers from earlier. We'll build the same list, but using list comprehension this time:  

```python  
squares = [value**2 for value in range(1,11)]
print(squares)
```  

We begin with a descriptive name for the list (`squares`). Then, we define the expression for the values we want to store in the new list. Here, the expression is `value**2`, which raises the value to the second power. Then, write a `for` loop to generate the numbers we want to feed into the expression.  


### Working with a Section of a List  






