Dynamic Programming is an algorithmic technique mainly used for optimising plain recursion.

The idea is to store the results of subproblems so those do not need to be recomputed when needed later. This simple optimisation can reduce time complexities from exponential to polynomial.

There are two methods used:

- **Memoization** - Top Down
- **Tabulation** - Bottom Up

# General Memoization Strategy

- Make a solution to the problem work (brute force)
    - Visualise the problem as a tree
    - implement the tree using recursion
    - test it
- Make it efficient
    - add a memo object
    - add a base case to return memo values
    - store return values into the memo
	 
# General Tabulation Strategy

- Visualize the problem as a table
- size the table based on the inputs
- initialize the table with default values (considering the required return value - e.g. maybe 0 for a counting problem, or false if a boolean)
- seed the trivial answer into the table (e.g. 0 index or 1st index)
- iterate throught the table 
    - fill further positions based on the current position

