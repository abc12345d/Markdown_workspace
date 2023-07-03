# Stack
Stack is a data structure in which the addition or removal of an element follows a particular order, i.e. LIFO (Last-In/First-Out) and FILO (First-In/Last-Out). We can implement a stack by using an array, deque, or singly linked list as long as it fulfils the interface of the stack. 

Interface of Stack:\
(1) push\
(2) pop\
(3) top
![](./images/20230209224253.png) 
Unlike an array, random access is not allowed for the objects contained in the stack.

# Queue
Queue is a data structure in which addition or removal of element only follows a particular order i.e. FIFO (First-In/First-Out).

Interface of Stack:\
(1) push\
(2) pop\
(3) peek: Returns the element at the front of the queue.
![](./images/20230209224253.png) 
Unlike an array, random access is not allowed for the objects contained in the stack.

# Monotonic queue/stack
A queue/stack where the elements are either increasing or decreasing from one side to the other side.

The interface of monotonic queue/stack is like a regular queue/stack with one key difference in the push operation: Before we push a new element `e` onto the queue/stack, we first check if adding `e` breaks the monotonic condition. If it does, then we pop the last element/top element off the queue/stack until the insertion of the new element `e` no longer breaks the monotonic condition.

### Applications of Monotonic Stack/Deque in leetcode questions
By definition, the elements in monotonic queue/stack are always sorted in a specific order, either in ascending or descending order. Hence, we can use this property to solve problems such as:

(1) next larger (or smaller member) for each element in a list: \
Maintained a monotonic decreasing queue (or monotonic increasing queue), new element that will break the monotonic condition will be the next larger (or smaller member) for the first element in the queue.

(2) maximum or minimum element in a sliding window: \
Access the maximum/minimum element by looking at the start of the monotonic queue. This means that for a sliding window, we can easily keep track of its maximum/minimum element. When we insert an item from one side, we perform the monotonic insertion method on the deque, and when we remove an item from the other side, we simply pop the item on the other side if the index matches.



# Reference
[代码随想录 - 栈](https://programmercarl.com/%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)\
[algomonster - Monotonic Stack/Deque Intro](https://algo.monster/problems/mono_stack_intro)
