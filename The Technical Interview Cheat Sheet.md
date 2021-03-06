## Helpful Links
- [technical interviews](https://www.cs.cmu.edu/~07131/f19/topics/extratations/interviews/interviews.pdf)

## Data Structure Basics
### Abstract Data Types v.s. implementations
- list: arraylist, linkedlist
- set: hashset, treeset
- map: hashmap, treemap
- stack: linkedlist, list
- queue: linkedlist, list
- priority queue: heap


### **Array**
#### Definition:
- Stores data elements based on an sequential, most commonly 0 based, index.
- Based on [tuples](http://en.wikipedia.org/wiki/Tuple) from set theory.
- They are one of the oldest, most commonly used data structures.

#### What you need to know:
- Optimal for indexing; bad at searching, inserting, and deleting (except at the end).
- **Linear arrays**, or one dimensional arrays, are the most basic.
  - Are static in size, meaning that they are declared with a fixed size.
- **Dynamic arrays** are like one dimensional arrays, but have reserved space for additional elements.
  - If a dynamic array is full, it copies its contents to a larger array.
- **Multi dimensional arrays** nested arrays that allow for multiple dimensions such as an array of arrays providing a 2 dimensional spacial representation via x, y coordinates.

#### Time Complexity:
- Indexing:         Linear array: O(1),      Dynamic array: O(1)
- Search:           Linear array: O(n),      Dynamic array: O(n)
- Optimized Search: Linear array: O(log n), Dynamic array: O(log n)
- Insertion:        Linear array: n/a        Dynamic array: O(n) 
- Popping: O(1) to pop the last element of a Python list, and O(N) to pop an arbitrary element (since the whole rest of the list has to be shifted)


### **Linked List**
#### Definition:
- Stores data with **nodes** that point to other nodes.
  - Nodes, at its most basic it has one datum and one reference (another node).
  - A linked list _chains_ nodes together by pointing one node's reference towards another node.
- header: contain size and first node of the LinkedList

#### What you need to know:
- Designed to optimize insertion and deletion, slow at indexing and searching.
- **Doubly linked list** has nodes that also reference the previous node.
- **Circularly linked list** is simple linked list whose **tail**, the last node, references the **head**, the first node.
- **Stack**, commonly implemented with linked lists but can be made from arrays too.
  - Stacks are **last in, first out** (LIFO) data structures.
  - Made with a linked list by having the head be the only place for insertion and removal.
- **Queues**, too can be implemented with a linked list or an array.
  - Queues are a **first in, first out** (FIFO) data structure.
  - Made with a doubly linked list that only removes from head and adds to tail.

#### Time Complexity:
- Indexing:         Linked Lists: O(n)
- Search:           Linked Lists: O(n)
- Optimized Search: Linked Lists: O(n)
- Insertion:        Linked Lists: O(1)

### ***Heap***
#### Definition:
- completeness: Every level (except last) completely filled. Nodes on bottom level are as far left as possible.
- heap order: maxHeap: every element <= its parent, but bigger element can be deeper in the tree

#### What you need to know:
- used to implement priority queue
- ``add(e)``
	- put the element at the leftmost empty node
	- bubble up while greater than its parent
- ``poll(e)``
	- save root in a local variable
	- assign last value to root, delete last node
	- bubble down: while less than a child, swap with the greater child
- children of node k: 2k+1, 2k+2

#### Time Complexity: 
- add: O(log n)
- poll: O(log n)
- peek: O(1)
- contains: O(n)
- remove: O(n)

### ***Graph***
- two vertices are adjacent if connected by one edge
- common graph representation: 
	- adjacency list: e.g. map from vertice to its neighbors; linkedlist, where each node contains vertex label and linkedlist of neighbors; array where each element is a linkedlist
	- adjacency matrix: arr[i][j] == 1 iff there is an edge from i to j
- space: 
	- adjacency list: O(|V| + |E|)
	- adjacency matrix: O(|V|^2)
- time to visit all edges:
	- adjacency list: O(|V| + |E|)
	- adjacency matrix: O(|V|^2)
- time to determine whether an edge from v1 to v2 exists:
	- adjacency list: O(|V| + outdegree of v1)
	- adjacency matrix: O(1)
- in a directed graph: 
	- outdegree of u: #edges where u is the source
	- indegree of u: #edges where u is the sink
- in an undirected graph: 
	- degree of u: #edges where u is an endpoint
- sparse: |E| << |V|^2, adjacency list better
- dense: |E| ~ |V|^2, adjacency matrix better
- topological order: and ordering of verticies as v1, v2, ..., vn, such that for every edge (vi, vj), it holds that i < j.
	- directed graph can be topologically ordered iff no cycle
	- acyclic: no cycle
- planar: can be drawn on the plane w/o any edge crossin
	- every planer graph is 4-colorable

### **Tree**
- an undirected graph if there is exactly one simple path between every vertex
- |E| = |V| - 1
- connected
- no cycles
- **spanning tree** of a connected graph (V, E) is a subgraph that is a tree
	- same set of vertices
	- maximal set of edges that contains no cycles / minimal set of edges that connect all vertices
	- ```
		find spannning tree (subtractive): 
			start with the whole graph
			while there is still a cycle:
				pick an edge from it and delete, graph remains connected
		```
	- but will throw out too many edges if graph dense
	- ```
		find spannning tree (additive): 
			while graph not connected:
				choose an edge that connects two components, and add it
				graph still has no cycle
		```

- check if a graph has cycle, similar to dfs:
	```
	def has_cycle(v):
		s = [v]
		visited = {v}
		while s:
			curr = s.pop()
			if curr in visited:
				return True
			for elem in v.neighbors:
				s.append(elem)
		return False
	```
- **minimum spanning tree (MST)**
	- suppose edge has weight > 0
	- sum of weights is minimum
- find MST
	- Kruskal's algorithm:
		```
		start with all nodes and no edges
		each step, add an edge that does not form a cycle with minimum weight 
		```
	- Prim's algorithm
		```
		start with one node
		each step, add an edge connected to the starting node with minimum weight
		the constructed tree always connected
		```
	- if edge weights all different, the Kruskal and Prim find the same MST


### **Hash Table or Hash Map**
#### Definition:
- Stores data with key value pairs.
- **Hash functions** accept a key and return an output unique only to that specific key / given a value **hashcode** to be put in the table, returns an index of where to put it
	- This is known as **hashing**, which is the concept that an input and an output have a one-to-one correspondence to map information.
  	- Hash functions return a unique address in memory for that data.
	- knows nothing about the table size, thus need to mod table_size
	- perfect hash function: map each input to a different index in the table
		- impossible
		- don't know size(table)
		- #possible values >> table size


#### What you need to know:
- Designed to optimize searching, insertion, and deletion.
- **Hash collisions** are when a hash function returns the same output for two distinct inputs.
	- multiple inputs are hashed to the same bucket
  	- All hash functions have this problem.
  	- This is often accommodated for by having the hash tables be very large.
	- solution: 
		- **chaining**: each bucket contains a linkedlist of items hashed to it
			- uses more memory
			- worst case: O(n), all elements hashed to one bucket
		- **open addressing**: each buckect contains one element, look in successive array element to find a place for new item
			- when removing an element, mark it as NP (not present), so when searching for an element, could keep looking
			- stop searching until it finds a null or the element you're searching for
			- clustering: nearby hashes have similar probe sequences, so more collision
			- linear probing: i, i+1, i+2, i+3, ..., 
				- problem: clustering
				- Deletion will be a problem. If one key involves a chain of several probes, it will be lost if somewhere along the chain, one of the other keys is removed, leaving an empty slot. Thus, you can't find the value stored after probing.
			- quadratic probing: i, i+1, i+4, i+9, ..., requires the len(table) to be a prime so have access to every bucket
- another problem: not all buckets get to used
- Hashes are important for associative arrays and database indexing.
- java.lang.Objects hashCode()
	- returns memory address of the object by default
	- if override equals, must override hashCode()
	- a.equals(b) returns true iff a and b are the same object
	- if equal, then should have the same hashcode and hashed to the same bucket
- **load factor** \lambda = # of entries / lenth of array
	- if \lambda = 1/2, expected # of probes = 2
	- if \lambda > 1/2, no longer expected constant operation
- expected time of add, contains, remove:
	- chaining
		- worst case: O(n), all elements hashed to the first buckect, and clustering at the front
		- if load factor small: O(1), most cases search length = 0, one case search length = n, 
		```
			((n-1)*0 + 1 * n) / len(table) = n/len(table) = load factor
		````
		- average chain length = load factor
	- linear probing
		- num of probes = 1/(1-\lambda)
- need to keep the size in a good range, not too many collision, not too large wasted memory
- **resizing**
	- when load factor too big, create a new array twice the size
	- move values into the new array
	- ArrayList does the same
	- dynanmic resizing: double the array, rehash all elements
	- amortize the cost of resizing over the time for adding elements
- HashMap in Java
	- computes key.hashCode(), so no duplicate key
	- default load factor = 0.75
	-  any class can serve as a key if and only if it overrides the equals() and hashCode() method
	- The bucket is a linkedlist but not java.util.Linkedlist. HashMap has its own implementation of the linkedlist. Therefore, it traverses through linkedlist and compares keys in each entry using keys.equals() until equals() returns true. Then, the value object is returned.
#### Time Complexity:
- Indexing: O(1) amortized
- Search: O(1), worst: O(n)
- Insertion: O(1), worst: O(n)


### **Binary Tree**
#### Definition:
- Is a tree like data structure where every node has at most two children.
  - There is one left and right child node.

#### What you need to know:
- depth: the length of the path to the root
- height: length of the longest path from the root to a leaf
- perfect tree: #node = 2^{h+1} - 1
- complete binary tree: Every level, except last, is completely filled, nodes on bottom level as far left as possible. No holes.
- Designed to optimize searching and sorting.
- A **degenerate tree** is an unbalanced tree, which if entirely one-sided is a essentially a linked list.
- Used to make **binary search trees**.
	- A binary tree that uses comparable keys to assign which direction a child is.
	- all nodes in the left tree are smaller
	- all nodes in the right tree are greater
	- There can be no duplicate node.
	- Because of the above it is more likely to be used as a data structure than a binary tree.
- **balanced BST**
  	- subtrees of any node are about the same height
- preorder traversal: root, left, right
- inorder: left, root, right
- postorder: left, right, root

#### Time Complexity:
- Indexing:  Binary Search Tree: O(log n)
- Search:    Binary Search Tree: O(log n)
- Insertion: Binary Search Tree: O(log n)





## Search Basics
### **Breadth First Search**
#### Definition:
- An algorithm that searches a tree (or graph) by searching levels of the tree first, starting at the root.
	- It finds every node on the same level, most often moving left to right.
	- While doing this it tracks the children nodes of the nodes on the current level.
	- When finished examining a level it moves to the left most node on the next level.
	- The bottom-right most node is evaluated last (the node that is deepest and is farthest right of it's level).
```
def bfs(v):
	q = [v]
	visited = {v}
	while q:
		curr = q.pop(0)
		if curr not in visited:
			visited.add(curr)
			for elem in curr.neighbors:
				q.append(elem)
```

#### What you need to know:
- Optimal for searching a tree that is wider than it is deep.
- Uses a queue to store information about the tree while it traverses a tree.
	- Because it uses a queue it is more memory intensive than **depth first search**.
	- The queue uses more memory because it needs to stores pointers

#### Time Complexity:
- Search: Breadth First Search: O(|V| + |E|)


### **Depth First Search**
#### Definition:
- An algorithm that searches a tree (or graph) by searching depth of the tree first, starting at the root.
	- It traverses left down a tree until it cannot go further.
	- Once it reaches the end of a branch it traverses back up trying the right child of nodes on that branch, and if possible left from the right children.
	- When finished examining a branch it moves to the node right of the root then tries to go left on all it's children until it reaches the bottom.
	- The right most node is evaluated last (the node that is right of all it's ancestors).
```
def dfs(v):
	s = [v]
	visited = {v}
	while s:
		curr = s.pop()
		if curr not in visited:
			visited.add(curr)
			for elem in curr.neighbors:
				s.append(elem)
```

#### What you need to know:
- Optimal for searching a tree that is deeper than it is wide.
- Uses a stack to push nodes onto.
	- Because a stack is LIFO it does not need to keep track of the nodes pointers and is therefore less memory intensive than breadth first search.
	- Once it cannot go further left it begins evaluating the stack.

#### Time Complexity:
- Search: Depth First Search: O(|E| + |V|)

#### Breadth First Search Vs. Depth First Search
- The simple answer to this question is that it depends on the size and shape of the tree.
  - For wide, shallow trees use Breadth First Search
  - For deep, narrow trees use Depth First Search
- space: O(|V|)
- time: 
	- adjacency list: O(|V| + |E|)
	- adjacency matrix: O(|V|^2)

#### Nuances:
- Because BFS uses queues to store information about the nodes and its children, it could use more memory than is available on your computer. (But you probably won't have to worry about this.)
- If using a DFS on a tree that is very deep you might go unnecessarily deep in the search. See [xkcd](http://xkcd.com/761/) for more information.
- Breadth First Search tends to be a looping algorithm.
- Depth First Search tends to be a recursive algorithm.


## Efficient Sorting Basics
### **Insertion Sort**
- In each iteration, push arr[i] to its sorted position in arr[0..i], swap arr[i] with arr[i-1] if arr[i] < arr[i-1], then increase i
- stable: two equal values stay in the same relative position
```
def insertionSort(arr):
	for i in range(len(arr)):
		k = i
		while k > 0 and arr[k-1] > arr[k]:
			swap(arr, k-1, k)
			k -= 1
```

#### Time Complexity:
- Worst case: O(n^2), reverse-sorted input
- Best case: O(n), sorted input
- Average case: O(n^2)
- Space: O(1)

### **Selection Sort**
- Each iteration, swap min value of the latter section with arr[i]
- not stable
```
def selectionSort(arr):
	for i in range(len(arr)):
		m = index of min(arr[i:])
		swap(arr, i, m)
```

#### Time Complexity:
- Worst case: O(n^2)
- Best case: O(n^2)
- Average case: O(n^2)
- Space: O(1)

### **Merge Sort**
#### Definition:
- A comparison based sorting algorithm
  - Divides entire dataset into groups of at most two.
  - Compares each number one at a time, moving the smallest number to left of the pair.
  - Once all pairs sorted it then compares left most elements of the two leftmost pairs creating a sorted group of four with the smallest numbers on the left and the largest ones on the right.
  - This process is repeated until there is only one set.
```
def mergeSort(arr, h, t):
    if len(arr) < 2:
        return
	mid = (h + t) // 2
	mergeSort(arr, h, mid)
	mergeSort(arr, mid+1, t)
	merge(arr, h, mid, t)

def merge(arr, h, mid, t):
	left = arr[h:mid+1]
	right = arr[mid+1:t]
	i = j = 0
	k = h
	if left[i] < right[j]:
		arr[k] = left[i]
		i += 1
	else:
		arr[k] = right[j]
		j += 1
	k += 1
```
#### What you need to know:
- This is one of the most basic sorting algorithms.
- Know that it divides all the data into as small possible sets then compares them.

#### Time Complexity:
- Best Case Sort: O(n)
	```
	if (arr[mid] > arr[mid + 1]) 
		merge(arr, low, mid, high); 
	```
- Average Case Sort: O(n log n)
- Worst Case Sort: O(n log n)
- Space: O(n)

### **Quicksort**
#### Definition:
- A comparison based sorting algorithm
  - Divides entire dataset in half by selecting the middle element and putting all smaller elements to the left of the element and larger ones to the right.
  - It repeats this process on the left side until it is comparing only two elements at which point the left side is sorted.
  - When the left side is finished sorting it performs the same operation on the right side.
- Computer architecture favors the quicksort process.
- good pivot value: median(arr[0], arr[-1], arr[n//2])
```
def quickSort(arr, h, t):
    if len(arr) < 2:
    	return;
    mid = partition(arr, h, t)
    quickSort(arr, h, mid-1)
    quickSort(arr, mid+1, t)
  
def partition(arr, h, t):
	pivot = h
	later = t
	while pivot < later:
		if arr[pivot+1] < arr[pivot]:
			swap(arr, pivot+1, pivot)
			pivot += 1
		else:
			swap(arr, pivot+1, later)
			later -= 1
```

#### What you need to know:
- While it has the same Big O as (or worse in some cases) many other sorting algorithms it is often faster in practice than many other sorting algorithms, such as merge sort.
- Know that it halves the data set by the average continuously until all the information is sorted.

#### Time Complexity:
- Best Case Sort: O(n log n), pivot always middle value, max depth O(log n)
- Average Case Sort: O(n log n)
- Worst Case Sort:  O(n^2), pivot always min/max value
- Space: O(log n)

### **Bubble Sort**
#### Definition:
- A comparison based sorting algorithm
	- It iterates left to right comparing every couplet, moving the smaller element to the left.
	- It repeats this process until it no longer moves an element to the left.

#### What you need to know:
- While it is very simple to implement, it is the least efficient of these three sorting methods.
- Know that it moves one space to the right comparing two elements at a time and moving the smaller on to left.

#### Time Complexity:
- Best Case Sort: Merge Sort: O(n)
- Average Case Sort: Merge Sort: O(n^2)
- Worst Case Sort: Merge Sort: O(n^2)

#### Merge Sort Vs. Quicksort
- Quicksort is likely faster in practice.
- Merge Sort divides the set into the smallest possible groups immediately then reconstructs the incrementally as it sorts the groupings.
- Quicksort continually divides the set by the average, until the set is recursively sorted.
- Java.util.Arrays has a method sort(array)
	- primitives: quickSort
	- objects implementing Comparable: timSort (modified mergeSort)
- Merge Sort requires extra space, and Quick Sort is in place
- They both deploy the idea of divide-and-conquer. In merge sort, the divide step does hardly anything, and all the real work happens in the combine step. Quick Sort is the opposite: all the real work happens in the divide step. In fact, the combine step in Quick Sort does absolutely nothing.
- quicksort unstable, mergesort stable

### **Heap Sort**
- make the array into a max heap
- pull elements at the top, put it at the end of the array

### **Topological Sort**
- delete a vertex with indegree 0 will not remove any cycle
```
def topological_sort():
	k = 0
	while there is a node of indegree 0:
		label it as k
		delete it and all edges leaving it
		k += 1
```
### **Graph Coloring**
```
def color():
	for each vertex v in graph:
		c = find_color(v.neighbors)
		color v with c

def find_color(vs):
	used = [0] * (vs.length + 1)
	for v in vs:
		if v.color < len(used):
			used[v.color] += 1
	return smallest c such that used[c] == 0
```


## Basic Types of Algorithms
### **Recursive Algorithms**
#### Definition:
- An algorithm that calls itself in its definition.
	- **Recursive case** a conditional statement that is used to trigger the recursion.
	- **Base case** a conditional statement that is used to break the recursion.

#### What you need to know:
- **Stack level too deep** and **stack overflow**.
	- If you've seen either of these from a recursive algorithm, you messed up.
	- It means that your base case was never triggered because it was faulty or the problem was so massive you ran out of alloted memory.
	- Knowing whether or not you will reach a base case is integral to correctly using recursion.
	- Often used in Depth First Search


### **Iterative Algorithms**
#### Definition:
- An algorithm that is called repeatedly but for a finite number of times, each time being a single iteration.
  	- Often used to move incrementally through a data set.

#### What you need to know:
- Generally you will see iteration as loops, for, while, and until statements.
- Think of iteration as moving one at a time through a set.
- Often used to move through an array.

#### Recursion Vs. Iteration
- The differences between recursion and iteration can be confusing to distinguish since both can be used to implement the other. But know that,
	- Recursion is, usually, more expressive and easier to implement.
	- Iteration uses less memory.
- **Functional languages** tend to use recursion. (i.e. Haskell)
- **Imperative languages** tend to use iteration. (i.e. Ruby)
- Check out this [Stack Overflow post](http://stackoverflow.com/questions/19794739/what-is-the-difference-between-iteration-and-recursion) for more info.

#### Pseudo Code of Moving Through an Array (this is why iteration is used for this)
```
Recursion                         | Iteration
----------------------------------|----------------------------------
recursive method (array, n)       | iterative method (array)
  if array[n] is not nil          |   for n from 0 to size of array
    print array[n]                |     print(array[n])
    recursive method(array, n+1)  |
  else                            |
    exit loop                     |
```
### **Greedy Algorithm**
#### Definition:
- An algorithm that, while executing, selects only the information that meets a certain criteria.
- The general five components, taken from [Wikipedia](http://en.wikipedia.org/wiki/Greedy_algorithm#Specifics):
	- A candidate set, from which a solution is created.
	- A selection function, which chooses the best candidate to be added to the solution.
	- A feasibility function, that is used to determine if a candidate can be used to contribute to a solution.
	- An objective function, which assigns a value to a solution, or a partial solution.
	- A solution function, which will indicate when we have discovered a complete solution.

#### What you need to know:
- Used to find the expedient, though non-optimal, solution for a given problem.
- Generally used on sets of data where only a small proportion of the information evaluated meets the desired result.
- Often a greedy algorithm can help reduce the Big O of an algorithm.

#### Pseudo Code of a Greedy Algorithm to Find Largest Difference of any Two Numbers in an Array.
```
greedy algorithm (array)
	var largest difference = 0
	var new difference = find next difference (array[n], array[n+1])
	largest difference = new difference if new difference is > largest difference
	repeat above two steps until all differences have been found
	return largest difference
```

This algorithm never needed to compare all the differences to one another, saving it an entire iteration.

## Data structures in Python
- ``[]`` denotes optional arguments
- ``...`` means there are multiple such elements

### ** String**

### **List**
#### Methods
- ``list.insert(i, x)``insert `x` at `i`
- ``list.remove(x)`` remove the first occurrence of x, raise `ValueError` if not found
- ``list.pop([i])`` remove elem at `i`, remove the last item if argument unspecified
- ``list.index(x[, start[, end]])`` return index of the first occurrence of `x`, raise `ValueError` if not found
- ``list.count(x)`` returns # of x in the list
- ``list.sort(key = None, reverse = False)`` sort in place
- ``a = sorted(list, key=None, reverse=False)``
- ``del``
	```
	del arr[0]
	del arr[2:4]
	```

#### Miscellaneous
- could use list as stack or queue
- use it as queue not efficient, use `collections.deque` instead.
- list comprehensions
	- ``l = [(i, j) for i in x for j in y if i == j]``
- comparing sequences and other types: uses lexicographical ordering
- if one sequence is an initial sub-sequence of another, the shorter one is smaller
	```
	[1,2,3] < [1,2,4]
	[1,2,3,4] < [1,2,4]
	[1,2] < [1,2,3]
	```


### **Tuple & Sequences**
- tuples are immutable
- tuple may contain 1 or 0 items
	```
	empty_tuple = ()
	singleton = ('hi', )
	```
- tuple unpacking: ``x, y, z = ('hi', 'hello', 'hey')``


### **Set**
```
d = {} #this creates empty dict
s = set() 

# put letters in a string to a set
letters = set('arabica')

# elems in a not in b
a - b

# elems in a or b
a | b

# elems in both a and b
a & b
a.intersection(b)

# elems in a or b but not in both
a ^ b

# set comprehension
a = {x for x in set1 if x not in set2}

# looping
s = {1,2,3,4}
for elem in sorted(s):
	...
```


### **Dictionary**
- keys has to be immutable
```
d = {'qwE': 20, 'kv': 22}

# could use the del keyword to delete key:value pair
del d['qwE']

# returns list of all keys
list(d)

# sort keys in ascending order
sorted(d)

# use 'in' to check if contains a key
's' in d

# constructs dictionary from sequences of key-value pairs
# could put list of tuples as argument
dict([('qwe', 20), ('kv', 22)])

# dict comprehension
{x: x**2 for x in range(4)}

# looping
for k, v in d.items():
	...

for elem in d.keys()/d.values():

# print all keys
''.join(d.keys())
```


### **Collections**
- starts with ``from collections import xxx``

#### deque
```
q = deque([1,2,3,4])
q.append(5)
q.popleft()
```

#### Counter
- support convenient tallies
- different fron dict: returns a zero count for missing items, instead of raising a `KeyError`
- **methods**
	- ``collections.Counter([iterable/mapping])``
		```
		c = Counter('hellohiheyyy')
		c = Counter(lst)
		c = Counter(dict)

		#remove elem entirely from counter
		del c['qwE']
		# does not remove, counter entry still has a 0 count
		c['qwE'] = 0
		```
	- ``elements()`` return an iterator over elements repeating as many times as its count
		````
		sorted(c.elements())
		#[1,1,1,2,2,3]
		````
	- ``most_common([n])`` return list of n most common elements and their counts; if n omitted then returns all
	- ``subtract([iterable/mapping])``
		```
		# c, d are two counters
		c.subtract(d)
		```
- examples
	```
	# sum of all counts
	sum(c.values())

	# list unique elements
	list(c)

	# convert to set/dict
	set(c)
	dict(c)

	# k least common elements
	c.most_common()[::-1][k]

	# mathematical operations are provided for combining Counter objects
	c = Counter(a=3, b=1)
	d = Counter(a=1, b=2)

	c + d # Counter({'a':4, 'b':3})

	# keep only positive counts
	c - d # Counter({'a':2})

	# intersection / take min over all elems
	c & d # Counter({'a':1, 'b':1})

	# union / max over all elems
	c | d # Counter({'a':3, 'b':2})
	```

#### defaultDict
- if a key encountered for the first time, an entry automatically created
```
d = defaultdict(list)
for k, v in pairs:
	d[k].append(v)
```

#### OrderedDict
- preserves the order in which the keys are inserted
- but built-in dict class remeber insertion order now (from Python 3.7)
- could used in LRU cache
- if value of a certain key changed, position of that key remains unchanged
- deleting and re-inserting push the key to the end
- ``popitem(last=True)`` returns and removes a key:value pair, popped in LIFO order if last=True, FIFO otherwise
- ``move_to_end(key, last=True)`` move an existing key to either end of the dict, moved to the right end if last=True, raise `KeyError` if key not in dict
- LRU cache
	```
	from collections import OrderedDict 

	class LRUCache: 
		# initialising capacity 
		def __init__(self, capacity: int): 
			self.cache = OrderedDict() 
			self.capacity = capacity 
	
		# we return the value of the key 
		# that is queried in O(1) and return -1 if we 
		# don't find the key in out dict / cache. 
		# And also move the key to the end 
		# to show that it was recently used. 
		def get(self, key: int) -> int: 
			if key not in self.cache: 
				return -1
			else: 
				self.cache.move_to_end(key) 
				return self.cache[key] 
	
		# first, we add / update the key by conventional methods. 
		# And also move the key to the end to show that it was recently used. 
		# But here we will also check whether the length of our 
		# ordered dictionary has exceeded our capacity, 
		# If so we remove the first key (least recently used) 
		def put(self, key: int, value: int) -> None: 
			self.cache[key] = value 
			self.cache.move_to_end(key) 
			if len(self.cache) > self.capacity: 
				self.cache.popitem(last = False) 
	
	```

### **itertools**
#### combinations
- ``combinations(iterable, r)`` return r length subsequences of elements from iterable
- inputs treated as unique based on their position, not value
- returns \frac{n!}{r!(n-r)!} items
- 
	```
	letters = 'abcdef'
	comb = combinations(letters, 3)
	#select 3 from letters
	y = [''.join(c) for c in comb]
	```
- ``combinations_with_replacement(iterable, r)`` allow individual elements to be repeated more than once
- returns \frac{(n+r-1)!}{r!(n-1)!} items
	```
	list(combinations_with_replacement(range(1, 5), 2))
	# [(1, 1), (1, 2), (1, 3), (1, 4), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (4, 4)]
	```
#### permutations
- ``permutations(iterable[, r])`` return successive r length permutations of elements in the iterable
-
	```
	permutations([1,2,3])
	# (1, 2, 3), (1, 3, 2), (2, 1, 3), (2, 3, 1), (3, 1, 2), (3, 2, 1)

	permutations([1,2,3], 2)
	# (1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)
	```

#### product
- ``product(*iterables[, r])`` return the cartesian product of input iterables
- `product(A, B)` is the same as `[(x, y) for x in A for y in B]`, but not ``for x, y in zip(A, B)``
- `*iterables` could be a list of lists, need to add `*` prior to the variable name
- 
	```
	arr1 = [1,2,3]
	arr2 = [5,6,7]
	product(arr1, arr2)
	# [(1, 5), (1, 6), (1, 7), (2, 5), (2, 6), (2, 7), (3, 5), (3, 6), (3, 7)]
	```


#### groupby
- ``groupby(iterable[, key])`` returns consecutive keys and groups from the iterable, key is a function that calculates keys for each elem in the iterable
- 
	```
	L = [("a", 1), ("a", 2), ("b", 3), ("b", 4)] 
	key_func = lambda x: x[0] 
	
	for key, group in groupby(L, key_func): 
		print(key + " :", list(group)) 

	# a : [('a', 1), ('a', 2)]
	# b : [('b', 3), ('b', 4)]

	lst = [1,1,2,2,2,3,3,1]
	groupby(lst)
	#[(1, 2), (2, 3), (3, 2), (1, 1)]
	```

### **heapq (minheap)**
- every parent node has a value less than or equal to any of its children
-  `import heap`
- `heap[k] <= heap[2*k+1] and heap[k] <= heap[2*k+2]`
- methods
	- `heapq.heappush(heap, item)`
	- `heappop(heap)` pop and return the smallest item from the heap. Raise `IndexError` if heap empty
	- `heap[0]` access the smallest element without popping it
	- `heapify(x)` transform list x into a heap, **in-place**, O(n)
	- `nlargest(n, iterable, key=None)` return a list with n largest elements from the iterable
	- `nsmallest(n, iterable, key=None)` return smallest n elements

- could define ``__lt__`` method to compare objects
	```
	def __lt__(self, other):
		return self.intAttribute < other.intAttribute
	```
- or use tuples and heapq sort by the first and subsequent elements
	
- Examples
	```
	h = []
	heappush(h, 5)
	heappush(h, 4)
	a = heappop(h)
	```

### **built-in functions**
- ``abs()``
- ``all(iterable)`` returns True if all elements in the iterable are true, or iterable empty
- ``any(iterable)`` returns True if any elem in the iterable true
- ``ascii(object)`` returns a string containing a printable representation fo an object, escape non-ASCII characters
- ``enumerate(iterable, start = 0)`` returns a list of tuples of index paired with each item in the iterable
- ``eval(expression)``
	```
	x = 1
	eval('x+1')
	eval('1+2+3')
	```
- ``isinstance(object, classinfo)`` return True if the object is an instance of the classinfo
- ``map(function, iterable, ...)`` return an iterator that applies function to every item of iterable
	```
	nums = [1,2,3,4]
	doubled_nums = list(map(lambda x: x + x, nums))
	```
- ``pow(base, exp[, mod])`` return base^pow; if mod present, return base^power%mod
- ``round(number[, ndigits])`` return number rounded to ndigits after the decimal point; if ndigits omitted or is None, return the nearest integer
- ``set(iterable)``
- ``sorted(iterable, *, key = None, reverse = False)``
- ``str(object)``
- ``var([object])`` return the `__dict__` attribute of object
	```
	class Fool:
		def __init__(self, name='qwE', age='20'):
			self.name = name
			self.age = age
	me = Fool()
	print(vars(me)) #{'name': 'qwE', 'age': 20}
	```
- ``zip(iterables)`` make an iterator that aggregates elements from each of the iterables
	```
	zip('abcd', 'xy') #ax, by
	```
- ``object.__dict__``
- ``instance.__class__`` the class of that instance
- ``definition.__name__`` the name of the class, function, method, ...


## common algs
### **Trie node**
- 
	```
	class Trie:
		def __init__(self, val=None):
			self.val = val
			self.char_map = {}
			
		def insert(self, word, move):
			if len(word) == 0:
				self.char_map['<END>'] = Trie(move)
			else:
				if word[0] not in self.char_map:
					self.char_map[word[0]] = Trie()
				self.char_map[word[0]].insert(word[1:], move)
				
		def is_str(self, node):
			if '<END>' in node.char_map:
				return node.char_map['<END>'].val
			return None
		
		def traverse(self, node, char):
			if char in node.char_map:
				return node.char_map[char]
			return None
	```
	```
	class TrieNode():
		def __init__(self):
			self.children = collections.defaultdict(TrieNode)
			self.is_word = False

	class WordDictionary:
		def __init__(self):
			"""
			Initialize your data structure here.
			"""
			self.root = TrieNode()        
			

		def addWord(self, word: str) -> None:
			"""
			Adds a word into the data structure.
			"""
			curr = self.root
			for w in word:
				curr = curr.children[w]   
			curr.is_word = True
	

		def search(self, word: str) -> bool:
			"""
			Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
			""" 
			self.res = False
			self.dfs(self.root, word)
			return self.res

		def dfs(self, node, word):
			if not word:
				if node.is_word:
					self.res = True
				return
			if word[0] == '.':
				for n in node.children.values():
					self.dfs(n, word[1:])
			else:
				if word[0] not in node.children:
					return
				self.dfs(node.children[word[0]], word[1:])
        
	```
- Insert and search costs O(key_length)
- the memory requirements of Trie is O(ALPHABET_SIZE * key_length * N), where N is number of keys in Trie
- backtracking algorithm, find all paths from one node to another
```
def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        res = []
        n = len(graph)
        def backtrack(node, path):
            if node == n-1:
                res.append(path)
            
            for nxt in graph[node]:
                if nxt not in path:
                    backtrack(nxt, path + [nxt])
        backtrack(0, [0])
        return res
```


### **functools**


### **math**
### **Other tricks**
- conditions can be chained: ``a < b == c``
- String.isdigit()
- initialize 2d array: dp = [[0 for i in range(n+1)] for j in range(n)]
- dp: if have multiple inputs (e.g. 2+ strings matching), use 2d array
- `math.ceil()` round to the next greatest int
- binary search: do iterative instead of recursive version
	```
	l, r = 1, ...
	while l < r:
		...
		if ... :
			l = mid + 1
		else:
			r = mid - 1

	return l
	```
- `div, mod = divmod(num, divisor)`
- valid parenthesis: if encounter ')' and if stack and stack[-1] == '(', pop the last '('
- `string.lower()` convert to lower case
- `string.isupper()`, `string.islower()`, `string.upper()`
- when using zip(a, b) and a b have different lengths, could not iterate through all possible combinations
- common data structure to use: hashmap, 1/2 queue(s), stack, minheap/maxheap, doubly linked list
- 2 deque for keeping max and min
	```
	for num in nums:
		while maxd and maxd[-1] < num:
			maxd.pop()
		while mind and mind[-1] > num:
			mind.pop()
		maxd.append(num)
		mind.append(num)
	```
- bitwise operators
	- ``(int) <<`` shift bits, e.g. ``carry = (x & y) << 1``
	- ``^`` XOR in binary
	- ``~`` flip all bits in binary and add 1 at front / add 1 to the number, e.g. `~24 = -25`
	- ``&`` AND in binary
	- ``|`` OR in binary

- `s = s.replace(" ", "")` remove whitespace in s
- use stack
- `idx ^= 1` flip the `idx` between 0 and 1
- ``chr(int)`` convert int to char
- ``ord(char)`` convert char to int
- ``str.isdigit()`` check if a string only contains numeric values
- ``str.isalpha()`` returns True if all the characters are alphabet letters (a-z).
- random number generation
	- ``random.randrange(start, stop[, step])`` return a randomly selected element from range(start, stop, step). 
	- ``random.randint(a, b)`` return a random integer N such that a <= N <= b. Alias for randrange(a, b+1).
	- ``random.choice(seq)`` return a random element from the non-empty sequence seq. If seq is empty, raises IndexError.
	- ``random.random()`` generate a random number between 0 and 1 
- ``gcd(a, b)``
- ``string.split()`` split by white space
- ``'a' <= c <= 'z'`` string comparison
- ``str.find(sub,start,end)`` returns the lowest index of the substring if it is found in given string. If not found then it returns -1.
- ``min(list, key = ...)`` find the min element in a list by key
todo:
add more from algo: dp, greedy
leetcode tricks
sorting优缺点
OOP problem
memoization
python constructor
functools
numpy

## 3410
https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=216941&highlight=two%2Bsigma
latency vs throughput
thread vs process

## random links
- https://www.evernote.com/shard/s576/client/snv?noteGuid=7e58b450-1abe-43a8-bf82-fbf07f1db13c&noteKey=049802174415b418a2e65f75b744ab72&sn=https%3A%2F%2Fwww.evernote.com%2Fshard%2Fs576%2Fsh%2F7e58b450-1abe-43a8-bf82-fbf07f1db13c%2F049802174415b418a2e65f75b744ab72&title=Interview%2BPreparation
