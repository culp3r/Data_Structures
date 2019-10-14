# Data_Structures

Here we'll be summarizing how the following DS work:
- Linked Lists
- Hashtables
- Stacks and Queues
- AVL Trees (Balanced Binary Search Tree)

### The Linked List
This data structure is useful for manipulation. That is, it has constant time insertions and deletions. Comparable to an array which are better for random access O(1) and have better locality. However, manipulation of an array, that is, insertion or removal of an item, is expensive. Additionally, a linked list is useful for saving memory. In using an array, we must either know or estimate the size needed. This is how much is allocated to memory; weather or not it is used. Even worse, if we don't initially choose the appropriate size, both increasing or decreasing the size of an array is a costly operation. (A new one would need to be created with the new size, then the data from the old one copied over.) With a linked list that is not necessary, it uses up memory on a as-needed basis. The time complexities for the linked list are compared to that of an array.

| Operation  | Linked List | Array |
| ------------- | ------------- | ------------- |
| Insertion | O(1) | O(n) |
| Deletion | O(1) | O(n) |
| Access | O(n) | O(1) |
| Search | O(n) | O(n) |
