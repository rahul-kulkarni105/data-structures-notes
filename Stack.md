# Stack
- Stack is a one-ended linear data structure, having two primary operations. Push() and Pop()
- LIFO - Last In First Out behavior

- Uses
  - used by undo mechanisms in text editors
  - used in compiler syntax checking matching brackets or braces
  - used to model pile of books or plates
  - used behind the scenes to support recursion by keeping track of previous function calls.
  - can be used to do a Depth First Search (DFS) on a graph. can be done by manually or recursion both using stacks.

- Complexity Analysis
  - pushing: O(1) // Because reference is at the top of the stack all the time.
  - popping: O(1)
  - peeking: O(1)
  - searching: O(n) // element we are looking for isn't necessary at the top of the stack hence linear
  - size: O(1)

- Example
  - valid bracket sequence
    - [{()}{}()] => valid
    - [{[}()] => invalid
    - for every bracket in bracket string, push the opening brackets to stack
    - when closing bracket is encountered then check if the stack is empty, if not check if the reverse of the current top (which we can get by pop, is equal to the current bracket.) if yes then push it in.
    - if not the string is invalid.
  - Tower of Hanoi