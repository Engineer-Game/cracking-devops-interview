# Programming

## Programming

### Math

Probabilities to have two dices with 9

Calculate 2^22 (2^11 \* 2^11)

### Fizzbuzz

```
for num in xrange(1,101):
    if num % 5 == 0 and num % 3 == 0:
        msg = "FizzBuzz"
    elif num % 3 == 0:
        msg = "Fizz"
    elif num % 5 == 0:
        msg = "Buzz"
    else:
        msg = str(num)
    print msg
```

### Trees

#### Detect the mirror and symmetric of a ree

```
class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None

def isMirror(root1 , root2):
    if root1 is None and root2 is None:
        return True

    """ For two trees to be mirror images, the following three
        conditions must be true
        1 - Their root node's key must be same
        2 - left subtree of left tree and right subtree
          of right tree have to be mirror images
        3 - right subtree of left tree and left subtree
           of right tree have to be mirror images
    """
    if (root1 is not None and root2 is not None):
            if  root1.key == root2.key:
                return (isMirror(root1.left, root2.right)and
                isMirror(root1.right, root2.left))

    # If neither of above conditions is true then root1
    # and root2 are not mirror images
    return False

def isSymmetric(root):
    return isMirror(root, root)

root = Node(1)
root.left = Node(2)
root.right = Node(2)
root.left.left = Node(3)
root.left.right = Node(4)
root.right.left = Node(4)
root.right.right = Node(3)
print "1" if isSymmetric(root) == True else "0"
```

#### Max Height

```
class Node:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

def height(n):
    if n == None:
        return 0

    return max(height(n.left), height(n.right)) + 1

node = Node(5)
node.left = Node(8)
node.right = Node(3)
node.left.left = Node(3)
node.left.left.left = Node(22)

print height(node)
```

#### Serialize and unserialize a tree (not finished)

```
class Node:
    def __init__(self, val):
        self.left = None
        self.right = None
        self.val = val
        self.str = []

    def traverse(self):
        list = []

        node = [self]

        while (len(node) > 0):
            val = node.pop()

            if val.left != None:
                node.push(val.left)

            if val.right != None:
                node.push(val.right)

            if val.left == None and val.right != None:
                self.str.push("-1")

            if val.left != None and val.right == None:
                self.str.push("-1")

            self.str.append(val.val)

     def decode(self, str):
         node = str[1]
         stack.append(node)
         for i in range(len(str), 1):
             if left(str, i) != -1:
                 appendLeft(stack.pop().left, left(str, i))
                 stack.append(str[i])
             if right(str, i) != -1:
                 appendRight(stack.pop().right,, node.right, right(str, i)
                 stack.append(str[i])

     def appendLeft(node, left):
         node.left = left

     def appendRight(node, right):
         node.right = right
```

### Traverse a tree (BFS/DFS)

```
BFS
class Node(object):
  def __init__(self, value, left=None, right=None):
    self.value = value
    self.left = left
    self.right = right

def traverse(rootnode):
  thislevel = [rootnode]
  while thislevel:
    nextlevel = list()
    for n in thislevel:
      print n.value,
      if n.left: nextlevel.append(n.left)
      if n.right: nextlevel.append(n.right)
    print
    thislevel = nextlevel

t = Node(1, Node(2, Node(4, Node(7))), Node(3, Node(5), Node(6)))

traverse(t)
```

```
def get_breadth_first_nodes(root):
    nodes = []
    stack = [root]
    while stack:
        cur_node = stack[0]
        stack = stack[1:]
        nodes.append(cur_node)
        for child in cur_node.get_children():
            stack.append(child)
    return nodes

def get_depth_first_nodes(root):
    nodes = []
    stack = [root]
    while stack:
        cur_node = stack[0]
        stack = stack[1:]
        nodes.append(cur_node)        
        for child in cur_node.get_rev_children():
            stack.insert(0, child)
    return nodes

########################################################################
class Node(object):
    def __init__(self, id_):
        self.id = id_
        self.children = []

    def __repr__(self):
        return "Node: [%s]" % self.id

    def add_child(self, node):
        self.children.append(node) 

    def get_children(self):
        return self.children         

    def get_rev_children(self):
        children = self.children[:]
        children.reverse()
        return children         

########################################################################
def println(text):
    sys.stdout.write(text + "\n")

def make_test_tree():
    a0 = Node("a0")
    b0 = Node("b0")      
    b1 = Node("b1")      
    b2 = Node("b2")      
    c0 = Node("c0")      
    c1 = Node("c1")  
    d0 = Node("d0")   

    a0.add_child(b0) 
    a0.add_child(b1) 
    a0.add_child(b2)

    b0.add_child(c0) 
    b0.add_child(c1) 

    c0.add_child(d0)

    return a0                  

def test_breadth_first_nodes():
    root = make_test_tree()
    node_list = get_breadth_first_nodes(root)
    for node in node_list:
        println(str(node))

def test_depth_first_nodes():
    root = make_test_tree()
    node_list = get_depth_first_nodes(root)
    for node in node_list:
        println(str(node))

########################################################################
if __name__ == "__main__":
    test_breadth_first_nodes()
    println("")
    test_depth_first_nodes()
```

### Topological Sort

```
def dfs(graph,start):
    path = []
    stack = [start]
    label = len(graph)
    result = {}
    while stack != []:
        for element in stack:
            if element not in result:
                result[element] = label
                label = label – 1
        v = stack.pop()
        if v not in path: path.append(v)
        for w in reversed(graph[v]):
            if w not in path and not w in stack:
                stack.append(w)
    result = {v:k for k, v in result.items()}
    return path, result
```

### Sets

#### Remove duplicates

```
seen = set()
uniq = []
for x in a:
    if x not in seen:
        uniq.append(x)
        seen.add(x)
```

#### Detect duplicates in a list

```
l = [1,2,3,4,4,5,5,6,1]
set([x for x in l if l.count(x) > 1])
```

### Recursion

#### Reverse a string

```
    def reverseString(string):    
        if len(string)==0:
            return string
        else:
            return string[-1:] + reverseString(string[:-1])
```

#### Length of a string

```
    def strlen(s):
      if s == '':
        return 0
      return 1 + strlen(s[1:])
```

#### Count a string

```
def count(s):
    if s == '':
        return 0
    else:
        return 1 + count(s[1::])
```

#### Permutations

```
def permut(array):
    if len(array) == 1:
        return [array]
    res = []
    for permutation in permut(array[1:]):
        for i in range(len(array)):
            res.append(permutation[:i] + array[0:1] + permutation[i:])
    return res
```

### **Python**

What is the deadlock

Multiprocess vs Multithread

Python 2.x vs 2.3

#### Write code to parse generic web logs and format it in different ways

Recursive call of LinkedIn api using REST.

### Java

Mvn

Devops

Parsing a log

REST Interface

#### Lifecycle of an object

As you work with objects in Java, understanding how objects are born, live their lives, and die is important. This topic is called the life cycle of an object, and it goes something like this:

1. Before an object can be created from a class, the class must be loaded. To do that, the Java runtime locates the class on disk (in a .class file) and reads it into memory. Then Java looks for any static initializers that initialize static fields — fields that don’t belong to any particular instance of the class, but rather belong to the class itself and are shared by all objects created from the class.

A class is loaded the first time you create an object from the class or the first time you access a static field or method of the class. For example, when you run the main method of a class, the class is initialized because the main method is static.

1. An object is created from a class when you use the new keyword. To initialize the class, Java allocates memory for the object and sets up a reference to the object so the Java runtime can keep track of it. Then, Java calls the class constructor, which is like a method but is called only once, when the object is created. The constructor is responsible for doing any processing required to initialize the object, such as initializing variables, opening files or databases, and so on.
2. The object lives its life, providing access to its public methods and fields to whoever wants and needs them.
3. When it’s time for the object to die, the object is removed from memory and Java drops its internal reference to it. You don’t have to destroy objects yourself. A special part of the Java runtime called the garbage collector takes care of destroying all objects when they are no longer in use.

### Big O Notation

#### Between red black trees, binary tree, linked list, hash table, btree, what is doesn't have to be nlogn

Btree (doesn't have to be balanced), Hash Table and LinkedList.

#### Array of 10000 having 16bit elements, find bits set (unlimited RAM)

Brute force: 10000 \* 16 \* 4 = 640,000 ops. (shift, compare, increment and iteration for each 16 bits word)

Faster way:

We can build table 00-FF -> number of bits set. 256 \* 8 \* 4 = 8096 ops

I.e. we build a table where for each byte we calculate a number of bits set.

Then for each 16-bit int we split it to upper and lower

```
for (n in array) {
byte lo = n & 0xFF; // lower 8-bits
byte hi = n >> 8; // higher 8-bits
// simply add number of bits in the upper and lower parts
// of each 16-bits number
// using the pre-calculated table
k += table[lo] + table[hi];
}
```

60000 ops in total in the iteration. I.e. 68096 ops in total. It's O(n) though, but with less constant (\~9 times less).

In other words, we calculate number of bits for every 8-bits number, and then split each 16-bits number into two 8-bits in order to count bits set using the pre-built table.

### How do you implement a Priority Queue

While priority queues are often implemented with [heaps](https://en.wikipedia.org/wiki/Heap\_\(data\_structure\)), they are conceptually distinct from heaps. A priority queue is an abstract concept like "a [list](https://en.wikipedia.org/wiki/List\_\(abstract\_data\_type\))" or "a [map](https://en.wikipedia.org/wiki/Associative\_array)"; just as a list can be implemented with a [linked list](https://en.wikipedia.org/wiki/Linked\_list) or an [array](https://en.wikipedia.org/wiki/Array\_data\_structure), a priority queue can be implemented with a heap or a variety of other methods such as an unordered array.

### Best and worst case in Quicksort

### When does the worst case of Quicksort occur?

The answer depends on strategy for choosing pivot. In early versions of Quick Sort where leftmost (or rightmost) element is chosen as pivot, the worst occurs in following cases.

1\) Array is already sorted in same order.\
2\) Array is already sorted in reverse order.\
3\) All elements are same (special case of case 1 and 2)

Implement merge sort (merging two lists)

Is it possible inplace?

Why people implement quicksort even heapsort is better in some cases?

### Quicksort

```
from random import randrange
​
def partition(list, start, end, pivot):
    list[pivot], list[end] = list[end], list[pivot]
    store_index = start

    for i in xrange(start, end):
        if list[i] < list[end]:
            list[i], list[store_index] = list[store_index], list[i]
            store_index += 1

    list[store_index], list[end] = list[end], list[store_index]

    return store_index
​
def quick_sort(list, start, end):
    if start >= end:
        return list

    pivot = randrange(start, end + 1)
    new_pivot = partition(list, start, end, pivot)
    quick_sort(list, start, new_pivot - 1)
    quick_sort(list, new_pivot + 1, end)
​
def sort(list):
    quick_sort(list, 0, len(list) - 1)
    return list
​
print sort([])
print sort([1,2,3,4])
print sort([2,3,4,1])
print sort([2,3,4,1, 5, -2])

W
[]                                                                       
2 3                                                                      
Store index 2                                                            
1 1                                                                      
Store index 1                                                            
[1, 2, 4, 3]                                                             
1 3                                                                      
Store index 2                                                            
1 1                                                                      
Store index 0                                                            
[2, 1, 4, 3]                                                             

>>>                                                                      

Alejandro Sanchez ran 36 lines of Python 2 (finished in 557ms):          

[]                                                                       
0 3                                                                      
Store index 0                                                            
1 3                                                                      
Store index 1                                                            
3 3                                                                      
Store index 3                                                            
[4, 4, 3, 4]                                                             
0 3                                                                      
Store index 1                                                            
2 3                                                                      
Store index 3                                                            
[1, 3, 3, 4]                                                             

>>>                                                                      

Alejandro Sanchez ran 34 lines of Python 2 (finished in 564ms):          

[]                                                                       
[2, 2, 4, 4]                                                             
[1, 2, 4, 4]                                                             

>>>                                                                      

Alejandro Sanchez ran 35 lines of Python 2 (finished in 594ms):          

[]                                                                       
[2, 2, 4, 4]                                                             
[2, 2, 4, 4]                                                             
[1, 1, 1, 5, 5, 7]                                                       

>>>                                                                      

Alejandro Sanchez ran 34 lines of Python 2 (finished in 720ms):          

[]                                                                       
[1, 2, 3, 4]                                                             
[1, 2, 3, 4]                                                             

>>>                                                                      

Alejandro Sanchez ran 35 lines of Python 2 (finished in 876ms):          

[]                                                                       
[1, 2, 3, 4]                                                             
[1, 2, 3, 4]                                                             
[-2, 1, 2, 3, 4, 5]                                                      

>>>
```

## Calculate the throughput in (b/s) for a particular ip (e.g. 10.4.3.34)

START\_TIME,END\_TIME,FILE\_SIZE,IP\_ADDRESS

0, 30, 3874, 10.4.3.34

0, 30, 324235, 10.4.3.32

0, 30, 324235, 10.4.3.38

10, 30, 324235, 10.4.3.34

10, 30, 324235, 10.4.3.34

10, 30, 324, 10.4.3.34

10, 30, 324235, 10.4.3.34

Log is 50K lines long

```
START_TIME,END_TIME,FILE_SIZE,IP_ADDRESS

    0, 30, 3874, 10.4.3.34
    0, 30, 324235, 10.4.3.32
    0, 30, 324235, 10.4.3.38
    10, 30, 324235, 10.4.3.34
    10, 30, 324235, 10.4.3.34
    10, 30, 324, 10.4.3.34
    10, 30, 324235, 10.4.3.34

Log is 50K lines long

throughput = {} 

with open ('times.log') as f:
  for line in f:
    data = line.split(",")
    initial = data[0]
    end = data[1]
    size = data[2]
    ip = data[3]

    bpers = size / (end - initial)

    if throughput.has_key(ip):
        throughput[ip] += bpers
    else
        throughput[ip] = bpers

for k,v in throughput.items():
    print k,v
```

### What level is?

```
def whatLevelIs(x, y):
    count = 1

    while (x != 1 and y != 1):
        if x < y:
            y = x - y
            count += 1

        if x > y:
            x = x - y
            count += 1

    return count

#
# Your previous Go content is preserved below:
#
# // This is not a binary tree problem but it looks like one.
# //
# // Imagine an infinite binary tree where:
# // - the root node is in the form of a tuple (1,1)
# // - the left leaf of a (x, y) node is (x, x+y)
# // - the right leaf of a (x, y) node is (x + y, y)
# // Example
# //               (1,1)
# //            /         \
# //      (1,2)              (2,1)
# //     /     \             /.  \
# //  (1,3).  (3,2)      (2,3).  (3,1)
# //          /.  \
# //       (3,5). (5,2)
# // Find the depth/level of a certain (a,b) node
# // If (a,b) = (1,1), the depth is 1
# // If (a,b) = (3,2), the depth is 3
# // If (a,b) = (3,5), the depth is 4
#
# package main
#
# import "fmt"
#
# func whatLevelIsThisOn(a, b int) int {
#   level := 99
#   // add logic here
#   return level
# }
#
# func main() {
#   a := 3
#   b := 5
#   fmt.Println(whatLevelIsThisOn(a, b))
# }
```
