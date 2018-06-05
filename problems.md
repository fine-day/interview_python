## mutable args 

```
def extendList(val, list=[]):
    list.append(val)
    return list

list1 = extendList(10)
list2 = extendList(123,[])
list3 = extendList('a')

print "list1 = %s" % list1
print "list2 = %s" % list2
print "list3 = %s" % list3


# output
list1 = [10, 'a']
list2 = [123]
list3 = [10, 'a']
```

```
def extendList(val, list=None):
  if list is None:
    list = []
  list.append(val)
  return list
  
list1 = [10]
list2 = [123]
list3 = ['a']
```

```
list = [ [ ] ] * 5
list  # output?
list[0].append(10)
list  # output?
list[1].append(20)
list  # output?
list.append(30)
list  # output?


# output
[[], [], [], [], []]
[[10], [10], [10], [10], [10]]
[[10, 20], [10, 20], [10, 20], [10, 20], [10, 20]]
[[10, 20], [10, 20], [10, 20], [10, 20], [10, 20], 30]

# list = [ [ ] ] * 5 does NOT create a list containing 5 distinct lists; rather, it creates a a list of 5 references to the same list.

```

## closure

```
def multipliers():
  return [lambda x : i * x for i in range(4)]
    
print [m(2) for m in multipliers()]

# The output of the above code will be [6, 6, 6, 6] (not [0, 2, 4, 6]).
# The reason for this is that Python’s closures are late binding. 

# some solutions

def multipliers():
  for i in range(4): yield lambda x : i * x 
  
# or
def multipliers():
  return [lambda x, i=i : i * x for i in range(4)]
# or
from functools import partial
from operator import mul

def multipliers():
  return [partial(mul, i) for i in range(4)]

# or
def multipliers():
  return (lambda x : i * x for i in range(4))

```

## Class

```
class Parent(object):
    x = 1

class Child1(Parent):
    pass

class Child2(Parent):
    pass

print Parent.x, Child1.x, Child2.x
Child1.x = 2
print Parent.x, Child1.x, Child2.x
Parent.x = 3
print Parent.x, Child1.x, Child2.x

# output
1 1 1
1 2 1
3 2 3
```



