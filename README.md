# Data_Structures

Here we'll be summarizing how the following DS work:
- Linked Lists
- Hashtables
- Stacks and Queues
- Heaps
- AVL Trees (Balanced Binary Search Tree)

## The Linked List
This data structure is useful for manipulation. That is, it has constant time insertions and deletions (at the front or end). Comparable to an array which are better for random access O(1) and have better locality. However, manipulation of an array, that is, insertion or removal of an item, is expensive. Additionally, a linked list is useful for saving memory. In using an array, we must either know or estimate the size needed. This is how much is allocated to memory; whether or not it is used. Even worse, if we don't initially choose the appropriate size, both increasing or decreasing the size of an array is a costly operation. (A new one would need to be created with the new size, then the data from the old one copied over.) With a linked list that is not necessary, it uses up memory on an as-needed basis. The time complexities for the linked list are compared to that of an array.

| Operation  | Linked List | Array |
| ------------- | ------------- | ------------- |
| Insertion | O(1) | O(n) |
| Deletion | O(1) | O(n) |
| Search | O(n) | O(n) |
| Access | O(n) | O(1) |

#### Implementation:
To have constant time insertions and deletions for both prepend and append, we’ll use a doubly linked list with a persistent ‘sentinel’ node. This node can be known as the ‘head’ of the linked list since it keeps track of the beginning of the list. Additionally, since our list is doubly linked, this ‘sentinel’ node keeps track of the end of the list; creating a ‘circular’ typology. The benefits of this are constant time prepending and appending:

```python
def prepend(self, value):
  n = LinkedList.Node(value, prior=self.head, next=self.next)
  # now the other nodes must point to our new node
  self.head.next.prior = self.head.next = n
  self.length += 1

def append(self, value):
  n = LinkedList.Node(value, prior=self.head.prior, next=self.head)
  # now the other nodes must point to our new node
  n.prior.next = n.next.prior = n
  self.length += 1
```

When implementing in high level languages like python there is no need to actually delete nodes since these languages manage memory for us. For example in the implementation of the delete function, we simply need to change the node references to point past the node we want to delete:

```python
def __delitem__(self, index):
  # get the node we want to delete
  n = self.node_at(index)
  # the node before me will point 'past' me to my next
  n.prior.next = n.next
  # the node after me will point 'past' me to my previous
  n.next.prior = n.prior
  self.length -= 1
  return None
```

## The Hash Table
Now, this data structure is useful for reducing runtime complexity. It has, on average, constant time searching, intertions, and deletions. However, in the worst case scenario, that is, when there is always a collision, the runtime for these operations would be O(n) rather than constant time. This data structure keeps track of (key, value) pairs. The values are searchable via a key. We add the average time complexities for the hash table:

| Operation  | Hash Table | Linked List | Array |
| ------------- | ------------- | ------------- | ------------- |
| Insertion | O(1) | O(1) | O(n) |
| Deletion | O(1) | O(1) | O(n) |
| Search | O(1) | O(n) | O(n) |
| Access | N/A | O(n) | O(1) |

#### Implementation:
For the constant time lookup, we use hash the key value to map it with a unique index. Keep in mind 'unique' is relative, due to the probability of a collision that depends on, both, the hash function used and the quantity of indexes we want to use. 

```python
def __setitem__(self, key, val):
  # Store our entries in tuples
  tmp = (key, val)
  # Compute key's index & create a node
  idx = hash(key) % len(self.indices)
  new_node = OrderedHashtable.Node(len(self.entries))
  # Check if there's no collision
  if not self.indices[idx]:
    self.indices[idx] = new_node
    self.entries.append(tmp)
  # If there is a collision, then we either update an existing value append to linked list
  else:
    current_node = self.indices[idx]
    while current_node:
      # If there's already an entry for this key we'll update its value
      if self.entries[current_node.index][0] == key:
        self.entries[current_node.index] = tmp
        break
      # If the key does not exist, then we'll append to linked list our new data
      if not current_node.next:
        current_node.next = new_node
        self.entries.append(tmp)
        break
      current_node = current_node.next
  self.count += 1
  return
```
#### Uses:
This is a lookup stucture and it can be said that it is best used as such. However, it does have an attractive runtime and it would be a shame not to leverage this structure to reduce the runtime of our programs when we can. For example, in finding the minimum number of swaps required to order a list consisting of consecutive values, the use of a hash table (of which, a python dictionary is) proves useful. In our first solution, 'slowMinSwaps', we search for the value that belongs in the current index we are looking at. This results in a higher runtime complexity. In our second solution, we use a dictionary to avoid that nested for loop. Instead, we can get the index of a desired value form the dictionary in constant time by simply looking it up: `dict[desiredVal]`.

```python
'''It's simple, for every item that is in the wrong spot, swap it with
  the one that belongs in this current item's spot. NOTE: O(n^2)'''

def slowMinSwaps(arr):
    swaps = 0
    for i in range(len(arr)):
        # Check if the current number needs to be swapped
        if i != arr[i]-1:
            # Find value that should be where this current value is
            for j in range(i+1, len(arr)):
                if i == arr[j]-1:
                    # swap them!
                    arr[j], arr[i] = arr[i], arr[j]
                    swaps += 1
                    break
    return swaps

# MUST Optimize!
# Use dict to map posititions with vals. This results in O(n^2) -> O(2n)
def minimumSwaps(arr):
    swaps = 0
    dict = {}
    # Populate dictionary with {value : index}
    for i in range(len(arr)):
        dict[arr[i]] = i
    for i in range(len(arr)):
        # Make sure the value need to be swapped first
        if arr[i]-1 != i:
            tmp = arr[i]
            arr[i], arr[dict[i+1]] = arr[dict[i+1]], arr[i]
            # Update dict after swapping
            dict[tmp] = dict[i+1]
            dict[i+1] = i
            swaps += 1
    return swaps
```

## Stacks and Queues
Both of these data structures are very similar. The stack works just like you'd imagine; you add items to the top and only to the top. Similarly, you only remove items from the top. This is known as a first-in last-out. That is, the first item you add in will be at the bottom of the stack and can only be reached by removing all the items that are on top of it first. Conversely, a queue is first-in first-out. Like a wait line, the first person to get in is also the first person to get out. Both of these structures, at a high level use the same mechanism; add values in only one spot and remove values only from one spot. They, then, have the same runtime complexities. 

| Operation  | Stacks & Queues | Hash Table | Linked List | Array |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| Insertion | O(1) | O(1) | O(1) | O(n) |
| Deletion | O(1) | O(1) | O(1) | O(n) |
| Search | N/A | O(1) | O(n) | O(n) |
| Access | N/A | N/A | O(n) | O(1) |

#### Uses:
The classic example for use of a stack: delimiter checks. For example, We can use a stack to verify an expression has proper use of delimiters, that is, every opening delimiter is paired with a closing delimiter. The below function does just that. Given an expression, `expr`, it returns true iff delimiters are correctly matched; otherwise, it returns false.
```python
delim_openers = '{([<'
delim_closers = '})]>'
def check_delimiters(expr):
    s = Stack()
    for c in expr:
        if c in delim_openers:
            s.push(delim_openers.index(c))
        elif c in delim_closers:
            if s.empty() or delim_closers.index(c) != s.pop():
                return False
    return s.empty()
```

## Heaps
The heap structure is a tree structure that stores it data in an array. It is useful for maintaining a certain value, usually a min or max, readily accessible in constant time. It is important to keep in mind, just accessing this value is a constant time operation, but maintaining the heap property itself costs **O(log(n))** for each value added/removed. If instead of just checking the top value, `peek`, we wanted to `pop` it, that is, get the value and remove it from the heap it would be a **O(log(n))** operation. Initially building the heap has a runtime of **O(n)**.

#### Heapsort
Additionally, the properties of a heap can be leveraged to implement heapsort, a sorting algorithm with a runtime of **O(n log(n))**. Its implementation is simple; to sort a list in ascending order, simply add all the data of that list to a min heap and then pop all the values out into an empty list. 
```python
def heapsort(unsorted_list):
    heap = Heap(lambda x: -x)
    for val in unsorted_list:
        heap.add(val)
    sorted_list = []
    while heap:
        sorted_list.append(heap.pop())
    return sorted_list
```

## AVL Trees:
This structure is a self-balancing binary search tree that makes searching for values easy and quick O(log(n)). This runtime remains for insertion and deletion. AVL Trees maintain the ‘middle’ value at the top, lesser values on the left, and greater values on the right and thus, by its nature, maintains order. It is made up of nodes that each hold a maximum of two child nodes and a value. To maintain balance, a parent node ensures that the heights of both its child nodes differ by no more than 1.

| Operation  | AVL Tree | Stacks & Queues | Hash Table | Linked List | Array |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| Insertion | O(log(n)) | O(1) | O(1) | O(n) |
| Deletion | O(log(n)) | O(1) | O(1) | O(n) |
| Search | O(log(n)) | O(1) | O(n) | O(n) |
| Access | O(log(n)) | N/A | O(n) | O(1) |

#### Implementation:
Naturally, we use binary search for searching. Take the implementation of the `__contains__` function.
```python
def __contains__(self, val):
  def find(node):
    if not node:
      return False
    elif val < node.val:
      return find(node.left)
    elif val > node.val:
      return find(node.right)
    else:
      return True
    return find(self.root)
```

In implementing addition of data, we make a helper function that traverses to where the value we wish to add is supposed to go. Once there, it returns a node containing that value which gets added to the correct parent nodes recursively on the way back out. 
```python
def add(self, val):
  def add_val(t):
    if not t:
      return AVLTree.Node(val)
    elif val > t.val:
      t.right = add_val(t.right)
    elif val < t.val:
      t.left = add_val(t.left)
    # Rebalance the tree when necessary
    if abs(AVLTree.Node.height(t.left) - AVLTree.Node.height(t.right)) > 1:
      AVLTree.rebalance(t)
    return t
  assert(val not in self)
  self.root = add_val(self.root)
```
