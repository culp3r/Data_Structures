# Data_Structures

Here we'll be summarizing how the following DS work:
- Linked Lists
- Hashtables
- Stacks and Queues
- AVL Trees (Balanced Binary Search Tree)

### The Linked List
This data structure is useful for manipulation. That is, it has constant time insertions and deletions (at the front or end). Comparable to an array which are better for random access O(1) and have better locality. However, manipulation of an array, that is, insertion or removal of an item, is expensive. Additionally, a linked list is useful for saving memory. In using an array, we must either know or estimate the size needed. This is how much is allocated to memory; whether or not it is used. Even worse, if we don't initially choose the appropriate size, both increasing or decreasing the size of an array is a costly operation. (A new one would need to be created with the new size, then the data from the old one copied over.) With a linked list that is not necessary, it uses up memory on an as-needed basis. The time complexities for the linked list are compared to that of an array.

| Operation  | Linked List | Array |
| ------------- | ------------- | ------------- |
| Insertion | O(1) | O(n) |
| Deletion | O(1) | O(n) |
| Access | O(n) | O(1) |
| Search | O(n) | O(n) |
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

### The Hash Table
Now, this data structure is useful for reducing runtime complexity. It has, on average, constant time searching, intertions, and deletions. However, in the worst case scenario, that is, when there is always a collision, the runtime for these operations would be O(n) rather than constant time.   
