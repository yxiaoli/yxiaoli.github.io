+++
title =  "Common Traps in Python - part 1"
tags = ["python", "SDE"]
categories = ["tech"]
description = "The first part of the python-traps series will explains the common mistakes we may encounter in list"
date =  2020-04-05T08:57:55+01:00
menu = ""
banner = "https://app.yinxiang.com/shard/s33/res/500de35a-8db4-4823-a516-af38d9ba1358/3.jpg"

images = []
+++


# Part 1 - The traps of List in Python

## 1. Assignment trap
`Note`:  Assignment with an = on lists does not make a copy. Instead, assignment makes the two variables point to the one list in memory.
```
 colors = ['red', 'blue', 'green']
b = colors ## Does not copy the list
b [1] = "yellow"
print (colors)   # ['red', 'yellow', 'green']
print(b)   # ['red', 'yellow', 'green']
```
and you may get really risky return
````
values = [0, 1, 2]
values[1] = values
print (values)  # [0, [..], 2]
````

use `copy` method!
		
	b = values.copy


## 2. List.expend(), List.append() and +

- list.append(elem) -- adds a single element to the end of the list. Common error: does not return the new list, just modifies the original.
- list.insert(index, elem) -- inserts the element at the given index, shifting elements to the right.
- list.extend(list2) adds the elements in list2 to the end of the list. Using **+ or +=** on a list is similar to using extend().

### 2.1 No Return 

Common error: note that the above methods do not *return* the modified list, they just modify the original list.

	 list = [1, 2, 3]
	 print list.append(4) ## NO, does not work, append() returns None
	 # Correct way:
	 list.append(4)
	 print list ## [1, 2, 3, 4]  

### 2.2 Difference Between Append and Extend Method

- **append**: Appends object at the end.
- **extend**: Extends list by appending elements from the iterable.  (equal to +)

![to illustrate the difference](https://app.yinxiang.com/shard/s33/res/6d2f875e-e773-4a82-a647-1aa7cbab8dbb) 

## 3. Initialization for Nested List
Q: Consider the two approaches below for initializing an array and the arrays that will result. How will the resulting arrays differ and why should you use one initialization approach vs. the other?

	>>> # INITIALIZING AN ARRAY -- METHOD 1
	...
	>>> x = [[1,2,3,4]] * 3>>> x
	[[1, 2, 3, 4], [1, 2, 3, 4], [1, 2, 3, 4]]
    

	>>> # INITIALIZING AN ARRAY -- METHOD 2
	...
	>>> y = [[1,2,3,4] for _ in range(3)]
	>>> y   # [[1, 2, 3, 4], [1, 2, 3, 4], [1, 2, 3, 4]]


	>>> # MODIFYING THE x ARRAY FROM THE PRIOR CODE SNIPPET:
	>>> x[0][3] = 99
	>>> x
	[[1, 2, 3, 99], [1, 2, 3, 99], [1, 2, 3, 99]]
	>>> # UH-OH, DON’T THINK YOU WANTED THAT TO HAPPEN!


	>>> # MODIFYING THE y ARRAY FROM THE PRIOR CODE SNIPPET:
	>>> y[0][3] = 99
	>>> y
	[[1, 2, 3, 99], [1, 2, 3, 4], [1, 2, 3, 4]]
	>>> # THAT’S MORE LIKE WHAT YOU EXPECTED!


![images to show ](https://app.yinxiang.com/shard/s33/res/0faddd4d-690d-42d6-89dc-c6eff10710ea.png)

# Reference:
https://developers.google.com/edu/python/lists

http://pythontutor.com/live.html#mode=edit
