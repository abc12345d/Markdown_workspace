# Heap (data structure)
A heap is a specialized Tree-based data structure in which the tree is a complete binary tree that satisfies the heap property. Two heap properties result in two types of the heap: (1) Min-Heap and (2) Max-Heap.

Min-Heap: \
For any given node C, if P is a parent node of C, then the value of P is less than or equal to the value of C.

Max-Heap: \
For any given node C, if P is a parent node of C, then the value of P is greater than or equal to the value of C.

As a heap is a complete binary tree, it is more efficient to implement it in an array instead of linked tree nodes. To map the elements of a heap into an array: if a node is stored at index i, then its left child is stored at index 2i + 1 and its right child at index 2i + 2.

<img width="428" alt="20230214104730" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/ef15eb30-b1e2-404b-ba97-e57100644c90">

### Operations of Heap
#### Insertion in Heap
When we insert an element, it always goes in the next empty spot(looking top to bottom and left to right). We first insert the element, then bubble it up until we get to the right spot.
```
Suppose the Heap is a Max-Heap as:
      10
    /    \
   5      3
  / \
 2   4

The new element to be inserted is 15.

Process:
Step 1: Insert the new element at the end.
      10
    /    \
   5      3
  / \    /
 2   4  15

Step 2: Heapify the new element following bottom-up 
        approach.
-> 15 is more than its parent 3, swap them.
       10
    /    \
   5      15
  / \    /
 2   4  3

-> 15 is again more than its parent 10, swap them.
       15
    /    \
   5      10
  / \    /
 2   4  3

Therefore, the final heap after insertion is:
       15
    /    \
   5      10
  / \    /
 2   4  3
```

#### Deletion in Heap
We remove the root node of the heap, and swap the last added element to the root. As the last added elememt might not be the right spot, so we take the root element and bubble it down to the next layer by comparing with its children node 

```
Suppose the Heap is a Max-Heap as:
      10
    /    \
   5      3
  / \
 2   4

The element to be deleted is root, i.e. 10.

Process:
The last element is 4.

Step 1: Replace the last element with root, and delete it.
      4
    /    \
   5      3
  / 
 2   

Step 2: Heapify root.
Final Heap:
      5
    /    \
   4      3
  / 
 2
```

# Heap sort
The time complexity of using heap in sorting is O(n log n). Both `heappush` and `heappop` operations need O(log n). Unlike other sorting algorithm, heap sort is not a stable sorting algorithm.
```PYTHON
def heapsort(iterable):
    h = []
    for value in iterable:
        heappush(h, value)
    return [heappop(h) for i in range(len(h))]
```
In Python, Heap elements can be tuples. This is useful for assigning comparison values (such as task priorities) alongside the main record being tracked:
```PYTHON
h = []
>>> heappush(h, (5, 'write code'))
>>> heappush(h, (1, 'write spec'))
>>> heappush(h, (3, 'create tests'))
>>> heappop(h)
(1, 'write spec')
```
# Others
- Priority queue is the abstract description of this concrete data structure.
- To implement min-heap in Python, use `heapq` module. There are two ways to implement max-heap in Python:\
(a) invert the value of the keys and use heapq. For example, turn 1000.0 into -1000.0 and 5.0 into -5.0.\
(b) use undocumented funtions of `heapq` module
```PYTHON
import heapq
listForTree = [1,2,3,4,5,6,7,8]    
heapq.heapify(listForTree)             # for a min heap
heapq._heapify_max(listForTree)        # undocumented function for a maxheap!!

# If you then want to pop elements, use:
heapq.heappop(minheap)      # pop from minheap
heapq._heappop_max(maxheap) # undocumented pop from maxheap
```

# Reference
[Wikipedia - heap (data structure)](https://en.wikipedia.org/wiki/Heap_(data_structure))\
[StackOverFlow - What do I use for a max-heap implementation in Python?](https://stackoverflow.com/questions/2501457/what-do-i-use-for-a-max-heap-implementation-in-python)\
[Python documentation - heapq module ](https://docs.python.org/3/library/heapq.html)\
[Geeksforgeeks - Insertion and Deletion in Heaps](https://www.geeksforgeeks.org/insertion-and-deletion-in-heaps/)
