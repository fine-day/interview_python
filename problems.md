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

## Test

```
How would you unit-test the following code?

async def logs(cont, name):
    conn = aiohttp.UnixConnector(path="/var/run/docker.sock")
    async with aiohttp.ClientSession(connector=conn) as session:
        async with session.get(f"http://xx/containers/{cont}/logs?follow=1&stdout=1") as resp:
            async for line in resp.content:
                print(name, line)


# A good answer would suggest a specific async mock library and async test case approach, including an ephemeral event loop that’s guaranteed to terminate (i.e. with a max number of steps before timeout.)

# A great answer would point out that synchronisation problems are fundamentally the same in synchronous and asynchronous code, the difference being preemption granularity.

# A beautiful answer would take into account that the above code only has one flow (easy) compared to some other code where flows are mixed (e.g. merging two streams into one, sorting, etc). For example, consider following upgrade to the given code:

# Here, any of the async statements could have a side-effect of changing the global keep_running.

keep_running = True

async def logs(cont, name):
    conn = aiohttp.UnixConnector(path="/var/run/docker.sock")
    async with aiohttp.ClientSession(connector=conn) as session:
        async with session.get(f"http://xx/containers/{cont}/logs?follow=1&stdout=1") as resp:
            async for line in resp.content:
                if not keep_running:
                    break
                print(name, line)
                

```


## Mixin

```
Given a list of N numbers, use a single list comprehension to produce a new list that only contains those values that are:
(a) even numbers, and
(b) from elements in the original list that had even indices

For example, if list[2] contains a value that is even, that value should be included in the new list, since it is also at an even index (i.e., 2) in the original list. However, if list[3] contains an even number, that number should not be included in the new list since it is at an odd index (i.e., 3) in the original list.

ans:
[x for x in list[::2] if x%2 == 0]
```


