# Tech Interview Cheat Sheet

# Table of Content
- [Big O Notation](#o-notation)
- [Data Structures](#data-structures)
  - [String](#string)
  - [Array](#array)
  - [Hash Table or Hash Map](#hash)
  - [Linked List](#linked-list)
  - [Stack](#stack)
  - [Queue](#queue)
  - [Tree](#tree)
    - [Binary Tree](#binary-tree)
    - [Breadth First Search](#breadth-first-search)
    - [Depth First Search](#depth-first-search)
    - [Binary Heaps (min-heap and max-heap)](#binary-heap)
    - [Tries (Prefix Tries)](#prefix-tries)
  - [Graph](#graph)
- [Algorithms](#algorithms)
  - [Algorithm Basics](#algorithm-basics)
  - [Searching](#searching)
    - [Linear Search](#linear-search)
    - [Binary Search](#binary-search)
  - [Sorting Algorithms](#sorting-algorithms)
    - [Selection Sort](#selection-sort)
    - [Insertion Sort](#insertion-sort)
    - [Merge Sort](#merge-sort)
    - [Quick Sort](#quick-sort)
- [System Design and Scalability](#system-design)
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
### <a id="string"></a>String

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



### <a id="linked-list"></a> Linked List
#### Definition
- Stores data with **nodes** that point to other nodes.
  - A linked list _chains_ nodes together by pointing one node's reference towards another node.

#### What you need to know
- Designed to optimize insertion and deletion, slow at indexing and searching.
- excellent alternative to arrays when insertion and deletion at beginning are frequenely required (array insertion at beginning requires all indices after to re-index, which is inefficient) (Arrays contain built-in index whereas linked lists don't) 
- Linked lists are the foundation of Stacks and Queues
- **Single-linked list** vs **double-linked list**
  - Nodes on single-linked list have `.val` and `.next`, while double-linked lists have that and `.prev` property
  - DLL is better than SLL for finding nodes (done in half the time because you can start at the tail instead of head)
  - DLL takes up more memory considering extra pointer
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

#### Class
```
class LinkedList {
  constructor() {
    this.head = null;
    this.size = 0;
  }
}

class Node = {
  constructor(val) {
    this.val = val;
    this.next = null;
    this.prev = null; // exists on double-linked lists only
  }
}
```

#### Algorithms
- **Get Node By Index**
```
function getNodeByIndex(index) {
  // check if index is within bounds
  if(index < 0 || index >= this.length) return null; 

  let counter = 0;
  let currentNode = this.head;
  while (counter !== index) {
    currentNode = currentNode.next;
    counter++;
  }
  return currentNode;
}
```

- **Set Node By Index**
```
function setNodeByIndex(index, val) {
  let foundNode = this.get(index);
  if(foundNode) {
    foundNode.val = value;
    return true;
  }
  return false;
}
```

- **Reverse SLL** 
```
// loop through linked list from head to tail, changing every node's next property to what their previous node was
// swap head and tail props
```
- **Remove duplicates from unsorted linked list**
```
function removeDuplicates(linkedList) {
  let linkedListMap = {}; // 0(1) access
  let currentNode = linkedList.head;
  let prevNode = null;
  while(currentNode !== null) {
    if(linkedListMap[currentNode.val] === true) {
      prevNode.next = currentNode.next;  // remove this node
    } else {
      linkedListMap[currentNode.val] = true
    }
    prevNode = currentNode;
    currentNode = currentNode.enxt;
  }
  return linkedList;
}
```
- **Return Kth to Last**
Find kth to last element of singly linked list
This solution is 0(n) time and 0(1) space
```
// use 2 pointers, a slow pointer and fast pointer
// make it so p1 and p2 are k nodes apart
// when p1 hits end of linked list, p2 will be length - k notes into the list or k nodes from teh end
function kthToLast(linkedList, k) {
  let p1 = head;
  let p2 = head;
  // move p1 pointer k nodes into the list
  for (let i = 0; i < k; i++) {
    if(p1 === null) return null; // out of bounds!
    p1 = p1.next;  // the check above will prevent error of calling property on undefined
  }
  // move both pointers at same pace here
  // when p1 hits end, p2 will be at the correct result element
  while (p1 !== null) {
    p1 = p1.next;
    p2 = p2.next;
  }
  return p2;
}
```
- **Delete middle node of single-linked list**
```
function deleteMiddleNode(node) {
  if (node === null || node.next === null) return false;

  // make copy of the next node;
  let nextNode = node.next;  

  // make current nodehave same properties as next node
  node.val = nextNode.val;  
  node.next = nextNode.next;

  return true;
}
// this cannot be solved if node to be deleted is the tail or last node in single-linked list because we have no access to prev property
```
- **Partition**
All elements less than x on left partition and . Parittion value can be in right partition. Doesn't have to be in between.
```
/*
array shifts can be very expensive but linked list much easier
create 2 linked lists
1) elements < x
2) elements >= x
combine the 2 linked lists
*/

function partition(node, x) {
  let head = node;
  let tail = node;
  while(node !== null) {
    let nextNode = node.enxt;
    if(node.data < x) {
      // insert node at head;
      node.next = head;
      head = newNode;
    } else { // greater than or equal to x
    //insert node at tail
      tail.next = newNode;
      tail = newNode;
    }
    node = nextNode;
  }
  tail.next = null;
  return head;  // return linked list
}
```
- **Sum Lists**
Adding 2 linked lists together and returning a linked list
```
/*
- consider that if they are different sizes, you will have to insert 0's before to pad
- compare lengths of list at beginning
- pad shorter list with 0's
*/

```
- **Intersecting linekd lists**
```
/*
- if 2 linked lists, intersect, they will share same tail (reference is the same, checking value of tail is not enough)
- since both lined lists might not be same size or length, you ignore the difference (longer SLL minus shorter SLL) and move pointer to another node so that both lists are the same length;
1) run through each linked list to get lengths and tails
2) compare tails by reference (not value). If they're not tehs ame -> no intersection and immediately return
3) set 2 pointers to start of each SLL, but move pointer to longer SLL so that both lists are same size
4) traverse on each linked list until pointers are thes ame and pointer on shorter and longer SSL collide or are equal.
- 0(A + B) time and 0(1) space
  - A is length of first SLL
  - B is length of second SLL
*/
```

- **Beginning of circular linked list**
```
- Collision spot is k nodes from loop start
1) create 2 pointers (fast and slow pointer)
2) Slow Runner - 0 steps into loop
Fast Runner - k steps into loop
SLow Runner - k steps behidn Fast Runner
Fast Runner - loop_size - k steps behind Slow Runner
Fast Runner eventually catches up to Slow RUnner at rate of 1 step per unit of time
3) Move FastPointer at 2 stesps vs Slow pointer at 1 step/time
WHen they collide, move SLowPointer to LinkedList head. Keep Fast pointer where it is.
Move SLow pointer and fast pointer at 1 step/time -> return collision spot.
```

### <a id="stack"></a> Stacks (LIFO)
#### What you need to know
- insertion and removal = 0(1)
- searching and access = 0(n) -> if you need to search or access within the stack, use a different structure
- can push and pop with array in javascript
- for pop() and push() in a linked list, you can add to the head so it is optimized for both SLL and DLL
#### Methods
- .pop() - remove top item
- .push(item) - adds item to top of stack
- .peek() - return top of stack
- .isEmpty() - return true only if stack is empty
#### Uses
- Javascript call stack or recursion
- routing (browser history object)
- undo/redo
- versioning
#### Class
```
class Queue {
  constructor() {
    this.first = null;
    this.last = null;
    this.size = 0;
  }
}

class Node {
  constructor(val) {
    this.val = null;
    this.next = null;
  }
}
```

### <a id="queue"></a> Queues (FIFO)
#### What you need to know
- insertion and removal = 0(1)
- searching and access = 0(n) -> if you need to search or access within the queue, use a different structure
- don't really want to implement queue as an array because inserting and removing the first item requires re-indexing all items afterwards, which is 0(n) vs 0(1) with queue or linked list
#### Methods
- .enqueue() - add item to end of line (get in line) (similar to array.unshift() add to beg)
- .dequeue() - remove first in line (get out of line)(remove head) (similar to array.pop() remove from end)
- .pop() - remove first item in queue
- .push(item) - adds item to end of queue
- .peek() - return first item
- .isEmpty() - return true only if queue is empty
#### Uses
- background tests
- print queue
- uploading files
#### Class
```
class Queue {
  constructor() {
    this.first = null;
    this.last = null;
    this.size = 0;
  }
}

class Node {
  constructor(val) {
    this.val = null;
    this.next = null;
  }
}
```

### <a id="tree"></a> Tree
#### Definition
- data structure that consists of nodes in parent/child relationship (if siblings are connected, then that's a **graph**)
  - tree is a type of graph, but not the other way around
  lists-linear vs trees - nonlinear
  - each tree has a root node
- **Leaf node** - no children
- **Edge** - connection btw 1 node to another

#### What you need to know
- Designed to optimize searching and sorting.
- A **degenerate tree** is an unbalanced tree, which if entirely one-sided, is essentially a linked list.
- They are comparably simple to implement than other data structures.

#### Uses
- HTML Dom
- network routing
- computer folders
- artificial intelligence
- json

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

### <a id="binary-tree"></a> Binary Search Tree
- **Binary Tree**: tree-like data structure where every node has up to 2 child nodes (1 left and 1 right child node)
- different from ternary tree and n-ary trees
- **Binary search trees**
  - 1 type of binary tree where all left descendants have a value sm
  - All left descendants < parent node
  - all right descendants > parent node
  - Clarify with interviewer
    - are there duplicate values?
    - if there are child nodes with same value as parent node, does it belong on left or right side?
- **Balanced Tree**: balanced enough to ensure 0(log n) times for insertion and search/find
- **Complete binary tree**: tree in which every level is filled from left to right (except lowest level) but must fill left before right
- **Full binary tree**: every node has 0 or 2 children
- **Perfect binary tree**: both full and complete, each level has max # of nodes (exactly (2^k - 1) nodes)

#### Time Complexity
- Search:    Binary Search Tree: `O(log n)`
- Insertion: Binary Search Tree: `O(log n)`
- if you double the nodes, you increase 1 extra step
- worst case scenario is if the binary tree is not balanced (like SLL) -> 0(n) for search

#### Class
```
class BinaryTreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}
```


#### Algorithms
- **Find Min Node** (left-most node)
```
function findMinNode(node) {
  if (node.left === null) {
    return node;
  } else {
    return findMinNode(node.left);
  }
}
```

- **Contains Value**
```
function contains(val) {
  let currentNode = this.root
  let foundNode = false;
  while(currentNode && !foundNode) {
    if (currentNode.val === val) {
      foundNode = true;
      break;
    } else if (val < currentNode.val) {
      currentNode = currentNode.left;
    } else {
      currentNode = currentNode.right;
    }
  }
  return foundNode;
}
```

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

- **Is binary tree height-balanced?**
Given a binary tree, determine if it is height-balanced.
For this problem, a height-balanced binary tree is defined as: a binary tree in which the left and right subtrees of every node differ in height by no more than 1.
```
const ckeckHeight = node => {
  if (node === null) return 0;
  
  const left = ckeckHeight(node.left);
  const right = ckeckHeight(node.right);
  
  if (
    // if a previous call has returned false,
    // we need to pass false all the way up
    left === false ||
    right === false ||
    Math.abs(left - right) > 1
  ) {
    return false;
  }
  
  // height of a node
  return Math.max(left, right) + 1;
};

const isBalanced = root => {
  if (root === null) return true;
  
  return ckeckHeight(root) !== false;
};
```

### <a id="breadth-first-search"></a>Breadth First Search
#### Definition
- An algorithm that searches a tree (or graph) by searching levels of the tree first, starting at the root.
  - It finds every node on the same level, most often moving left to right. (visits nodes by row)
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

#### Algorithm
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

#### Algorithm
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

### Breadth First Search Vs. Depth First Search
- The simple answer to this question is that it depends on the size and shape of the tree.
  - For wide, shallow trees use Breadth First Search
  - For deep, narrow trees use Depth First Search
  - Because BFS uses queues to store information about the nodes and its children, it could use more memory than is available on your computer. (But you probably won't have to worry about this.)
  - If using a DFS on a tree that is very deep you might go unnecessarily deep in the search. See [xkcd](http://xkcd.com/761/) for more information.
  - Breadth First Search tends to be a looping algorithm.
  - Depth First Search tends to be a recursive algorithm.


### <a id="binary-heap"></a> Binary Heaps (Min Heaps and Max Heaps)
#### Definition
- heap is a category of trees
- completely binary tree filled from left to right (not necessarily all right have 2 child nodes yet)
- no relationship for siblings
- **Min Heap** - each parent node < children -> root = min element
- **Max Heap** - each parent node > children -> root = max element
- **Represent Binary Heap as array**
  - n = index
  - left child = 2n + 1
  - right child = 2n + 2
  - parent = Math.floor((n-1)/2)

#### Time Complexity
- Search:    Binary Search Tree: `O(log n)`
- Insertion: Binary Search Tree: `O(log n)`
  - start by adding node to right-most node (or end of the array) and then swap (bubble up) to swap with parents to its place
- if you double the nodes, you increase 1 extra step
- worst case scenario is if the binary tree is not balanced (like SLL) -> 0(n) for search

### <a id="tries"></a>Trie(Prefix Trees)
#### Definition
- variant of n-any tree
- stores characters at each node
- each path represents a word
- Terminating Trie Node means word is complete

#### Time Complexity
- same run time as hash table 0(k) or O(1)
  - k = character length


### <a id="graphs"></a>Graphs
#### Definition
- collection of nodes with edges (bridges) that connect them
- **directed** (1-way) vs **undirected** (2-way)
- **Connected graph**: graph between all vertices
- **Acyclic graph**: graph without cycles

#### Graph Search
  - **Breadth-First Search**: start at root adn go to all neighbors of root before going to their neighbors
    - use queue and store visited
    - better for finding shortest path between nodes
  - **Depth-First Search**: start at root and explore each branch completely before moving onto next branch
    - preferred if we want to visit every node
    - recursive
  - when implementing graph search, check if each node has been visited because graphs have multiple ways to get to the ame place whereas trees only have one way

#### Uses
- Facebook friends

#### Class

#### Algorithms
- **Bidirectional Search**: find shortest path between source and destination nodes
```
run 2 simultaneous breadth-first searches, 1 from each node -> when their searches collide -> found path
```

# <a id="algorithms"></a> Algorithms
## <a id="algorithm-basics"></a> Algorithm Basics
## <a id="searching"></a> Searching
### <a id="linear-search">Linear Search</a>
- loop through all items in array once to see if there is a match
- Avg and worst 0(n) - if it is found towards the end
- Best 0(1) - if it is found early
```
function linearSearch(arr, val) {
  for (let i = 0; i<arr.length; i++) {
    if (arr[i] === val) {
      return 1;
    }
  }
  return -1;
}
```

### <a id="binary-search">Binary Search</a>
- input is **sorted array**
- search for elements by comparing midpoint of array -> if less, search left half, and if more, search right half -> get new middle and cut in half again until you find it
- Runtime:
  - avg and worst O(log n)
    - if you double nodes -> add only 1 more step
    - 2^x = n  #x: 2^5 = 32
  - Best 0(1)

```
function binarySearch(arr, elem) {
  let leftIndex = 0; 
  let rightIndex = arr.length - 1;
  let middleIndex;
  while (leftIndex <= rightIndex) {
    middleIndex = Math.floor((leftIndex + rightIndex)/2)
    if(elem < arr[middleIndex]) {
      rightIndex = middleIndex - 1;
    } else if (elem > arr[middleIndex]) {
      leftIndex = middleIndex + 1;
    } else {
      return middle;
    }
    return -1 // error or can't find
  }
}
```

### Techniques
- **Sliding window**
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

### <a id="system-design"></a>System Design and Scalability
#### Ex: Twitter
1. Ask interviewer questions
  - Ask about your assumptions
  - Estimate when necessary
2. Scope the problem
- "Twitter has many different features. Is there a particular area you want me to focus on?"
- Ex: Design Tiny Urls
  1. Url is picked by user or is it auto-generated
  2. Lasts forever or times out
  3. What would it be used for
3.  Draw user interface and discuss core features
- clarify features
- identify 2 core features by drawing and let interviewer correct you aot this time
- Ex: draw user feed, who to follow, trending categories
  - Tweeting
    - What does a tweet look like in database?
    - How to make #topic and @mention systems
    - How to re-implement retweets
  - Feed
    - How to interesting tweets at top of feed
    - User in database
4.  Possible implementation
  - can start with API design and then draw out components with props
  - focus on data model (Ex: how to store tweets or users or follows in db)
  - I would use web framework taht can easily render list of tweets and easily show on mobile-friendly device
  - Ex: React Native for all 3 (web, android, ios)
  - Database sql vs no sql
    - **SQL** SQL databases are valuable in handling structured data, or data that has relationships between its variables and entities. Because SQL works with such a strictly predefined schema, it requires organizing and structuring data before starting with the SQL database.
      -  SQL database has a high level of reliability (ACID):
        - Atomicity: All transactions must succeed or fail completely and cannot be left partially complete, even in the case of system failure.
        - Consistency: The database must follow rules that validate and prevent corruption at every step.
        - Isolation: Concurrent transactions cannot affect each other.
        - Durability: Transactions are final, and even system failure cannot “roll back” a complete transaction.
    - **NoSQL** is a non-relational database, meaning it allows different structures than a SQL database (not rows and columns) and more flexibility to use a format that best fits the data. 
      -  different data structures within a database. Because they allow a dynamic schema for unstructured data, there’s less need to pre-plan and pre-organize data, and it’s easier to make modifications. NoSQL databases allow you to add new attributes and fields, as well as use varied syntax across databases.
      - NoSQL databases scale better horizontally, which means one can add additional servers or nodes as needed to increase load.
      - CAP theorm
        - Consistency: Every request receives either the most recent result or an error. MongoDB is an example of a strongly consistent system, whereas others such as Cassandra offer eventual consistency.
        - Availability: Every request has a non-error result.
        - Partition tolerance: Any delays or losses between nodes do not interrupt the system operation.
      - not tables and rows
        -  Column-oriented, where data is stored in cells grouped in a virtually unlimited number of columns rather than rows.
        - Key-value stores, which use an associative array (also known as a dictionary or map) as their data model. This model represents data as a collection of key-value pairs.
        - Document stores, which use documents to hold and encode data in standard formats, including XML, YAML, JSON (JavaScript Object Notation) and BSON. A benefit is that documents within a single database can have different data types.
        - Graph databases, which represent data on a graph that shows how different sets of data relate to each other. Neo4j, RedisGraph (a graph module built into Redis) and OrientDB are examples of graph databases.
5. Identify and Address Difficulties and Trade-offs with Scaling and Performance
  - this shows your experience buliding software that you can predict difficulties before starting
  - slow operations should ideally be done asyncronously to avoid making user wait long time
  - How to handle lots of data
    - **List virtualization**
      - So many elements in the DOM can cause two problems: slow initial rendering and laggy scrolling
      - render to the DOM only what is visible to the user and unload them when they are not by replacing them with new ones
      - Then, when scrolling, the remaining list items render while replacing the items that exit the viewport
      - Libraries: `react-window` and `react-virtualized`
    - **Pagination**
      - split our large dataset into chunks ( or pages ) that we can gradually fetch and display to the user, thus reducing the load on the database
      - **Pagination** works best on websites where users are looking for specific pieces of content, not just browsing content. **Infinite scroll** is better suited for the exploration of content, where users are browsing aimlessly for something interesting. 
      - Pro:
        - can save your spot and go back to it
      - Con:
        - extra action to click
      - Types
        1. **Offset Pagination**
          - `limit` - Number of rows to fetch from the database
          - `offset` - Number of rows to skip. Offset is like a page number, but with a bit of math around it (offset = (page-1) * limit)
          -offset in databases is implemented in a way that loops through rows to know how many should be skipped. That means that the higher our offset is, the longer our database query will take.
        2. **Cursor Pagination**
          - `limit` - Same as before, amount of rows we want to show on one page
          - `cursor` - ID of a reference element in the list. This can be the first item if you're querying the previous page and the last item if querying the next page
          - `cursorDirection` - If user clicked Next or Previous (after or before)
          - solve all issues that offset pagination has — performance, missing data and data duplication
          - Queries that use a `cursor` instead of `offset` are more performant because the WHERE query helps skip unwanted rows, while OFFSET needs to iterate over them, resulting in a full-table scan. Skipping rows using WHERE can get even faster if you set up proper indexes on your IDs. The index is created by default in case of your primary key.
    - **Infinite scrolling**: web-design technique that loads content continuously as the user scrolls down the page, eliminating the need for pagination
      - good for browsing lists and feeds or discovery, without extra clicks/taps
      - not good if you have a call to action at bottom. Endless scrolling is not recommended for goal-oriented finding tasks.
      - API implemented similar to pagination
      - **Pagination** works best on websites where users are looking for specific pieces of content. **Infinite scroll** is better suited for the exploration of content, where users are browsing aimlessly for something interesting. 
      - Cons
        - can’t bookmark their location and come back to it later like pagination
        - lack of footer
        - irrelevant scrollbar
    - **Content Delivery Network (CDN)** - CDN servers may be geographically closer to user, so responses may come back faster
    - **Caching** - going from server to database is very expensive. Must get data first time but you can cache in memory store like Redis to reduce hitting the database 
        - simple key-value pairing and sits btw app and db
        - when app requests info, it 
          1) tries cache first 
          2) if cache doesn't contain that key -> go to db to look
        - u can cache a query, its results or specific object (like rendered version of website or most recent blog post)
        - used cached version  Ex: display page that lists popular posts and comments that are slightly out of date
  - improve user experience (React)
    - **Optimistic UI** is a pattern that you can use to simulate the results of a mutation and update the UI even before receiving a response from the server. 
      - instead of waiting for response from server, we can display that it was updated on UI, and save message in **queue** like SQS. When it is ready to be run, it can trigger a function or lambda to update the item in the database.
    - **lazy loading images** we can wait until each of the images is about to appear in the viewport before we render them in the DOM
      - Similar to the concept of windowing mentioned above, lazy loading images prevents the creation of unnecessary DOM nodes, boosting the performance of our React application.
    - **useCallback** hook to remember function
    - **useMemo** hook to remember calculation based on same input and output
    - **code-splitting** 
      - By default, when a React application renders in a browser, a “bundle” file containing the entire application code loads and serves to users at once. This file generates by merging all the code files needed to make a web application work.
      - bundling is useful because it reduces the number of HTTP requests a page can handle. However, as an application grows, the file sizes increase, thus increasing the bundle file. At a certain point, this continuous file increase slows the initial page load, reducing the user’s satisfaction.
      - React allows us to split a large bundle file into multiple chunks using dynamic import() followed by lazy loading these chunks on-demand using the React.lazy
      ```
        // before
        import Home from "./components/Home";
        import About from "./components/About";
        
        // after
        const Home = React.lazy(() => import("./components/Home"));
        const About = React.lazy(() => import("./components/About"));
      ```
 
  - **Load Balancer**: balances/distribbutes load to different servers so 1 server isn't overloaded and crash
    - frontend can also be thrown behind load balancer 
  - Ex: Some urls infrequently accessed and others can peak (popular form in Reddit) -> don't want it to constantly hit db so use a memory store
  - Types of scaling
    1. **Vertical scaling** - inc resources to specific node
      - Ex: add additional memory to server to handle more load changes
      - usually easier than horizontal scaling but more limited b/c u can onlyl add so much memory or disk space
    2. **Horizontal scaling** - inc # of nodes
      - add additional servers -> dec load on any one server
- **MapReduce**
  - used to process large amounts of data
  - can do processing in parallel
  1. Map - input data, output key, value pair
  2. Reduce - reduces values for each key -> outputs new key and value
7. Iterate on solutions





### <a id="language-frameworks"></a>Language Frameworks
#### Javascript
Eventing system
```
class Events {
  on(eventName, callback) {...}
  trigger(eventName) {...}
  off (eventName) {...}
}
// events: onClick, onHover, onExit
// can add functions to happen on eventName
```