Algorithms are operations on different data structures and sets of instructions for executing them.

# Searching and Sorting

## Linear Search

Start from the leftmost element and compare one by one with given element.

Time Complexity: O(n)

## Binary Search

- Search a **sorted** array by repeatedly dividing the search interval in half. 
- Start with an interval covering the whole array, and check the value of the item in the middle of the interval. 
- Narrow the interval to the lower/upper half if the value is less/more. 
- Repeatedly check until interval is empty or value is found. 

Time Complexity: O(log n)

