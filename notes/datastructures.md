## Overview

A data structure is a way to store/organize data in a computer so that it can be used effectively.

The idea is to reduce the space and time complexity of different tasks.

Data structures can be talked about as:
- Mathematical/logical models, or Abstract data types (ADTs)
    - e.g. List is a ADT 
    - Abstract data types (ADTs) define data and operations, but no implementation details.
- Implementation
    - Arrays are a concrete implementation

Some data structures are:
- Arrays
- Linked List
- Stack
- Queue
- Tree
- Graph

These are some topics that should be explored with data structures:
- Logical view
- Operations
- Cost of operations
- Implementation

## List

Static list as an ADT
- Stores a given number of elements of a given data type
- Write/Modify element at a position
- Read element at a position

Dynamic list as an ADT
- empty list has a size 0
- insert
- remove
- count
- read and modify element at a position
- specify data type

Some implementations of lists are Arrays or Linked Lists.

## Array

Data structure consisting of a collection of elements stored at contiguous memory locations.

**Memory**: Fixed Size - size of array provided before storing data. 
- There may be unused memory if array is larger than what is being used.
- Memory may not be available as one large block when trying to create a large array (array requires continguous memory).

**Time Complexity**:

- Access:  O(1) / Constant time. 
	- Elements are stored in continguous locations. 
	- E.g. the 3rd element of an int array with base memory address X will be at location X+(4*3) [4bytes/32bits for an int]
- Search: 
	- O(n) for Sequential Search
	- O(log n) for Binary Search (Array is Sorted)
- Insertion: O(n) for worst case
	- at start: O(n) - requires shifting of elements after
	- at ith position: O(n)
	- at end: O(1) if array is not full, O(n) if array full. Since the size of the array is fixed, to add more elements after the array is filled up, a new array with a larger size needs to be created and elements copied across (resize).
- Deletion: O(n)
	- Like insertion, worst case happens at beginning of array and requires shifting of all of the elements

## ArrayList

## Linked List

A linear data structure (like arrays) where each element is a separate object. Each element (node) of a list is comprised of two items - the data and reference to next node.

**Memory**: No unused memory.
	- Multiple small blocks of non-contiguous memory instead of one large block.
	- Extra memory required for pointer variables (i.e. 4 extra bytes per node). 

**Time Complexity**:

- Access: O(n)
- Search: O(n)
- Insertion:
	- O(1) if we are at the position to insert
	- at start: O(1)
	- at ith position: O(n)
	- at end: O(n)
- Deletion: 
	- O(1) if we know address of node previous of the node to be deleted

### Linked Lists Types

- **Singly Linked List**: every node stores address or reference of the next node in htte list and the last node has the next address or reference as `NULL`.
- **Doubly Linked List** - Each node has two references associated - one points to the next node and one to the previous node. The advantage of this data structure is that we can traverse in both directions and for deletion, we don't need to have explicit access to the previous node. Eg. NULL<-1<->2<->3->NULL 
- **Circular Linked List** - all nodes are connected to form a circle. There is no `NULL` at the end. A circular linked list can be a singly circular linked list or doubly circular linked list. The advantage is that any node can be made as starting node. This is useful in implementation of circular queues in the linked list

## Stack

- LIFO (Last In, First Out)
- Abstract data type that serves as a collection of elements, with two principal operations:
	- push - add element to collection
	- pop - remove the last element that was added
- Both the operations take place at the same end (top of stack).
- It can be implemented using both array and linked list

### Monotonic Stack
- A stack whose elements are monotonically increasing or decreasing
- If we need to pop smaller elements before pushing a new element, then the stack is *decreasing* from bottom to top. Otherwise its *increasing* from bottom to top.
	- e.g. Before: `[5, 4, 2, 1]`. After: `[5, 4, 3]`. Need to pop smaller elements before pushing 3.

**Time Complexity**:

- Access: O(n)
- Insertion: O(1)
- Deletion: O(1)

### Examples

- Maintaining function calls (last called function must finish execution first)
- Reverse a word
- Check for balanced parenthesis
- Editors where the word typed last is first to be removed (undo)
- Back functionality in web browsers

## Queue

- FIFO (First In, First Out)
- Abstract data type that serves as a collection of elements, with two principal operations:
	- enqueue - adding an element to the collection (added from the rear side)
	- dequeue - removing the first element that was added (removed from the front side)
- It can be implemented using both array and linked list

**Time Complexity**:

- Access: O(n)
- Insertion: O(1)
- Deletion: O(1)

### Example

- Any situation where resources are shared among multiple users and served on a first come first serve basis
	- CPU scheduling, Disk Scheduling
- When data is transferred asynchronously (data not received at the same rate as sent) between two processes. Examples include IO Buffers, pipes, file IO.

### Circular Queue

Last position is connected back to first position to make it a circle. Also called 'Ring Buffer'.

## Tree

- A Tree is an undirected, connected, acyclic graph
- Trees are hierarchial data structures, unlike Arrays, Linked LIsts, Stacks, and Queues (which are linear)
- No two references should point to the same node

## Binary Tree

A tree data structure in which each node has at most two children, often referred to as left and right children. Implemented mainly using Links.

### Binary Tree Representation

- A tree is represented by a pointer to the topmost node in the tree
- If tree is empty then the value of root is `NULL`
- A Binary Tree node contains:
	- Data
	- Pointer to left child
	- Pointer to right child

A Binary Tree can be traversed in two ways
- Depth First Traversal:
	- Inorder (Left - Root - Right)
	- Preorder (Root - Left - Right)
	- Postorder (Left - Right - Root)
- Breadth-First Traversal: Level Order Traversal

### Properties

- Max no. of nodes at level i is `2ⁱ`
- Max no. of nodes for tree of height h is `2ʰ⁺¹-1` (height is considered maximum no. of edges on a path from root to leaf)
- Time Complexity of Tree Traversal: O(n)

## Binary Search Tree (BST)

Type of binary tree which has additional properties:
 - the value in each node must be greater than or equal to any value stored in the left sub-tree
 - the value in each node must be less than or equal to any value stored in the right sub-tree
 - the left and right subtree each must also be a binary search tree

**Time Complexity**:

Where `h` is the height of BST and `n` is the number of nodes in BST:
- Search: O(h)
- Insertion: O(h)
- Deletion: O(h)
- Extra Space: O(n) for pointers

BSTs provide moderate access/search (Quicker than Linked List and slower than arrays).

Bsts provide moderate insertion/deletion (quicker than arrays and slower than linked lists)

If BST is Height Balanced, then `h = O(Log n)`

Self-Balancing BSTs (such as AVL Tree, Red-Black Tree, Splay Tree) make sure that the height of BST remains `O(Log n)`

### Examples

- Main use is in search applications where data is constantly entering/leaving and data needs to be printed in sorted order.
	- For example in E-commerce websites where a new product is added or a product goes out of stock and all products are listed in sorted order

## Binary Heap

A Binary Tree with following properties:
- It is a complete tree (All levels are completely filled expect the possibly the last level has all keys as left as possible). This property makes them suitable to be stored in an array.
-  A Binary Heap is eithe Min Heap or Max Heap.
	- In a Min Binary Heap, the key at the root must be minimum among all keys present in Binary Heap. The same property must be recursively true for all nodes in Binary Tree
	- Max Binary Heap is similar

It is mainly implemented using an array.

**Time Complexity**:
- Get Minimum in Min Heap (or get Max in Max Heap): O(1)
- Extract Minimum Min Heap (or Extract Max in Max Heap): O(Log n)
- Decrease Key in Min Heap (or Decrease key in Max Heap): O(Log n)
- Insertion: O(Log n)
- Deletion: O(Log n)

### Examples
- Implementing efficient priority queues
	- used for scheduling processes in operating systems
	- used in Dijkstra’s and Prim’s graph algorithms
- Heap data structure can be used to efficiently find the k smallest (or largest) elements in an array, merging k sorted arrays, a median of a stream.
- Heap is a special data strcutre and cannot be used for searching a particular element.

## Hashing

- **Hash Function** - a function that converts a given big input key to a smaller practical integer value. The mapped integer value is used as an index in the has table. A good hash function has the following properties:
	- Efficiently computable
	- Uniformly distribute keys (Each table position equally likely for each key)
- **Hash Table** - An array that stores pointers to records corresponding to given input key. An entry in a hash table is NIL if no existing input key has a hash function value equal to the index for the entry.
- **Collision Handling** - Since a hash functions gives a small number for a key, there is possibility that two keys result in the same value. The situation where a newly inserted key maps to an already occupied slot in hash table is a collision. This must be handled using some collision handling technique:
	- Chaining: Make each cell of the hash table point to a linked list of records that have the same hash function value. Chaining is simple but requires additional memory outside of the table.
	- Open Addressing: All elements are stored in the hash table itself and each table entry contains either a record or NIL. When searching for an element, we examine the table slots one by one until the desired element is found, or it is clear that the element is not in the table.


**Complexity**:
- Space: O(n)
- Search: O(1) {Average} O(n) {Worst case}
- Insertion: O(1) {Average} O(n) {Worst case}
- Deletion: O(1) {Average} O(n) {Worst case}

### Example

- Can be used to remove duplicates from a set of elements, or find the frequency of all items.
- For example, check visited URLs in web browsers, or detect spam in firewalls.
- Hasing can be used in any situation where search, insert, deleted is needed in O(1) time.