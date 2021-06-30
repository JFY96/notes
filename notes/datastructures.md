## Overview

A data structure is a way to store/organize data in a computer so that it can be used effectively.

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

Memory: Fixed Size - size of array provided before storing data. There may be unused memory if array is larger than what is being used. Memory may not be available as one large block when trying to create a large array (array requires continguous memory).

Time Complexity:

- Access:  O(1) / Constant time. Elements are stored in continguous locations. 
E.g. the 3rd element of an int array with base memory address X will be at location X+(4*3) [4bytes/32bits for an int]

- Insertion at start: O(n) - requires shifting of elements after
Insertion at ith position: O(n)
Insertion at end: O(1) if array is not full, O(n) if array full. Since the size of the array is fixed, to add more elements after the array is filled up, a new array with a larger size needs to be created and elements copied across (resize).

- Search: 

- Remove:

## ArrayList

## Linked List

Memory: No unused memory. Extra memory required for pointer variables (i.e. 4 extra bytes per node. Multiple small blocks of non-contiguous memory instead of one large block.

Time Complexity:

- Access: O(n)

- Insertion at start: O(1)
Insertion at ith position: O(n)
Insertion at end: O(n)

- Search: 

- Remove:

## Other Linked Lists

Doubly Linked List

Circular Linked List

## Tree

- A Tree is an undirected, connected, acyclic graph
- Trees are hierarchial data structures
- No two references should point to the same node

## Binary Tree

A tree data structure in which each node has at most two children, often referred to as left and right children.

## Binary Search Tree (BST)

- Type of binary tree which has the property that the value in each node must be greater than or equal to any value stored in the left sub-tree, and less than or equal to any value stored in the right sub-tree

