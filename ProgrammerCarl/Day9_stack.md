# 232. Implement Queue using Stacks
In order to implement queue by using only stack, we need at least two stacks which responsible for input and output respectively.

```PYTHON
class MyQueue:

    def __init__(self):
        self.in_stack = []
        self.out_stack = []

    def push(self, x: int) -> None:
        self.in_stack.append(x)

    def pop(self) -> int:
        if self.out_stack:
            return self.out_stack.pop()
        else:
            # move all the elements from in_stack to out_stack
            # so that we can get the first-in element
            while (self.in_stack):
                self.out_stack.append(self.in_stack.pop())
            return self.out_stack.pop()

    def peek(self) -> int:
        val = self.pop()
        self.out_stack.append(val)
        return val

    def empty(self) -> bool:
        return not(self.in_stack or self.out_stack) 
```

# 225. Implement Stack using Queues
```PYTHON
from collections import deque
class MyStack:

    def __init__(self):
        self.queue = deque()

    def push(self, x: int) -> None:
        self.queue.append(x)
        
    def pop(self) -> int:
        # move all the elements except the last element into 
        # a temporarily deque, so that we can get the last-in element
        temp = deque()
        while(len(self.queue) > 1):
            temp.append(self.queue.popleft())

        val = self.queue.popleft()
        # reassign the temporarily deque to self.queue
        # after storing the value for output
        self.queue = temp

        return val

    def top(self) -> int:
        temp = deque()
        while(len(self.queue) > 1):
            temp.append(self.queue.popleft())

        val = self.queue.popleft()
        # add the last-in element back to the temporarily deque
        # after storing the value for output
        temp.append(val)
        self.queue = temp

        return val

    def empty(self) -> bool:
        return not (self.queue)     
```