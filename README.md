# Tech Interview Cheat Sheet

# Table of Content
- [Big O Notation](#o-notation)
- [Data Structures](#data-structures)
  - [String](#string)
  - [Array](#array)
  - [Linked List](#linked-list)
  - [Hash Table or Hash Map](#hash)
  - [Tree](#tree)
  - [Graph](#graph)
- [Algorithms](#algorithms)
  - [Algorithm Basics](#algorithm-basics)
  - [Search Algorithms](#search-algorithms)
    - [Breadth First Search](#breadth-first-search)
    - [Depth First Search](#depth-first-search)
  - [Sorting Algorithms](#sorting-algorithms)
    - [Selection Sort](#selection-sort)
    - [Insertion Sort](#insertion-sort)
    - [Merge Sort](#merge-sort)
    - [Quick Sort](#quick-sort)
- [Language/Frameworks](#language-frameworks)
- [Additional Resources](#additional-resources)


# <a id="o-notation"></a>Big O Notation
### Definition:

The following are the Asymptotic rates of growth from best to worst:
- constant growth - ``O(1)`` Runtime is constant and does not grow with `n`
- logarithmic growth – ``O(log n)`` Runtime grows logarithmically in proportion to `n`
- linear growth – ``O(n)`` Runtime grows directly in proportion to `n`
- superlinear growth – ``O(n log n)`` Runtime grows in proportion _and_ logarithmically to `n`
- polynomial growth – `O(n^c)` Runtime grows quicker than previous all based on `n`
- exponential growth – `O(c^n)` Runtime grows even faster than polynomial growth based on `n`
- factorial growth – `O(n!)` Runtime grows the fastest and becomes quickly unusable for even
small values of `n`

[(source: Soumyadeep Debnath, _Analysis of Algorithms | Big-O analysis_)](https://www.geeksforgeeks.org/analysis-algorithms-big-o-analysis/)

Visualized below; the x-axis representing input size and the y-axis representing complexity:

![#](https://upload.wikimedia.org/wikipedia/commons/thumb/7/7e/Comparison_computational_complexity.svg/400px-Comparison_computational_complexity.svg.png)

[(source: Wikipedia, _Computational Complexity of Mathematical Operations_)](https://en.wikipedia.org/wiki/Computational_complexity_of_mathematical_operations)

#### Big-O notation
Big-O refers to the upper bound of time or space complexity of an algorithm, meaning it worst case runtime scenario. An easy way to think of it is that runtime could be better than Big-O but it will never be worse.
#### Big-Ω (Big-Omega) notation
Big-Omega refers to the lower bound of time or space complexity of an algorithm, meaning it is the best runtime scenario. Or runtime could worse than Big-Omega, but it will never be better.
#### Big-θ (Big-Theta) notation
Big-Theta refers to the tight bound of time or space complexity of an algorithm. Another way to think of it is the intersection of Big-O and Big-Omega, or more simply runtime is guaranteed to be a given complexity, such as `n log n`.

#### What you need to know
- Big-O and Big-Theta are the most common and helpful notations
- Big-O does _not_ mean Worst Case Scenario, Big-Theta does _not_ mean average case, and Big-Omega does _not_ mean Best Case Scenario. They only connote the algorithm's performance for a particular scenario, and all three can be used for any scenario.
- Worst Case means given an unideal input, Average Case means given a typical input, Best case means a ideal input. Ex. Worst case means given an input the algorithm performs particularly bad, or best case an already sorted array for a sorting algorithm.
- Best Case and Big Omega are generally not helpful since Best Cases are rare in the real world and lower bound might be very different than an upper bound.
- Big-O isn't everything. On paper merge sort is faster than quick sort, but in practice quick sort is superior.

#### Complexitiesrates of growth from best to worst:
- constant growth - ``O(1)`` Runtime is constant and does not grow with `n`
- logarithmic growth – ``O(log n)`` Runtime grows logarithmically in proportion to `n`
- linear growth – ``O(n)`` Runtime grows directly in proportion to `n`
- superlinear growth – ``O(n log n)`` Runtime grows in proportion _and_ logarithmically to `n`
- polynomial growth – `O(n^c)` Runtime grows quicker than previous all based on `n`
- exponential growth – `O(c^n)` Runtime grows even faster than polynomial growth based on `n`
- factorial growth – `O(n!)` Runtime grows the fastest and becomes quickly unusable for even
small values of `n`

# <a id="data-structures"></a>Data Structures
### <a id=="string"></a> String

#### Algorithms
- [**Reverse String**](https://leetcode.com/problems/reverse-string/)
Write a function that reverses a string. The input string is given as an array of characters s.
You must do this by modifying the input array in-place with O(1) extra memory.
```
var reverseString = function(s) {
    if(s.length <= 1) {
        return s;
    }
    // return s.reverse();
    
    // another way to do it is to loop through s array and swap and stop at middle index
    // O(n)
    // [0, 1, 2, 3]
    const midIndex = Math.floor(s.length/2);
    for (let i = 0; i < midIndex; i++) {
        // swap far left and far right
        [s[i], s[s.length - i - 1]] = [s[s.length - i - 1], s[i]]
    }
    return s;
};
```
- [**Valid Parentheses**](https://leetcode.com/problems/valid-parentheses/)
Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
```
var isOpenBracket = function(string) {
    return string === '{' || string === '(' || string === '['
}
var isValid = function(s) {
    // edge case
    if(!s || s === 1) {
        return false;
    }
    
    // way to find closing bracket
    const charMatch = {
        ')': '(',
        '}': '{',
        ']': '['
    }
    const charMap = {
        '(': 0,
        '{': 0,
        '[': 0
    }
    
    // must be closed in correct order like a stack
    let stack = ''
    // for every beginning bracket, there must be a matching closing bracket AFTER it
    for (let str of s) {
       if(isOpenBracket(str)) {
           stack += str;
       } else {
           // if closing bracket
           const lastLetter = stack[stack.length - 1]
           const matchingOpenBracket = charMatch[str]
           if(lastLetter === matchingOpenBracket) {
               // remove last letter
               stack = stack.substring(0, stack.length - 1)
           } else {
               return false;
           }
       }
    }
    if(stack === '') {
        return true;
    } else {
        return false;   
    }
};
```
- [**Longest Substring Without Repeating Characters**](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
Given a string s, find the length of the longest substring without repeating characters.
```
var lengthOfLongestSubstring = function(s) {
    // find the length of the longest substring without repeating characters
    // loop through string
    // save characters in a hash map so if the character is already there, u know it's a repeating cahracter
    
    // edge cases
    if(typeof(s) !== 'string') {
        return undefined;
    }
    if(s.length <= 1) {
        return s.length;
    }
    let longestSubstring = 0;
   //  let hashMap = {}; // maybe set instead of hash map because the number of times the characters appear doesn't matter
      let set = new Set();
        let substring = 0;
    // loop through each letter as start of substring
    for (const letter of s) {
   
        // reset substring if that is a repeating character
        if(set.has(letter)) {
            if(substring > longestSubstring) {
                longestSubstring = substring;
            }
            
            // reset the character set
            set = new Set();
            set.add(letter)
            substring = 1;
              // continue;
        } else {
            // increment if no repeating character
            substring += 1;
            set.add(letter);
        }
        if(substring > longestSubstring) {
         longestSubstring = substring;
      };
            
    }
 
    return longestSubstring;
    
};
```

### <a id="array"></a> Array
#### Definition
- Stores data elements based on an sequential, most commonly 0 based, index.
- Based on [tuples](http://en.wikipedia.org/wiki/Tuple) from set theory.
- They are one of the oldest, most commonly used data structures.

#### What you need to know
- Optimal for indexing; bad at searching, inserting, and deleting (except at the end).
- **Linear arrays**, or one dimensional arrays, are the most basic.
  - Are static in size, meaning that they are declared with a fixed size.
- **Dynamic arrays** are like one dimensional arrays, but have reserved space for additional elements.
  - If a dynamic array is full, it copies its contents to a larger array.
- **Multi dimensional arrays** nested arrays that allow for multiple dimensions such as an array of arrays providing a 2 dimensional spacial representation via x, y coordinates.

#### Time Complexity
- Indexing:         Linear array: `O(1)`,      Dynamic array: `O(1)`
- Search:           Linear array: `O(n)`,      Dynamic array: `O(n)`
- Optimized Search: Linear array: `O(log n)`,  Dynamic array: `O(log n)`
- Insertion:        Linear array: n/a,         Dynamic array: `O(n)`

#### Algorithms
- [**Two Sum**](https://leetcode.com/problems/two-sum/)
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
```
var twoSum = function(nums, target) {
    //  return the indices of the two numbers such that they add up to target.
    // CONDITIONS/Questions
    // each input would only have 1 solution
    // cannot use same element twice
    // does it have to be sorted?
    
    // BRUTE FORCE SOLUTION
    // double loop through nums and add them together to see if they equal target
    // if they do, return their index #
    // for (const i = 0; i < nums.length; i++) {
    //     for (const j = i+1; j < nums.length; j++) {
    //         if(nums[j] + nums[i] === target) {
    //             return [i, j]
    //         }
    //     }
    // }
    
    // MORE EFFICIENT
    // more efficient solution utilize a hash map
    // with sums, there is only one other number that can add up to the target
    // so if you store that number in hash map
    // Does it matter if there are multiple numbers? let's assume yes. Or can the same only apppear once.
    // key is the number in the array and value is the index of that number in the array
    const hashMap = {};
    for (let i = 0; i < nums.length; i++) {
        const num = nums[i];
        const diff = target - num;
        if(hashMap[diff] >= 0) {
            return [i, hashMap[diff]]
        }
        
        // store value for later
        hashMap[num] = i;
    }
    return [];
};
```
- [**Two Sum with Input Sorted Array**](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 <= numbers.length.
Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2.
The tests are generated such that there is exactly one solution. You may not use the same element twice.
Your solution must use only constant extra space.
```
var twoSum = function(numbers, target) {
    // numbers are sorted in asc order
    // use 2 pointers: leftIndex and rightIndex
    // if sum of values are the target -> return index
    // if sum of values are larger than target -> move left
    // if sum of values are smaller than target -> move right
    
    let leftIndex = 0;
    let rightIndex = numbers.length - 1;
    while(leftIndex <= rightIndex) {
        const sum = numbers[rightIndex] + numbers[leftIndex]
        if(sum === target) {
            // sorted in smaller nubmer first
            // not 0 based
            return [leftIndex + 1, rightIndex + 1];
        } else if (sum > target) {
            // move left
            rightIndex--;
        } else {
            // sum < target -> move right
            leftIndex++;
        }
        if(rightIndex < 0 || leftIndex < 0 || rightIndex >= numbers.length || leftIndex >= numbers.length) {
            break;
        }
    }
    
    // return indices of the 2 numbers that add up to the target
    return null;
};
```
- **Remove Duplicates from Sorted Array**
Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.
```
var removeDuplicates = function(nums) {
    // let set = new Set();
    for (let i = 1; i < nums.length; i++) {
        const currentNum = nums[i];
        const prevNum = nums[i-1]
        // compare currentNum and prevNum
        if(prevNum === currentNum) {
            // 1st param is index, number of deleting is second
            nums.splice(i, 1);
            i--;
        }
    }
    return nums.length;
};
```
- [**Contains Duplicate**](https://leetcode.com/problems/contains-duplicate/)
Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.
```
var containsDuplicate = function(nums) {
    const set = new Set(nums);
    return nums.length !== set.size;
};
```
- [**Single Number**](https://leetcode.com/problems/single-number/)
Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.
You must implement a solution with a linear runtime complexity and use only constant extra space.
```
var singleNumber = function(nums) {
    let set = new Set()
    // loop through nums array
    // if it is not in set, then add to set
    // if it is in set, remove from set
    // set should have one element leftover
    for (const num of nums) {
        if(set.has(num)) {
            set.delete(num)
        } else {
            set.add(num)
        }
    }
    
    let result;
    for (let item of set.keys()) {
        result = item;
    }
    return result;
};
```
- [**Rotate Array**](https://leetcode.com/problems/rotate-array/)
Given an array, rotate the array to the right by k steps, where k is non-negative.
Example: 
```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```
```
var rotate = function(nums, k) {
    // get k items from the end and insert in beginning
     for (let i = 0; i < k; i++) {
        const lastItem = nums.pop();
        // add item at end to the beginning of array
        nums.unshift(lastItem);
         // probably not optimal because unshift will create a shift in all indexes in array
    }
};
```
- [**Single Number II**]()
Given an integer array nums where every element appears three times except for one, which appears exactly once. Find the single element and return it.

You must implement a solution with a linear runtime complexity and use only constant extra space.
```
var singleNumber = function(nums) {
    // nums is an array of integers that always has elements that appear 3 times except for 1 element
    // every element appears 3 times except for 1
    // use hash map to keep track of how many with key is the number and value is the count
    const hashMap = {};
    for (const num of nums) {
        hashMap[num] = hashMap[num] ? (hashMap[num] + 1) : 1;
    }
    
    // loop through keys in hashMap and look for what has value of 1
    for (const [key, value] of Object.entries(hashMap)) {
        if(value === 1) {
            return key;
        }
    }
};
```

### <a id="linked-list"></a> Linked List
#### Definition
- Stores data with **nodes** that point to other nodes.
  - Nodes, at its most basic it has one datum and one reference (another node).
  - A linked list _chains_ nodes together by pointing one node's reference towards another node.

#### What you need to know
- Designed to optimize insertion and deletion, slow at indexing and searching.
- **Doubly linked list** has nodes that also reference the previous node.
- **Circularly linked list** is simple linked list whose **tail**, the last node, references the **head**, the first node.
- **Stack**, commonly implemented with linked lists but can be made from arrays too.
  - Stacks are **last in, first out** (LIFO) data structures.
  - Made with a linked list by having the head be the only place for insertion and removal.
- **Queues**, too can be implemented with a linked list or an array.
  - Queues are a **first in, first out** (FIFO) data structure.
  - Made with a doubly linked list that only removes from head and adds to tail.

#### Time Complexity
- Indexing:         Linked Lists: `O(n)`
- Search:           Linked Lists: `O(n)`
- Optimized Search: Linked Lists: `O(n)`
- Append:           Linked Lists: `O(1)`
- Prepend:          Linked Lists: `O(1)`
- Insertion:        Linked Lists: `O(n)`


### <a id="hash"></a> Hash Table or Hash Map
#### Definition
- Stores data with key value pairs.
- **Hash functions** accept a key and return an output unique only to that specific key.
  - This is known as **hashing**, which is the concept that an input and an output have a one-to-one correspondence to map information.
  - Hash functions return a unique address in memory for that data.

#### What you need to know
- Designed to optimize searching, insertion, and deletion.
- **Hash collisions** are when a hash function returns the same output for two distinct inputs.
  - All hash functions have this problem.
  - This is often accommodated for by having the hash tables be very large.
- Hashes are important for associative arrays and database indexing.

#### Time Complexity
- Indexing:         Hash Tables: `O(1)`
- Search:           Hash Tables: `O(1)`
- Insertion:        Hash Tables: `O(1)`


### <a id="tree"></a> Tree
#### Definition
- **Binary Tree**: tree like data structure where every node has at most two children.
  - There is one left and right child node.

#### What you need to know
- Designed to optimize searching and sorting.
- A **degenerate tree** is an unbalanced tree, which if entirely one-sided, is essentially a linked list.
- They are comparably simple to implement than other data structures.
- Used to make **binary search trees**.
  - A binary tree that uses comparable keys to assign which direction a child is.
  - Left child has a key smaller than its parent node.
  - Right child has a key greater than its parent node.
  - There can be no duplicate node.
  - Because of the above it is more likely to be used as a data structure than a binary tree.

#### Time Complexity
- Indexing:  Binary Search Tree: `O(log n)`
- Search:    Binary Search Tree: `O(log n)`
- Insertion: Binary Search Tree: `O(log n)`

#### Class
```
class TreeNode {
  constructor(val) {
    this.val = val;
    this.children = [];
  }
}

class Tree {
  constructor(val) {
    this.root = null;
  }
}
```
```
class BinaryTreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}
```

#### Breadth-First Search (BFS)
- visit nodes by row
```
function breadthFirstSearch(root) {
  let queue = []; // to keep track of children in rows. Queue follows FIFO principle.
  let result = [];
  let node;
  queue.push(root);  // start the queue
  while(queue.length) {
    node = queue.shift(); // dequeue - remove first item in queue
    result.push(node);  // put the first item in queue into results array
    if(node.left) queue.push(node.left);
    if(node.right) queue.push(node.right);
  }
  return result;
}
```

#### Depth-First Search (DFS)
- starts at root to end of branch before backtracking
1. **In-Order** - left, root, right
2. **Pre-Order** - root, left, right
3. **Post-Order** - left, right, root

```
function inOrderDFS() {
  let visited = [];

  function traverse(node) {
    if(node.left) traverse(node.left);    // if node has left, use recursion to find left-most leaf node
    visited.push(node.val);
    if(node.right) traverse(node.right);     // if node has right, use recursion to find right-most leaf node
  }

  traverse(root);
  return visited;
}
```

```
// parent node before child nodes (left, right)
function preOrderDFS() {
  let visited = [];
  
  function traverse(node) {
    visited.push(node.val);
    if(node.left) traverse(node.left);    // if node has left, use recursion to find left-most leaf node
    if(node.right) traverse(node.right);     // if node has right, use recursion to find right-most leaf node
  }

  traverse(root);
  return visited;
}
```

```
// parent node after child nodes (left, right)
function postOrderDFS() {
  let visited = [];
  
  function traverse(node) {
    if(node.left) traverse(node.left);    // if node has left, use recursion to find left-most leaf node
    if(node.right) traverse(node.right);     // if node has right, use recursion to find right-most leaf node
    visited.push(node.val);
  }

  traverse(root);
  return visited;
}
```
#### Algorithms
- **Height of Binary Tree**
```
// recursive solution
// time 0(n) - # of nodes, space: 0(h) - height of tree
function heightBinaryTree(node {
  if (root === null) return 0;
  // recur for left and right subtree to consider maximum height
  return 1 + Math.max(height(node.left), height(node.right));
})
```
- **Is it the same tree?**
Given 2 roots p and q, check if they are the same or not.  Same is structurally identical and all have the same values.
```
// Recursive solution
// time 0(n) - # of nodes because you visit every node, space: 0(log(n)) for balanced tree and 0(n) for completely unbalanced tree to keep recursion stack
function isSameTree(p, q) {
  // base case
  if (p === null && q === null) return true;
  if (p === null || q === null) return false;

  if (p.val !== q.val) return false;

  return isSameTree(p.right, q.right) && isSameTree(p.left, q.left);
}
```
```
// Iterative solution
// time 0(n) - # of nodes because you visit every node, space: 0(log(n)) for balanced tree and 0(n) for completely unbalanced tree to keep recursion stack

// helper function
function isSameNodeVal(p,q) 
  // base case
  if (p === null && q === null) return true;
  if (p === null || q === null) return false;
  if (p.val !== q.val) return false;
  return true;
}
function isSameTree(p, q) {
  if(!isSameNodeVal(p,q)) return false; // compare both roots
  // create queues for both p and q and add root to them
  let pQueue = [p];
  let qQueue = [q];
  while (pQueuee.length >= 1) {
    // remove first element from both queues
    p = pQueue.shift();
    q = qQueue.shift();

    if(!isSameNodeVal9p,q) return false;
    if(p) {
      pQueue.push(p.left);
      pQueue.push(q.right);
    }
    if(q) {
      qQueue.push(q.left);
      qQueue.push(q.right);
    }
  }
  return true;
}
```
- **Is the tree symmetric?**
isSymmetric if root is same and right subtree is reflection of left subtree
```
// Recursive solution
// time 0(n) - # of nodes because you visit every node, space: 0(log(n)) for balanced tree and 0(n) for completely unbalanced tree to keep recursion stack
function isSymmetric(root) {
  function isMirror(node1, node 2) {
    if(node1 === null && node2 === null) return true;
    if(node1 === null || node2 === null) return false;
    return (node1.val === node2.val) && isMirror(node1.left, node2.right) && isMirror(node1.right, node2.left);
  }
  return isMirror(root, root);
}
```
```
// Iterative solution
// time 0(n) - # of nodes because you visit every node, space: 0(log(n)) for balanced tree and 0(n) for completely unbalanced tree to keep recursion stack

function isSymmetric(root) {
  const leftQueue = [root.left];
  const rightQueue = [root.right];
  while(leftQueue.length && rightQueue.length) {
    // take first node from the queues
    const leftNode = leftQueue.shift();
    const rightNode = rightQueue.shift();

    if(!leftNode && !rightNode) continue; //skip
    if(!leftNode || !rightNode || (leftNode.val !== rightNode.val)) return false;
    
    leftQueue.push(leftNode.left, leftNode.right);
    rightQueue.push(rightNode.right, rightNode.left);  // push in opposite order
  }
  return true;
  }
  return true;
}
```

# <a id="algorithms"></a> Algorithms
## <a id="algorithm-basics"></a> Algorithm Basics
### Recursive Algorithms
#### Definition
- An algorithm that calls itself in its definition.
  - **Recursive case** a conditional statement that is used to trigger the recursion.
  - **Base case** a conditional statement that is used to break the recursion.

#### What you need to know
- **Stack level too deep** and **stack overflow**.
  - If you've seen either of these from a recursive algorithm, you messed up.
  - It means that your base case was never triggered because it was faulty or the problem was so massive you ran out of alloted memory.
  - Knowing whether or not you will reach a base case is integral to correctly using recursion.
  - Often used in Depth First Search


### Iterative Algorithms
#### Definition
- An algorithm that is called repeatedly but for a finite number of times, each time being a single iteration.
  - Often used to move incrementally through a data set.

#### What you need to know
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

#### Pseudo Code of Moving Through an Array
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

### Greedy Algorithms
#### Definition
- An algorithm that, while executing, selects only the information that meets a certain criteria.
- The general five components, taken from [Wikipedia](http://en.wikipedia.org/wiki/Greedy_algorithm#Specifics):
  - A candidate set, from which a solution is created.
  - A selection function, which chooses the best candidate to be added to the solution.
  - A feasibility function, that is used to determine if a candidate can be used to contribute to a solution.
  - An objective function, which assigns a value to a solution, or a partial solution.
  - A solution function, which will indicate when we have discovered a complete solution.

#### What you need to know
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

## <a id="search-algorithms"></a>Search Algorithms
### <a id="breadth-first-search"></a>Breadth First Search
#### Definition
- An algorithm that searches a tree (or graph) by searching levels of the tree first, starting at the root.
  - It finds every node on the same level, most often moving left to right.
  - While doing this it tracks the children nodes of the nodes on the current level.
  - When finished examining a level it moves to the left most node on the next level.
  - The bottom-right most node is evaluated last (the node that is deepest and is farthest right of it's level).

#### What you need to know
- Optimal for searching a tree that is wider than it is deep.
- Uses a queue to store information about the tree while it traverses a tree.
  - Because it uses a queue it is more memory intensive than **depth first search**.
  - The queue uses more memory because it needs to stores pointers

#### Time Complexity
- Search: Breadth First Search: O(V + E)
- E is number of edges
- V is number of vertices

### <a id="depth-first-search"></a>Depth First Search
#### Definition
- An algorithm that searches a tree (or graph) by searching depth of the tree first, starting at the root.
  - It traverses left down a tree until it cannot go further.
  - Once it reaches the end of a branch it traverses back up trying the right child of nodes on that branch, and if possible left from the right children.
  - When finished examining a branch it moves to the node right of the root then tries to go left on all it's children until it reaches the bottom.
  - The right most node is evaluated last (the node that is right of all it's ancestors).

#### What you need to know
- Optimal for searching a tree that is deeper than it is wide.
- Uses a stack to push nodes onto.
  - Because a stack is LIFO it does not need to keep track of the nodes pointers and is therefore less memory intensive than breadth first search.
  - Once it cannot go further left it begins evaluating the stack.

#### Time Complexity
- Search: Depth First Search: O(|E| + |V|)
- E is number of edges
- V is number of vertices


#### Breadth First Search Vs. Depth First Search
- The simple answer to this question is that it depends on the size and shape of the tree.
  - For wide, shallow trees use Breadth First Search
  - For deep, narrow trees use Depth First Search

#### Nuances
  - Because BFS uses queues to store information about the nodes and its children, it could use more memory than is available on your computer. (But you probably won't have to worry about this.)
  - If using a DFS on a tree that is very deep you might go unnecessarily deep in the search. See [xkcd](http://xkcd.com/761/) for more information.
  - Breadth First Search tends to be a looping algorithm.
  - Depth First Search tends to be a recursive algorithm.


## <a id="sorting-algorithms"></a>Sorting Algorithms

### <a id="selection-sort"></a>Selection Sort
#### Definition
- A comparison based sorting algorithm.
  - Starts with the cursor on the left, iterating left to right
  - Compares the left side to the right, looking for the smallest known item
    - If the left is smaller than the item to the right it continues iterating
    - If the left is bigger than the item to the right, the item on the right becomes the known smallest number
    - Once it has checked all items, it moves the known smallest to the cursor and advances the cursor to the right and starts over
  - As the algorithm processes the data set, it builds a fully sorted left side of the data until the entire data set is sorted
- Changes the array in place.

#### What you need to know
- Inefficient for large data sets.
- Very simple to implement.

#### Time Complexity
- Best Case Sort: `O(n^2)`
- Average Case Sort: `O(n^2)`
- Worst Case Sort: `O(n^2)`

#### Space Complexity
- Worst Case: `O(1)`

#### Visualization
![#](https://upload.wikimedia.org/wikipedia/commons/9/94/Selection-Sort-Animation.gif)

[(source: Wikipedia, _Selection Sort_)](https://en.wikipedia.org/wiki/Selection_sort)

### <a id="insertion-sort"></a>Insertion Sort
#### Definition
- A comparison based sorting algorithm.
  - Iterates left to right comparing the current cursor to the previous item.
  - If the cursor is smaller than the item on the left it swaps positions and the cursor compares itself again to the left hand side until it is put in its sorted position.
  - As the algorithm processes the data set, the left side becomes increasingly sorted until it is fully sorted.
- Changes the array in place.

#### What you need to know
- Inefficient for large data sets, but can be faster for than other algorithms for small ones.
- Although it has an `O(n^2)` time complexity, in practice it is slightly less since its comparison scheme only requires checking place if it is smaller than its neighbor.

#### Time Complexity
- Best Case: `O(n)`
- Average Case: `O(n^2)`
- Worst Case: `O(n^2)`

#### Space Complexity
- Worst Case: `O(n)`

#### Visualization
![#](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)

[(source: Wikipedia, _Insertion Sort_)](https://en.wikipedia.org/wiki/Insertion_sort)

### <a id="merge-sort"></a>Merge Sort
#### Definition
- A divide and conquer algorithm.
  - Recursively divides entire array by half into subsets until the subset is one, the base case.
  - Once the base case is reached results are returned and sorted ascending left to right.
  - Recursive calls are returned and the sorts double in size until the entire array is sorted.

#### What you need to know
- This is one of the fundamental sorting algorithms.
- Know that it divides all the data into as small possible sets then compares them.

#### Time Complexity
- Worst Case: `O(n log n)`
- Average Case: `O(n log n)`
- Best Case: `O(n)`

#### Space Complexity
- Worst Case: `O(1)`

#### Visualization
![#](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Merge_sort_algorithm_diagram.svg/400px-Merge_sort_algorithm_diagram.svg.png)

[(source: Wikipedia, _Merge Sort_)](https://en.wikipedia.org/wiki/Merge_sort)

### <a id="quick-sort"></a>Quicksort
#### Definition
- A divide and conquer algorithm
  - Partitions entire data set in half by selecting a random pivot element and putting all smaller elements to the left of the element and larger ones to the right.
  - It repeats this process on the left side until it is comparing only two elements at which point the left side is sorted.
  - When the left side is finished sorting it performs the same operation on the right side.
- Computer architecture favors the quicksort process.
- Changes the array in place.

#### What you need to know
- While it has the same Big O as (or worse in some cases) many other sorting algorithms it is often faster in practice than many other sorting algorithms, such as merge sort.

#### Time Complexity
- Worst Case: `O(n^2)`
- Average Case: `O(n log n)`
- Best Case: `O(n log n)`

#### Space Complexity
- Worst Case: `O(log n)`

#### Visualization
![#](https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif)

[(source: Wikipedia, _Quicksort_)](https://en.wikipedia.org/wiki/Quicksort)

#### Merge Sort Vs. Quicksort
- Quicksort is likely faster in practice, but merge sort is faster on paper.
- Merge Sort divides the set into the smallest possible groups immediately then reconstructs the incrementally as it sorts the groupings.
- Quicksort continually partitions the data set by a pivot, until the set is recursively sorted.

## <a id="additional-resources"></a>Additional Resources
[Khan Academy's Algorithm Course](https://www.khanacademy.org/computing/computer-science/algorithms)
