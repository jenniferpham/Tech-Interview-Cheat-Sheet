# Tech Interview Cheat Sheet

# Table of Content
- [Big O Notation](#o-notation)
- [Data Structures](#data-structures)
  - [String](#string)
  - [Array](#array)
  - [Hash Table or Hash Map](#hash-map)
  - [Linked List](#linked-list)
  - [Stack](#stack)
  - [Queue](#queue)
    - [Priority Queue](#priority-queue)
  - [Tree](#tree)
    - [Binary Tree](#binary-tree)
    - [Breadth First Search](#breadth-first-search-tree)
    - [Depth First Search](#depth-first-search-tree)
    - [BFS vs DFS](#bfs-vs-dfs)
    - [Binary Heaps (min-heap and max-heap)](#binary-heap)
    - [Tries (Prefix Tries)](#prefix-tries)
  - [Graph](#graphs)
    - [BFS vs DFS](#bfs-vs-dfs-graph)
    - [Breadth First Search](#bfs-graph)
    - [Depth First Search](#dfs-graph)
- [Algorithms](#algorithms)
  - [Techniques](#algorithm-techniques)
    - [Divide and Conquer](#divide-conquer)
    - [Sliding Window](#sliding-window)
    - [Multiple Pointers](#multiple-pointers)
  - [Recursion](#recursive-algorithms)
  - [Iterative](#iterative-algorithms)
  - [Dynamic Programming](#dynamic-programming)
  - [Math Algorithms](#math-algorithms)
    - [Permutations vs Combinations](#permutation-combination)
  - [Search Algorithms](#search-algorithms)
    - [Linear Search](#linear-search)
    - [Binary Search](#binary-search)
  - [Maze/Matrix Algorithms](#maze-matrix-algos)
  - [Sorting Algorithms](#sorting-algorithms)
    - [Quadratic Sorting](#quadratic-sorting-algorithms)
      - [Bubble Sort](#bubble-sort)
      - [Selection Sort](#selection-sort)
      - [Insertion Sort](#insertion-sort)
    - [Faster/Complex Sorting Algorithms](#faster-sorting-algorithms)
      - [Merge Sort](#merge-sort)
      - [Quick Sort](#quick-sort)
      - [Radix Sort](#radix-sort)
- [System Design and Scalability](#system-design)
  - [Frontend](#frontend)
    - [How to handle lots of data on Front-end](#data-frontend)
    - [Saving state](#save-state)
  - [Scalability](#scalability)
  - [Database](#database)
    - [Database Types](#database-types)
      - [SQL (relational) Databases](#sql-db)
      - [NoSQL (non-relational) Databases](#nosql-db)
    - [Database Partitioning (Sharding)](#db-partitioning)
- [Additional Resources](#additional-resources)


# <a id="o-notation"></a>Big O Notation
### Definition:

Visualized below; the x-axis representing input size and the y-axis representing complexity:

![#](https://upload.wikimedia.org/wikipedia/commons/thumb/7/7e/Comparison_computational_complexity.svg/400px-Comparison_computational_complexity.svg.png)

[(source: Wikipedia, _Computational Complexity of Mathematical Operations_)](https://en.wikipedia.org/wiki/Computational_complexity_of_mathematical_operations)

#### Big-O notation
Big-O refers to the upper bound of time or space complexity of an algorithm, meaning it worst case runtime scenario. An easy way to think of it is that runtime could be better than Big-O but it will never be worse.

#### What you need to know
- Worst Case means given an unideal input, Average Case means given a typical input, Best case means a ideal input. Ex. Worst case means given an input the algorithm performs particularly bad, or best case an already sorted array for a sorting algorithm.
- Best Case and Big Omega are generally not helpful since Best Cases are rare in the real world and lower bound might be very different than an upper bound.
- Big-O isn't everything. On paper merge sort is faster than quick sort, but in practice quick sort is superior.

#### Rates of growth from best to worst:
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
    const midIndex = Math.floor((s.length - 1)/2);
    for (let i = 0; i < midIndex; i++) {
        // swap far left and far right using ES6 destructuring assignment
        [s[i], s[s.length - 1 - i]] = [s[s.length - 1 - i], s[i]];  
    }
    return s;
};

// ES6 destructuring assignment example as a way to swap in place
// [arr[0], arr[1]]  = [arr[1], arr[0]]
// input: [1,2,3,4] -> output: [2,1,3,4] 
```
- [**Valid Parentheses**](https://leetcode.com/problems/valid-parentheses/)
Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if: Open brackets must be closed by the same type of brackets. Open brackets must be closed in the correct order.
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

### <a id="hash-map"></a> Hash Table or Hash Map
#### Definition
- Stores data with key value pairs.
- **Hash functions** accept a key and return an output unique only to that specific key.
  - This is known as **hashing**, which is the concept that an input and an output have a one-to-one correspondence to map information.
  - Hash functions return a unique address in memory for that data.

#### What you need to know
- Designed to optimize searching, insertion, and deletion.
- If hashing two or more keys returns the same value, this is called a **hash collision** (returns same output for 2 distinct inputs).
  - All hash functions have this problem.
  - This is often accommodated for by having the hash tables be very large.  
  - To mitigate this, instead of storing actual values, the underlying array may hold references to linked lists that, in turn, contain all values with the same hash.
- A **set** is a variation of a hash table that stores keys but not values.
- Hashes are important for associative arrays and database indexing.
- Average search, insertion and deletion are O(1). Fast!
- Cons: No ordering means looking up minimum and maximum values is expensive. 0(n)

#### Time Complexity
- Indexing:         Hash Tables: `O(1)`
- Search:           Hash Tables: `O(1)`
- Insertion:        Hash Tables: `O(1)`

#### Uses
- cache
- database partitioning

#### Class
```
class Hashmap {
  constructor() {
    this._storage = [];
  }

  hashStr(str) {
    let finalHash = 0;
    for (let i = 0; i < str.length; i++) {
      const charCode = str.charCodeAt(i);
      finalHash += charCode;
    }
    return finalHash;
  }

  set(key, val) {
    let idx = this.hashStr(key);

    if (!this._storage[idx]) {
      this._storage[idx] = [];
    }

    this._storage[idx].push([key, val]);
  }

  get(key) {
    let idx = this.hashStr(key);

    if (!this._storage[idx]) {
      return undefined;
    }

    for (let keyVal of this._storage[idx]) {
      if (keyVal[0] === key) {
        return keyVal[1];
      }
    }
  }
}

var dict = new Hashmap();
dict.set("james", "123-456-7890");
dict.set("jess", "213-559-6840");
console.log(dict.get("james"));
```


### <a id="linked-list"></a> Linked List
#### Definition
- Stores data with **nodes** that point to other nodes.
- The first node is called the **head**.
- The last node is called the **tail**.
- **Single-linked list** vs **double-linked list**
  - Nodes on single-linked list have `.val` and `.next`, while double-linked lists have `.val`, `.next` and `.prev` property
  - DLL is better than SLL for finding nodes (done in half the time because you can start at the tail instead of head). DLL allows for traversing a list backwards.
  - DLL takes up more memory considering extra pointer

#### What you need to know
- Designed to optimize insertion and deletion, slow at indexing and searching.
- excellent alternative to arrays when insertion and deletion at beginning are frequenely required (array insertion at beginning requires all indices after to re-index, which is inefficient) (Arrays contain built-in index whereas linked lists don't) 
- Stacks (SLL) and Queues (DLL) are commonly implemented with Linked Lists, less often using arrays
- **Circularly linked list** is simple linked list whose **tail**, the last node, references the **head**, the first node.

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
- **Intersecting linked lists**
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

### <a id="queue"></a> Queues (FIFO)
#### What you need to know
- Made with a doubly linked list - removes from head (dequeue) and adds to tail (enqueue)
- FIFO - First In First Out
- fast insertion and deletion = 0(1)
- searching and access = 0(n) -> if you need to search or access within the queue, use a different structure
- don't really want to implement queue as an array because inserting and removing the first item requires re-indexing all items afterwards, which is 0(n) vs 0(1) with queue or linked list
- To **enqueue**, insert at the tail of the linked list.
- To **dequeue**, remove at the head of the linked list.
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
    this.queue = [];
  }

  enqueue(item) {
    return this.queue.unshift(item);
  }

  dequeue() {
    return this.queue.pop();
  }

  peek() {
    return this.queue[this.length - 1];
  }

  get length() {
    return this.queue.length;
  }

  isEmpty() {
    return this.queue.length === 0;
  }
}

const queue = new Queue();
queue.enqueue(1);
queue.enqueue(2);
console.log(queue);

queue.dequeue();
console.log(queue);
```

#### <a id="priority-queue"></a> Priority Queue (FIFO)

##### Class
```
// same as queue class except enqueue is different

let queue = [];
// element type ['Jennifer', 1] // 0 index is value and 1 index is the priority
// [['Jennifer', 1], ['Bryan', 2], ['Serena', 3]]
function enqueuePQ(element) {
  if(queue.length === 0) {
    queue.push(element);  
  } else {
    let added = false;
    for (let i = 0; i < queue.length; i++) {
      const priorityIndex = element[1];

      if(priorityIndex < queue[i][1]) {  // compare priorities smallest to largest
        queue.splice(i, 0, element);  // add element sorted by priority and mutate original array
        added = true;
        break;
      }
      if(!added) {
        queue.push(element)
      }
    }
  }
}
```

#### Algorithms
- **Min Meeting Rooms**(https://leetcode.com/problems/meeting-rooms-ii/)
Given an array of meeting time intervals intervals where intervals[i] = [starti, endi], return the minimum number of conference rooms required.
```
function minMeetingRooms(intervals) {
    if (intervals.length === 0) return 0;
    if (intervals.length === 1) return 1;
  
    intervals.sort((a, b) => a[0] - b[0])
  
    let rooms = []
    
    rooms.push(intervals[0][1])
    for(let i = 1; i < intervals.length; i++) {
        let addedToRoom = false
        for(let x = 0; x < rooms.length; x++) {
            if(rooms[x] <= intervals[i][0])  {
                rooms[x] = intervals[i][1]
                addedToRoom = true
                break
            }
        }
        if(!addedToRoom) rooms.push(intervals[i][1])
        
    }
    return rooms.length
};
```


### <a id="stack"></a> Stacks (LIFO)
#### What you need to know
- LIFO = Last In First Out
- Made with a linked list by having the head be the only place for insertion (push) and removal (pop).
- fast insertion and removal = 0(1)
- searching and access = 0(n) -> if you need to search or access within the stack, use a different structure
- LIFO - last in first out - like stack of dishes
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
class Stack {
  constructor() {
    this.stack = [];
  }

  push(item) {
    return this.stack.push(item);
  }

  pop() {
    return this.stack.pop();
  }

  peek() {
    return this.stack[this.length - 1];
  }

  get length() {
    return this.stack.length;
  }

  isEmpty() {
    return this.length === 0;
  }
}

const newStack = new Stack();
newStack.push(1);
newStack.push(2);
console.log(newStack);

newStack.pop();
console.log(newStack);
```


### <a id="tree"></a> Tree
#### Definition
- data structure that consists of nodes in parent/child relationship (if siblings are connected, then that's a **graph**)
  - tree is a type of graph, but not the other way around. Between any two nodes, there is only one possible path.
  lists-linear vs trees - nonlinear
- each tree has a root node
- **Leaf node** - no children
- **Edge** - connection btw 1 node to another
- The number of links from the root to a node is called that node's **depth**.
- The **height** of a tree is the number of links from its root to the furthest leaf.
- Pros:For most operations, the average time complexity is O(log(n)), which enables solid scalability. Worst time complexity varies between O(log(n)) and O(n).
- Cons: Performance degrades as trees lose balance, and re-balancing requires effort. No constant time operations: trees are moderately fast at everything they do.
- 2k+1 be the right child and 2k+2 the left child if represented in array

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
function TreeNode(value) {
  this.value = value;
  this.children = [];
  this.parent = null;

  this.setParentNode = function (node) {
    this.parent = node;
  };

  this.getParentNode = function () {
    return this.parent;
  };

  this.addNode = function (node) {
    node.setParentNode(this);
    this.children[this.children.length] = node;
  };

  this.getChildren = function () {
    return this.children;
  };

  this.removeChildren = function () {
    this.children = [];
  };
}

const root = new TreeNode(1);
root.addNode(new TreeNode(2));
root.addNode(new TreeNode(3));

const children = root.getChildren();
for (let i = 0; i < children.length; i++) {
  for (let j = 0; j < 5; j++) {
    children[i].addNode(new TreeNode("second level child " + j));
  }
}

console.log(root);
children[0].removeChildren();
console.log(root);
console.log(root.getParentNode());
console.log(children[1].getParentNode());
```

### <a id="binary-tree"></a> Binary Tree
- **Binary Tree**: tree-like data structure where every node has up to 2 child nodes (1 left and 1 right child node)
- has 1 root
- has exactly 1 path between root  and any node
- different from ternary tree and n-ary trees
- <a id="binary-search-tree"></a>**Binary search trees**
  - 1 type of binary tree where all left descendants have a value sm
  - All left descendants < parent node
  - all right descendants > parent node
  - Clarify with interviewer
    - are there duplicate values? usually not allowed
    - if there are child nodes with same value as parent node, does it belong on left or right side?
- **Balanced Tree**: balanced enough to ensure 0(log n) times for insertion and search/find
- **Complete binary tree**: tree in which every level is filled from left to right (except lowest level) but must fill left before right
- **Full binary tree**: every node has 0 or 2 children
- **Perfect binary tree**: both full and complete, each level has max # of nodes (exactly (2^k - 1) nodes)
- In an **unbalanced binary tree**, there is a significant difference in height between subtrees.
- An completely one-sided tree is called a **degenerate tree** and becomes equivalent to a linked list.
- **Tips for algorithms**
  - Does order matter? Breadth-first (use queue and while loop) or depth-first search (use stack and while loop or recursive which uses call stack)?
  - Should we include nulls in queue or stack? (if question is about structure of tree, u usually need it. If question is about values of nodes, you can usually disregar null nodes)
  - for recursive algorithms, think if there were no nodes and only 1 node, what should it return. those are your base cases.
  - question about paths usually implies DFS

#### Time Complexity
- Search:    Binary Search Tree: `O(log n)` on average, but `O(n)` for unbalanced trees like SLL
- Insertion: Binary Search Tree: `O(log n)`, but `O(n)` for unbalanced trees like SLL
- if you double the nodes, you increase 1 extra step

#### Class
```
function Node(val) {
  this.val = val;
  this.left = null;
  this.right = null;
}

class BST {
  constructor(val) {
    this.root = new Node(val);
  }

  add(val) {
    let newNode = new Node(val);

    function findPosAndInsert(currNode, newNode) {
      if (newNode.val < currNode.val) {
        if (!currNode.left) {
          currNode.left = newNode;
        } else {
          findPosAndInsert(currNode.left, newNode);
        }
      } else {
        if (!currNode.right) {
          currNode.right = newNode;
        } else {
          findPosAndInsert(currNode.right, newNode);
        }
      }
    }

    if (!this.root) {
      this.root = newNode;
    } else {
      findPosAndInsert(this.root, newNode);
    }
  }

  remove(val) {
    let self = this;
    let removeNode = function (node, val) {
      if (!node) {
        return null;
      }
      if (val === node.val) {
        if (!node.left && !node.right) {
          return null;
        }
        if (!node.left) {
          return node.right;
        }
        if (!node.right) {
          return node.left;
        }
        let temp = self.getMinimum(node.right);
        node.val = temp;
        node.right = removeNode(node.right, temp);
        return node;
      } else if (val < node.val) {
        node.left = removeNode(node.left, val);
        return node;
      } else {
        node.right = removeNode(node.right, val);
        return node;
      }
    };
    this.root = removeNode(this.root, val);
  }

  getMinimum(node) {
    if (!node) {
      node = this.root;
    }
    while (node.left) {
      node = node.left;
    }
    return node.val;
  }

  // helper method
  contains(value) {
    let doesContain = false;

    function traverse(bst) {
      if (this.root.value === value) {
        doesContain = true;
      } else if (this.root.left !== undefined && value < this.root.value) {
        traverse(this.root.left);
      } else if (this.root.right !== undefined && value > this.root.value) {
        traverse(this.root.right);
      }
    }

    traverse(this);
    return doesContain;
  }
}

const bst = new BST(4);
bst.add(3);
bst.add(5);
bst.remove(3);
console.log(bst);
```


#### Algorithms
- **Contains Value**
```
// iterative
function treeIncludes(root, target) {
  if(root === null) return false;
  const stack = [root];
  while(stack.length > 0) {
    const current = stack.pop();
    if(current.val === target) return true;
    if(current.left) stack.push(current.left);  // don't want to push nulls
    if(current.right) stack.push(current.right);
  }
  return false;
}

// recursive
function treeIncludes(root, target) {
  if(root === null) return false;  //edge case
  if(root.val === target) return true; // positive base case
  return treeIncludes(root.left) || treeIncludes(root.right);
}
```

- **Sum of Tree Values**
```
// iterative
function sumTree(root) {
  if(root === null) return 0;
  let sum = 0;
  let stack = [root];
  while(stack.length > 0) {
    const current = stack.pop();
    sum += current.val;
    if(current.left) stack.push(current.left);  // only push if child nodes are not null
    if(current.right) stack.push(current.right);
  }
  return sum;
}

// recursive
function sumTree(root) {
  if(root === null) return 0;
  return root.val + sumTree(root.left) + sumTree(root.right);
}
```

- **Get Min Value in Tree**
```
// iterative
function treeMinValue(root) {
  if(root === null) return Infinity;
  let min = Infinity;
  let stack = [root];
  while(stack.length > 0) {
    let current = stack.pop();
    min = Math.min(min, current.val);

    if(current.left) stack.push(current.left);
    if(current.right) stack.push(current.right);
  }
  return min;
}

// recursive
function treeMinValue(root) {
  if(root === null) return Infinity;

  const leftMinValue = treeMinValue(root.left);
  const rightMinValue = treeMinValue(root.right);

  return Math.min(root.val, leftMinValue, rightMinValue);
}
```

- **Max Root to Leaf Sum**
```
function maxPathSum(root) {
  if(root === null) return -Infinity;  // base case and so you can call properties .left and .right later
  if(root.left === null && root.right === null) return root.val;  // positive base case- this is leaf node
  const maxChildPathSum = Math.max(maxPathSum(root.left), maxPathSum(root.right));
  return root.val + maxChildPathSum;
}
```

- [**Sum of Root to Leaf Path**](https://leetcode.com/problems/path-sum/)
Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.
```
// recursive
var hasPathSum = function(root, targetSum) {
    if(root === null) return false;
    // if it's a leaf node, then return it's value
    
    // subtract root value from target sum
    targetSum -= root.val;
    
    // base case positive
     if(!root.left && !root.right) {  // leaf node
        return targetSum === 0;  // returns true/false once it reaches leaf node
     }
    
    const leftPathSum = hasPathSum(root.left, targetSum)
    const rightPathSum = hasPathSum(root.right, targetSum)
    
    return leftPathSum || rightPathSum
}

// iterative
var hasPathSum = function(root, targetSum) {  
    if(root === null) return false;
    
    // keep track of tree nodes
    // start off with targetSum minus root.val. Keep subtracting as you traverse down to the leaf node.
    // stack has these objects to keep track of diff { node: root, diff: # }
    let stack = [{node: root, diff: targetSum - root.val}];
    
    while(stack.length > 0) {
        const current = stack.pop();
        const diff = current.diff;
        const currentNode = current.node;
        
          // if child node, break
        // OR if target sum is hit, then break? but can remaining numbers be 0?
        if(!currentNode.left && !currentNode.right && diff === 0) { // leaf node
            return true;
        } else if (!currentNode.left && !currentNode.right && diff !== 0) {
            continue; //  find other path but break out of this current loop
        }
        
        if(currentNode.left) stack.push({node: currentNode.left, diff: current.diff - currentNode.left.val});
        if(currentNode.right) stack.push({node: currentNode.right, diff: current.diff - currentNode.right.val});
        
    }
    
    return false;
};
```

- **Is Valid Binary Search Tree**
A valid BST is defined as follows:
The left subtree of a node contains only nodes with keys less than the node’s key.
The right subtree of a node contains only nodes with keys greater than the node’s key.
Both the left and right subtrees must also be binary search trees.
```
// Iterative soln
var isValidBST = function(root) {
    // use inorder traversal template
    let prevVal = -Infinity;
    let stack = [];
    
    while (root !== null || stack.length !== 0) {
        while (root !== null) {
            stack.push(root);
            root = root.left;
        }
        root = stack.pop();
        if (root.val <= prevVal) return false;
        prevVal = root.val;
        root = root.right;
    }
    return true;
};

// Recursive soln
const isValidBST = function(root) {
     
    function helper(root, min = null, max = null){
         if (!root){
             return true
         }
        if ((min !== null && root.val <= min) || (max !== null && root.val >= max)){
            return false
        }
        return helper(root.left, min, root.val) && helper(root.right, root.val, max)
    }

    return helper(root, null, null)
};
```
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

### <a id="breadth-first-search-tree"></a>Breadth First Search
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

### <a id="depth-first-search-tree"></a>Depth First Search
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
function inOrderDFSIterative(root) {
  if(root === null) return []; // edge case

  let stack = [root];
  let result = [];

  while (stack.length > 0) {
    const current = stack.pop();

    if(current.left) stack.push(current.left);
    visited.push(current.val)
    if(current.right) stack.push(current.right);
  }
  return result;
}
```
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

- LowestCommonAncestor of 2 nodes
```
function lowestCommonAncestor(root, p, q) {
  if(!root || (root === p) || (root === q) ) return root;
  const left = lowestCommonAncestor(root.left, p, q);
  const right = lowestCommonAncestor(root.right, p, q);
  if(!left) return right;
  if(!right) return left;
  return root;
}
```

### <a id="bfs-vs-dfs"></a>Breadth First Search vs Depth First Search
- The simple answer to this question is that it depends on the size and shape of the tree.
  - BFS grows the tree level by level, whereas DFS grows it sub-tree by sub-tree. 
  - If our tree is broad/wide, use DFS as BFS will take too much memory. Similarly, if our tree is very deep, choose BFS over DFS.
  - **BFS** tends to be a queue and while loop algorithm, while **DFS** tends to be a stack and while loop or recursive algorithm.


### <a id="binary-heap"></a> Binary Heaps (Min Heaps and Max Heaps)
[min and max heap](./assets/min-max-heap.png)
#### Definition
- heap is a category of trees
- **Completely Binary Tree** filled at every level, except possibly the last level, from left to right (not necessarily all right have 2 child nodes yet)
- no relationship for siblings
- **Min Heap** - each parent node < children -> root = min element
- **Max Heap** - each parent node > children -> root = max element
- heap is not natively represented in JS
- **Represent Binary Heap as array**
  - n = index
  - `left child = 2n + 1`  (Ex: if parent index = 0 -> left child is index 1 and right child is index 2)
  - `right child = 2n + 2`
  - `parent = Math.floor((n-1)/2)`  (Ex: if child is index 2 -> parent is index 0)
- primarily used for getting the minimum or the maximum value present in a heap in `O(1)` time. The linear data structures like Arrays or LinkedList can get you this value in `O(n)` time while non-linear data structures like Binary Search Trees(BST) can get you this value in `O(log n)` time where n is the number of elements.

#### Time Complexity
- Access the min/max value: `O(1)` time
- Search:    `O(log n)`
- Insertion and Removing: `O(log n)`
  - start by adding node to right-most node (or end of the array) and then swap (bubble up) to swap with parents to its place
- if you double the nodes, you increase 1 extra step
- worst case scenario is if the binary tree is not balanced (like SLL) -> `0(n)` for search

#### Class
```
class MinHeap {

    constructor () {
        this.heap = []
    }

    getMin () {
        /* Accessing the min element at index 1 in the heap array */
        return this.heap[0]
    }
    
    insert (node) {

        /* Inserting the new node at the end of the heap array */
        this.heap.push(node)

        /* Finding the correct position for the new node */

        if (this.heap.length > 0) {
            let current = this.heap.length - 1; // set current to last index

            /* Traversing up the parent node until the current node (current) is smaller than the parent (current/2)*/
                                    // child node                // its parent node
            while (current > 1 && (this.heap[current] < this.heap[Math.floor(current/2)])) {

                /* Swapping the two nodes (parent and child) by using the ES6 destructuring syntax*/
                [this.heap[Math.floor(current/2)], this.heap[current]] = [this.heap[current], this.heap[Math.floor(current/2)]]
                current = Math.floor(current/2)  // set current to its parent
            }
        }
    }
    
    remove() {
        /* Smallest element is at the index 1 in the heap array */
        let smallest = this.heap[1]

        /* When there are more than two elements in the array, we put the right most element at the first position
            and start comparing nodes with the child nodes
        */
        if (this.heap.length > 2) {
            this.heap[1] = this.heap[this.heap.length-1]
            this.heap.splice(this.heap.length - 1)

            if (this.heap.length === 3) {
                if (this.heap[1] > this.heap[2]) {
                    [this.heap[1], this.heap[2]] = [this.heap[2], this.heap[1]]
                }
                return smallest
            }

            let current = 1
            let leftChildIndex = current * 2
            let rightChildIndex = current * 2 + 1

            while (this.heap[leftChildIndex] &&
                    this.heap[rightChildIndex] &&
                    (this.heap[current] > this.heap[leftChildIndex] ||
                        this.heap[current] > this.heap[rightChildIndex])) {
                if (this.heap[leftChildIndex] < this.heap[rightChildIndex]) {
                    [this.heap[current], this.heap[leftChildIndex]] = [this.heap[leftChildIndex], this.heap[current]]
                    current = leftChildIndex
                } else {
                    [this.heap[current], this.heap[rightChildIndex]] = [this.heap[rightChildIndex], this.heap[current]]
                    current = rightChildIndex
                }

                leftChildIndex = current * 2
                rightChildIndex = current * 2 + 1
            }
        }

        /* If there are only two elements in the array, we directly splice out the first element */

        else if (this.heap.length === 2) {
            this.heap.splice(1, 1)
        } else {
            return null
        }

        return smallest
    }
}
```

### <a id="prefix-tries"></a>Trie(Prefix Trees)
[Trie data structure starts with empty root node](./assets/trie-data-structure.png)

#### Definition
- tree-like data structure that efficiently stores and retrieves keys in a dataset of strings
- Each trie has an empty root node, with links (or references) to other nodes — one for each possible alphabetic value. The shape and the structure of a trie is always a set of linked nodes, connecting back to an empty root node.
-  nodes of the tree store the entire alphabet and strings/words can be retrieved by traversing down a branch path.
- stores characters at each node
- each path represents a word
- Terminating Trie Node means word is complete
- Uses: auto-complete and spellcheker

#### Time Complexity
- same run time as hash table 0(k) or O(1)
  - k = character length
  - Find is` 0(p + n)` 
    - p = length of prefix
    - n = # of words
  - Insert, Search, Delete is `O(a*n)`
    - a = length of word being searched/inserted/deleted
    - n = # of total words
  - Space complexity is `0(n*k)`
    - n = # of total words
    - k = # of references each node is storing (26 for the English alphabet)

#### Class
```
var Trie = function() {
    this.root = {};
};

/**
 * Inserts a word into the trie. 
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function(word) {  //dog -> this.root = {'d': {'o': 'g': {}}}
    let node = this.root;
    for(let letter of word) {
        if (node[letter] === undefined) node[letter] = {};
        node = node[letter]
    }
    node.isEnd = true;
};

/**
 * Returns if the word is in the trie. 
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function(word) {
    let node = this.root
    for(let letter of word) {
        // check if current letter is in the node
        if(!node[letter]) {
            return false;
        } else {
            node = node[letter];
        }
    }

    return node && node.isEnd === true;
};

/**
 * Returns if there is any word in the trie that starts with the given prefix. 
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function(prefix) {
    let node = this.root;
    for(let letter of prefix) {
        if(!node[letter]) {
            return false;
        } else {
            node = node[letter];
        }
    }
    return true;
};
```

### <a id="graphs"></a>Graphs
#### Definition
- Each graph element is called a **node**, or **vertex**.
- Graph nodes are connected by **edges** (bridges that connect them)
- can be **directed** (1-way) or **undirected** (2-ways).
- can be **cyclic** or **acyclic**. A **cyclic graph** contains a path from at least one node back to itself. **Acyclic graph** is a graph without cycles.
- can be weighted or unweighted. In a weighted graph, each edge has a certain numerical weight.
- can be traversed (see Trees)
- **Connected graph**: graph between all vertices
- Data structures used to represent graphs:
  - **Edge list**: a list of all graph edges represented by pairs of nodes that these edges connect.
  - **Adjacency list**: a list or hash table where a key represents a node and its value represents the list of this node's neighbors.
  - **Adjacency matrix**: a matrix of binary values indicating whether any two nodes are connected.

#### Uses
- Facebook friends
- Recommendations in ecommerce websites.
- Mapping services

#### Class
```
class Graph {
  constructor() {
    this.adjacencyList = {};
  }

  addVertex(vertex) {
    if(!this.adjacencyList[vertex]) {
      this.adjacencyList[vertex] = [];
    }
  }

  addEdge(vertex1, vertex2) {
    this.adjacencyList[vertex1].push(vertex2);
    this.adjacencyList[vertex2].push(vertex1);
  }

  removeVertex(vertex) {
    // remove vertex adn all edges (remove instances of that vertex in other arrays)
    // loop thorugh only props that have that key
    while(this.adjacencyList[vertex].length) {
      const adjVertex = this.adjacencyList[vertex].pop();
      this.removeEdge(adjVertex, vertex)
    }
    delete this.adjacencyList[vertex];
  }

  removeEdge(vertex) {
    // cut relationship so vertex1 arr doesn't contain vertex2 and vice versa
    this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter((edge) => edge !== vertex2);  // remove vertex2 from vertex1 arr
    this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter((edge) => edge !== vertex1); // remove vertex1 from vertex2 arr
  }
}
```

  
#### <a id="bfs-vs-dfs-graph"></a>Breadth First Search vs Depth First Search
- when implementing graph search, check if each node has been visited because graphs have multiple ways to get to the same place whereas trees only have one way
  - **BFS** better for finding shortest path, while **DFS** better if solutions are farther from the path
  - **DFS** are often used in simulations of games (and game-like situations in the real world). In a typical game you can choose one of several possible actions. Each choice leads to further choices, each of which leads to further choices, and so on into an ever-expanding tree-shaped graph of possibilities.

#### <a id="bfs-graph"></a>Breadth-First Search
- start at root and go to all neighbors of root before going to their neighbors
- use queue and store visited
- better for finding shortest path between nodes
- **Time complexity of BFS**: `O(V + E)` when **Adjacency List** is used and `O(V^2)` when **Adjacency Matrix** is used (V = vertices; E = edges)
- higher **space complexity** than DFS b/c you use a queue

```
// method inside of graph class
// remember: BBQ - breadth and queue
breadthFirstSearch(startVertex) {
  let queue = [startVertex];
  let visited = {startVertex: true};
  let orderedResults = [];
  let currentVertex;
  while (queue.length > 0) {
    currentVertex = queue.shift();  // remove first item from queue
    orderedResults.push(currentVertex); // add first item from queue into orderedResults
    this.adjacencyList[currentVertex].forEach((neighbor) -> {
      if(!visited[neighbor]) {
        queue.push(neighbor);
        visited[neighbor] = true;
      } 
    })
    return orderedResults;
  }
}
```
- template for finding a node in a graph with BFS
```
function bfs(target) {
  const queue = [[0, 0]]; // 1. Initialize queue with Node and current distance 0
  const visited = new Set(0); // 2. Initialize set

  for (const [current, distance] of queue) { // 3. Loop through queue until it's empty
    if (current === target) return distance; // 4. Check dequeued is solution
    for (const [neighbor, currDist] of getNeighbors(node)) { // 5. Get next possible moves (neighbor nodes)
      if (visited.has(neighbor) continue; // 6. Skip visited nodes
      visited.add(neighbor); // 7. Mark next node as visited.
      queue.push([neighbor, currDist + 1]); // 8. Add neighbor to queue and increase the distance.
    }
  }

  return -1; // 9. If you didn't find the answer, return something like -1/null/undefined.
}

function getNeighbors(node) {
  // TODO: implement based on the problem.
}
```

##### Algorithms
- **Bidirectional Search**: find shortest path between source and destination nodes
```
run 2 simultaneous breadth-first searches, 1 from each node -> when their searches collide -> found path
```
- **Maze Path**
You have a ball at a starting point, that can roll up, down, left and right. However, the ball won’t stop rolling until it hits a wall. Your task is to check if there’s a path from start to destination. You may assume that the borders of the maze are all walls. The maze is represented in a grid (2d array):
  - Walls are represented as 1.
  - Empty spaces are 0.
```
Example input:
start = [ 0, 4 ]
end = [ 4, 4 ]
maze = [
  [ 0, 0, 1, 0, 0 ],
  [ 0, 0, 0, 0, 0 ],
  [ 0, 0, 0, 1, 0 ],
  [ 1, 1, 0, 1, 1 ],
  [ 0, 0, 0, 0, 0 ]
]
```
```
function hasPath(maze, start, destination) {
    // looking for shortest way to destination and then cut -> use BFS
    // empty spaces = 0
    // walls = 1

    let queue = [[start, 0]];  // [[[0,4], 0]] // [start, distance] inside of queue
    const visited = new Set([start.join()]); // [04]
    const directions = [[0,1], [0,-1], [1,0], [-1,0]]  // first index is row, second index is col -> right, left, top, bottom (like CSS)
    
    // loop through items in queue
    for (let [[row, col], distance] of queue) {
        if(row === destination[0] && col === destination[1] && maze[row] && maze[row][col] === 0) { //found destination AND that spot is an empty space (not a wall)
            return true;
        }
        
          // loop through neighboring directions
        for (let [rowDirection, colDirection] of directions) {
            // set coordinates to current to reset them
            let newRow = row;
            let newCol = col;
            while(maze[newRow + rowDirection] && maze[newRow + rowDirection][newCol + colDirection] === 0) { // check that row exists first so we're not calling a property of undefined AND check that it is an empty wall (0), not a wall (1)
                newRow += rowDirection;
                newCol += colDirection;
            }
            if(visited.has([newRow, newCol].join())){
                continue; // skip if we visited this cell already
            } else {
                // add to visited
                visited.add([newRow, newCol].join())
                queue.push([[newRow, newCol], distance + 1])
            }
        }
    }
  
    return false; // if you loop through the entire maze and you don't find the destination, then return false;
};
```


#### <a id="dfs-graph"></a>Depth-First Search 
- start at root and explore each branch completely before moving onto next branch
- preferred if we want to visit every node
- recursive or use stack
- better if solutions are farther from the path or explore all possible paths
- - **Time complexity of DFS**: `O(V + E)` when **Adjacency List** is used and `O(V^2)` when **Adjacency Matrix** is used (V = vertices; E = edges)
- has lesser **space complexity** than BFS, because at a time it needs to store only single path from the root to leaf node.

```
depthFirstSearchIterative(startVertex) {
  let stack = [startVertex];
  let visited = {startVertex: true};
  let results = [];
  let currentVertex;
  while (stack.length > 0) {
    currentVertex = stack.pop();
    results.push(currentVertex);
    this.adjacencyList[currentVertex].forEach((neighbor) => {
      if(!visited[neighbor]) {
        visited[neighbor] = true;
        stack.push(neighbor);
      }
    })
  }
  return results;
}

---

depthFirstSearchRecursive(startVertex) {
  let result = [];
  let visited = {};
  const adjacencyList = this.adjacencyList;  // for scope within inner function

  function dfs(vertex) {
    if(!vertex) return null;
    visited[vertex] = true;
    result.push(vertex);
    adjacencyList[vertex].forEach((neighbor) => {
      if(!visited[neighbor]){
        return dfs(neighbor)
      }
    })
  } 

  dfs(startVertex);
  return results;
}
```


# <a id="algorithms"></a> Algorithms
- **All Paths From Source to Target**
Given a directed acyclic graph (DAG) of n nodes labeled from 0 to n - 1, find all possible paths from node 0 to node n - 1 and return them in any order.The graph is given as follows: graph[i] is a list of all nodes you can visit from node i (i.e., there is a directed edge from node i to node graph[i][j]).
```
var allPathsSourceTarget = function(graph) {
    let currentNode = 0;
    let currentPath = [];
    let searchedElement = graph.length-1;
    let paths = [];
    
    findPath(graph, currentNode, currentPath, searchedElement, paths);
    
    return paths;
};


var findPath = function(graph, currentNode, currentPath, searchedElement, paths) {    
    currentPath.push(currentNode);

    if(currentNode === searchedElement) {
        paths.push(currentPath.slice());
        return;
    }    
    
    for(let node of graph[currentNode]) {
        findPath(graph, node, currentPath, searchedElement, paths);
        currentPath.pop();
    }
}
```

### <a id="algorithm-techniques"></a>Techniques
#### <a id="divide-conquer"></a> **Divide and Conquer**
- good for when input is sorted
- instead of looping from far left to far right, divide up the data set
- Ex: binary search - time complexity is 0(log n), instead of 0(n) looping through entire data set from far left to far right
```
binarySearch([1,2,3,4,5,6], 11)

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
#### <a id="sliding-window"></a> **Sliding Window**
- good for consecutive string or consecutive array elements
```
maxSubarraySum([2,6,9,2,1,8,5,6,3], 3)

function maxSubarraySum(arr, num) {
  let maxSum = 0;
  let tempSum = 0;

  // edge case: array must be greater than num
  if(arr.length < num) {  
    return null;
  }

  for (let i = 0; i < num; i++) {
    maxSum += arr[i];  // 2 + 6 + 9 = 17;
  }
  tempSum = maxSum;
  for (let i = num; i < arr.length; i++) {
    tempSum = tempSum - arr[i - num] + arr[i];
    maxSum = Math.max(maxSum, tempSum);
  }
  return maxSum;
}
```

#### <a id="multiple-pointers"></a> **Multiple Pointers**
- good for solving problems with sorted input and minimal space complexity
- creating pointers or values that correspond to index or position and move towards beg, middle, end based on certain conditions
```
sumZero([-3, -2, -1, -, 1, 2, 3])  // [-3, 3]
sumZero([-2, 0, 1, 3])  // undefined
sumZero([1, 2, 3])  // undefined

function sumZero(arr) {
  let left = 0;
  let right = arr.length - 1;
  while (left < right) {
    let sum = arr[left] + arr[right];
    if(sum === 0) {
      return [arr[left], arr[right]];
    } else if (sum > 0) {
      right--;
    } else {
      left++;
    }
  }
  return undefined;
}
```

## <a id="math-algorithms"></a> Math Algorithms
- **Reverse integer**
```
// x is an int
function reverse(x) {
  let result = 0;
  while(x !== 0) {
    result = (result * 10) + (x % 10);  // (x % 10) to get last digit
    x = x/10;
  }
  const integerBoundary = Math.pow(2, 31);
  if ((result < (integerBoundary * -1)) || (result > integerBoundary)) {
    return 0;
  } else {
    return result;
  }
}
```

### <a id="permutation-combination">Permutations vs Combinations</a>
- **Permutations** are for lists (order matters)
  - formula: `P = n! / (n-k)!`
  - You have n items and want to find # ways that k items can be ordered.
  - You have 52 cards and want to find # possible ways to have a 5 card hand.
- **Combinations** are for groups (order doesn't matter)
  - formula: `C = n! / (k! * (n-k)!)`
  - Find all the ways to pick k people from n and divide by the k! variants.
  - Ex: Picking team of 3 from a group of 10
  - Ex: Choosing 3 desserts from menu of 10


### <a id="recursive-algorithms"></a>Recursive Algorithms
#### Definition
- A method that calls itself
  - **Base case** - the stopping condition so that it stops calling itself
  - **Recursive case** some small subproblem that calls itself. What is the smallest amount of work I can do in each iteration?
#### What you need to know
- **Stack level too deep** and **stack overflow**.
  - If you've seen either of these from a recursive algorithm, you messed up.
  - It means that your base case was never triggered or the input was so massive you ran out of alloted memory.
  - Knowing whether or not you will reach a base case is integral to correctly using recursion.
- Often used in Depth First Search
- elegant solution for complex problems
- can take up too much space
- works well with JSON objects, trees, graphs - focus on 1 tiny unit at a time

### <a id="iterative-algorithms"></a>Iterative Algorithms
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


## <a id="dynamic-programming"></a> Dynamic Programming
- type of algorithm that works on the principle of recursion where each problem is broken into smaller sub-problems and the solution of the final problem is dependent on the solutions of the smaller ones
- stores the solutions of each sub-problem and then using these states later to simplify complexities and reduce computation time.
```
// regular fib
def fib(n):
	if n <= 0:
		raise Exception('Number must be greater than or equal to 1')
	elif n == 1:
		return 0
	elif n == 2:
		return 1
	else:
		return fib(n-1) + fib(n-2)

// dynamic fib - stores solutions
fib_array=[0, 1]

def fib(n):
	if n <= 0:
		raise Exception('Number must be greater than or equal to 0')
	elif n <= len(fib_array):
		return fib_array[n - 1]
	else:
		temp = fib(n - 1) + fib(n - 2)
		fib_array.append(temp)
		return temp
```

### <a id="greedy-algorithms"></a>Greedy Algorithms
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

## <a id="maze-matrix-algos"></a>Maze/Matrix Algorithms
- **DFS**
  - follow 1 path until we hit a dead end -> push intersections to **stack**
  - if we hit a dead end, we will backtrack to previous intersection and follow a different path
  - stop when we find exit or entire map has been visited
- **BFS**
  - enqueue every interesection in a **queue**
  - visit all paths at same time 1 step at a time 
  - stop when we find exit or until entire map has been visited

## <a id="sorting-algorithms"></a>Sorting Algorithms
### <a id="quadratic-sorting-algorithms"></a>Quadratic Sorting Algorithms
| Sorting Type    | Best Time | Avg Time | Worst Time | Space |
| --------------- | --------- | -------- | ---------- | ----- |
| Bubble Sort     | 0(n)      | 0(n^2)   | 0(n^2)     | O(1)  |
| Insertion Sort  | 0(n)      | 0(n^2)   | 0(n^2)     | O(1)  |
| Selection Sort  | 0(n)      | 0(n^2)   | 0(n^2)     | O(1)  |

- Bubble and Insertion sort perform well with nearly sorted data
- all these 3 sorts have same avg and worst and best time and space complexity
- all 3 of these are good for smaller sets of data

#### <a id="bubble-sort"></a>Bubble Sort
- sort and swap adjacent elements

#### <a id="selection-sort"></a>Selection Sort
- A comparison based sorting algorithm.
  - Starts with the cursor on the left, iterating left to right
  - Compares the left side to the right, looking for the smallest known item
    - If the left is smaller than the item to the right it continues iterating
    - If the left is bigger than the item to the right, the item on the right becomes the known smallest number
    - Once it has checked all items, it moves the known smallest to the cursor and advances the cursor to the right and starts over
  - As the algorithm processes the data set, it builds a fully sorted left side of the data until the entire data set is sorted
- Changes the array in place.
- linear scan and swap smallest element to front. scan again and second smallest element is swapped second to front.

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

#### <a id="insertion-sort"></a>Insertion Sort
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


### <a id="faster-sorting-algorithms"></a>Faster/Complex Sorting Algorithms
| Sorting Type    | Best Time   | Avg Time    | Worst Time  | Space       |
| --------------- | ----------- | ----------- | ----------- | ----------- |
| MergeSort       | 0(n log n)  | 0(n log n)  |  0(n log n) | O(n)        |
| QuickSort       | 0(n log n)  | 0(n log n)  |  0(n^2)     | O(log n)    |
| RadixSort       | 0(nk)       | 0(nk)       |  0(nk)      | O(n + k)    |
- k = # of digits

- Bubble and Insertion sort perform well with nearly sorted data
- all these 3 sorts have same avg and worst and best time and space complexity
- all 3 of these are good for smaller sets of data


### <a id="merge-sort"></a>Merge Sort
#### Definition
- A divide and conquer algorithm.
1. Split the given list into two halves (roughly equal halves in case of a list with an odd number of elements).
2. Continue dividing the subarrays in the same manner until you are left with only single element arrays.
3. Starting with the single element arrays, merge the subarrays so that each merged subarray is sorted.
4. Repeat step 3 until we end up with a single sorted array.

#### What you need to know
- Know that it divides all the data into as small possible sets then compares them.
- Merge Sort is one of the fastest sorting algorithms out there.
- Unlike Quick Sort, Merge Sort is not an in-place sorting algorithm, meaning it takes extra space other than the input array. This is because we are using auxiliary (helper) arrays to store the sub-arrays. The space complexity of the merge sort is O(n).
- Another advantage of Merge Sort is that it lends itself very well to multi-threading, since each respective half and be sorted on its own. Another common way of reducing the runtime of Merge Sort is to stop when we get to relatively small subarrays (~7) and using Insertion Sort to sort them. This is done because Insertion Sort performs really well on small, or nearly sorted arrays. Much better than it's more globally efficient counterparts.

#### Time Complexity
- Worst Case: `O(n log n)`
- Average Case: `O(n log n)`
- Best Case: `O(n)`
#### Space Complexity
- Space Complexity - Worst Case: `O(1)`

####  Algorithm
```
function mergeSort(arr) {
  // No need to sort the array if the array only has one element or empty
  if(arr.length <= 1) return arr; // base condition

  // divide array into left half and right half
  const middleIndex = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, middleIndex)); // merge left half, not including middleIndex
  const right = mergeSort(arr.slice(middleIndex)); // merge right half, from middleIndex to end

  // Using recursion to combine the left and right
  return merge(left, right);
}

// helper function
// Merge the two arrays: left and right
function merge (left, right) {
  let resultArray = [], leftIndex = 0, rightIndex = 0;

  // We will concatenate values into the resultArray in order
  while (leftIndex < left.length && rightIndex < right.length) {
    if (left[leftIndex] < right[rightIndex]) {
      resultArray.push(left[leftIndex]);
      leftIndex++; // move left array cursor
    } else {
      resultArray.push(right[rightIndex]);
      rightIndex++; // move right array cursor
    }
  }

            // Concatenating the leftover elements
    // (in case we didn't go through the entire left or right array. There will be at least 1 elem from euther left or right array.
    return [ ...resultArray, ...left, ...right ]
}
```

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
- Best + Avg Case: `O(n log n)`

#### Space Complexity
- Worst Case: `O(log n)`

#### Algorithm
```
function quickSort(arr, left = 0, right = arr.length - 1) {
  let index = partition (arr, left, right);
  if (left < index - 1) { // sort left half
    quickSort(arr, left, index - 1)
  })
  if(index < right) { // sort right half
    quickSOrt(arr, index, right);
  }
}

//helper function
function partition(arr, left, right) {
  const pivot = arr[(left + right)/2]; // picking middle as pivot
  while (left <= right) {
    while(arr[left] < pivot) left++; // find elem on left that should go to right
    while(arr[right] > pivot) right++; // find elem on right that should go to left
    // swap elements and move left/right pointers
    if(left <= right) {
        swap(arr, left, right);
        left++;
        right--;
    }
  }
  return left;
}
```

#### Visualization
![#](https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif)

[(source: Wikipedia, _Quicksort_)](https://en.wikipedia.org/wiki/Quicksort)

#### Merge Sort Vs. Quicksort
- Quicksort is likely faster in practice, but merge sort is faster on paper.
- Merge Sort divides the set into the smallest possible groups immediately then reconstructs the incrementally as it sorts the groupings.
- Quicksort continually partitions the data set by a pivot, until the set is recursively sorted.


# <a id="system-design"></a>System Design and Scalability

## <a id="scalability">Scalability</a>
- Consider:
  - Read-heavy - try caching
  - Write-heavy - queue up writes
- ![Web Architecture](./assets/web-application-architecture-overview.png?raw=true "Web Application Architecture")
- Types:
  1. **Vertical Scaling** - buy bigger machines (easier but limited)
    - no load balancer
    - single point of failure
    - inter-process communication fast
    - consistent data (in only 1 machine)
    - hardware limit
  2. **Horizontal Scaling** - buy more machines
    - load balancing required
    - resilient
    - slow network calls
    - data inconsistent across multiple machines
    - scales well as user increases

## <a id="frontend">Frontend</a>

### <a id="save-state">Save State</a>
- save data on server side if possible
  - 1 source of truth and consistent across all sessions and devices
- client side
  - useContext React API
  - local component state like loading or error state for fetching data
  - Web storage
    - local storage or session storage
  - state management libraries: React Context API, Redux, Recoil
  - **Idempotent** -  you can safely retry it with the same idempotency key. same input produces same output.
  - Stripe API implements idempotency keys on mutating endpoints (POST) by allowing clients to pass a unique value in with the special **Idempotency-Key** header, so if request fails due to a network connection error, you can safely retry it with the same idempotency key, and the customer is charged only once.
  - In REST API, GET and PUT and DELETE w/ same data are idempotent, not POST b/c given same input, it gives back same output
  - guarantees that it is safe to do over and over again
  - When performing a request, a client generates a unique ID to identify that operation to the server along with the normal payload. The server receives the ID and correlates it with the state of the request on its end. If the client notices a failure, it retries the request with the same ID, and that ID guarantees that it's only done once.

### <a id="data-frontend">How to handle lots of data on Front-end</a>
  - **List virtualization**
    - So many elements in the DOM can cause two problems: slow initial rendering and laggy scrolling
    - render to the DOM only what is visible to the user and unload them when they are not visible by replacing them with new ones
    - when scrolling, the new list items render when they are in view and replace the old items that exit the viewport
    - Libraries: `react-window` and `react-virtualized`
  - **Pagination**
    - split our large dataset into chunks ( or pages ) that we can gradually fetch and display to the user, thus reducing the load on the database
    - works best on websites where users are looking for specific pieces of content, not just browsing content. 
    - **Infinite scroll** is better suited for the exploration of content, where users are browsing aimlessly for something interesting, not good for goal-oriented tasks
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
  - **Content Delivery Network (CDN)** - CDN servers may be geographically closer to user, so cached content loaded using that server closest to the end user may load responses faster
    - cache static files like images, CSS, JavaScript, videos, PDF (but can also serve dynamic content if needed).
  - **Caching** - going from server to database is very expensive. Must get data first time but you can cache in memory store like Redis to reduce hitting the database 
      - simple key-value pairing and sits btw app and db
      - when app requests info, it 
        1) tries cache first 
        2) if cache doesn't contain that key -> go to db to look
      - u can cache a query, its results or specific object (like rendered version of website or most recent blog post)
      - used cached version  Ex: display page that lists popular posts and comments that are slightly out of date
      - Try to achieve a higher** cache hit ratio **(how many times you can reuse the same cached response);
  - **Load Balancer**: balances/distributes load to different servers so 1 server isn't overloaded and crash
    - frontend can also be thrown behind load balancer 
    - Types of load balancers:
      -  A hosted service (e.g. Elastic Load Balancer by Amazon);
      - A self-managed software-based load balancer (e.g. Nginx);
      - A hardware load balancer.


### Improve user experience in React
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

## <a id="database">Database</a>
### <a id="database-types">Database Types</a> 
  - **SQL Databases (Relational Database)(RDBMS) vs NoSQL Databases (Non-relational database)**
    - SQL databases are table based databases whereas NoSQL databases can be document based, key-value pairs, graph databases.
    - SQL databases are vertically scalable while NoSQL databases are horizontally scalable.
    - SQL databases have a predefined schema whereas NoSQL databases use dynamic schema for unstructured data.
#### <a id="sql-db"></a>SQL (Relational) Database
- SQL databases are valuable in handling structured data, or data that has relationships between its variables and entities. Because SQL works with such a strictly predefined schema, it requires organizing and structuring data before starting with the SQL database.
  - Relational databases like MySQL Database, Oracle, Ms SQL Server, Sybase, etc. use SQL 
  - should be used when data validity is super important
  - SQL database has a high level of reliability - **ACID Compliance**:
    - **ACID (atomicity, consistency, isolation, durability)** is a set of properties of database transactions intended to guarantee data validity despite errors, power failures, and other mishaps. 
    - For example, a transfer of funds from one bank account to another, even involving multiple changes such as debiting one account and crediting another, is a single transaction.
    - **Atomicity**: All transactions must succeed or fail completely and cannot be left partially complete, even in the case of system failure. If 1 part fails, it will roll back to previou state.
    - **Consistency**: The database must follow rules that validate and prevent corruption at every step.
    - **Isolation**: Concurrent transactions cannot affect each other.
    - **Durability**: Transactions are final, and even system failure cannot “roll back” a complete transaction.
#### <a id="nosql-db"></a>NoSQL(non-relational) Database 
- **NoSQL** is a non-relational database, meaning it allows different structures than a SQL database (not rows and columns) and more flexibility to use a format that best fits the data. 
  - does not require a fixed schema, avoids joins, and is easy to scale
  - use when it’s more important to have fast data than correct data and you need to scale based on changing requirements
  -  different data structures within a database. Because they allow a dynamic schema for unstructured data, there’s less need to pre-plan and pre-organize data, and it’s easier to make modifications. NoSQL databases allow you to add new attributes and fields, as well as use varied syntax across databases.
  - NoSQL databases scale better horizontally, which means one can add additional servers or nodes as needed to increase load.
  - **CAP theorm**: it is impossible to build a distributed system that would simultaneously guarantee Consistency, Availability, and Partition tolerance. only 2 of 3 attributes can be met at a time. 
    - Consistency: The same data becomes visible to all of the nodes at the same time. Every request receives either the most recent result or an error. MongoDB is an example of a strongly consistent system, whereas others such as Cassandra offer eventual consistency.
    - Availability: Every request has a non-error or valid response.
    - Partition tolerance: Any delays or losses between nodes do not interrupt the system operation.
  - not tables and rows
    -  Column-oriented, where data is stored in cells grouped in a virtually unlimited number of columns rather than rows.
    - Key-value stores, which use an associative array (also known as a dictionary or map) as their data model. This model represents data as a collection of key-value pairs.
    - Document stores, which use documents to hold and encode data in standard formats, including XML, YAML, JSON (JavaScript Object Notation) and BSON. A benefit is that documents within a single database can have different data types.
    - Graph databases, which represent data on a graph that shows how different sets of data relate to each other. Neo4j, RedisGraph (a graph module built into Redis) and OrientDB are examples of graph databases.

### <a id="db-partitioning"></a>Database Partitioning (Sharding)
- splitting data across multiple machines while ensuring you have way of figuring out which data is on which machine
- Types:
  1. **Vertical Partitioning**: partitioning by feature
    - Ex: building social network -> 1 partition for tables related to profiles, 1 for messages, etc.
    - drawback: if 1 of these tables gets very large, you will need to re-partition
  2. **Key-Based (or Hash-Based) Partitioning**: use some part of data (Ex: ID) to partition modules
    - Ex: allocate N servers and put data on mod (key)
    - drawback: # of servers is fixed. If you add more servers -> re-allocate all the data (very expensive)
  3. **Directory-based Partitioning**: maintain lookup data where data can be found
    - easy to add additional servers
    - drawbacks: Lookup table is single point of failure and constantly accessing this table impacts performance
- most architectures use multiple partitioning schemas

## Examples:
- Design TinyURL or bit.ly
- Youtube/Netflix
- Quora/Reddit/HackerNews (social network or message board forum system)
- FB Messenger or Whatsapp
- Dropbox/Google Drive/ Google Photos
- FB, Twitter, Instagram
- Web crawler
Uber
- Rate limiter

Ex: Twitter
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

5. Identify and Address Difficulties and Trade-offs with Scaling and Performance
  - this shows your experience buliding software that you can predict difficulties before starting
  - slow operations should ideally be done asyncronously to avoid making user wait long time
 
 
 

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







## <a id="additional-resources"></a>Additional Resources
[Khan Academy's Algorithm Course](https://www.khanacademy.org/computing/computer-science/algorithms)
[Javascript interview questions](https://www.toptal.com/javascript/interview-questions)
[Data Structures AlgoDaily](https://algodaily.com/lessons/an-executable-data-structures-cheat-sheet)
[Software Architecture](https://medium.com/@olgamitroshyna/software-architecture-i-wish-i-had-known-about-this-earlier-4df43eae57db)