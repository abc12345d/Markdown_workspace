# 203. Remove Linked List Elements
To delete a node in a linked list, we need both the previous node and the next node of the desired node (see figure). However, a linked list's head node does not have a previous node. Therefore, how we handle the deletion of a head node will differ from how we handle the deletion of a non-head node. 

<img width="725" alt="20230204124443" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/6529f3ee-ca6b-474f-9fcf-c3c5b07adfa8">

There are two ways to address this situation:\
(1) Write two separate while loops, one for handling the head node and another one for handling the non-head nodes. 
(2) Add a dummy head node so that the real head node becomes like any other node.

### way(1): two separate while-loops for handling the head node and non-head nodes differently

```PYTHON
def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        
        while(head and head.val == val):            
            head = head.next                        # delete head node
        
        if (not head):                              
            return                                 
            
        prev = head
        while(prev.next):                           
            if(prev.next.val == val):
                prev.next = prev.next.next          # delete non-head node
            else:
                prev = prev.next                    # move prev forward 

        return head
```

### way(2): add a dummy head node

```PYTHON
def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy_head = ListNode(val = -1, next = head)
        
        prev = dummy_head
        while(prev.next):
            if (prev.next.val == val):
                prev.next = prev.next.next
            else:
                prev = prev.next
            
        return dummy_head.next
```

# 707. Design Linked List
To simplify the insertion and deletion in linked list, the `head` of the MyLinkedList class is set to point to a dummy head node. Besides, most of the methods of MyLinkedList class will receive index as input, so adding a `size` variable will make the implementation easier.

```PYTHON
    def __init__(self):
        # intialise with a dummy head node
        self.head = ListNode(val=-1)
        self.size = 0
```

The while loop in these methods (`get`, `addAtTail`, `addAtIndex`, `deleteAtIndex`) are used to locate desired node. Since insertion and deletion in linked list require both the previous node and the next node of the desired node, hence functions involve these two operations should start from dummy head node (see figure). Moreover, we must update the `self.size` for methods involving insertion and deletion in linked list.

<img width="742" alt="20230205190351" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/69ae6a48-84bb-4f57-a1a8-ecf51c6a6961">

```PYTHON
def get(self, index: int) -> int:
        if(index >= self.size or index < 0):
            return -1
        
        curr = self.head.next
        while(index > 0):
            curr = curr.next
            index -=1
        
        return curr.val

def addAtHead(self, val: int) -> None:
    temp = self.head.next
    self.head.next = ListNode(val = val, next = temp)
    self.size += 1

def addAtTail(self, val: int) -> None:
    prev = self.head
    while(prev.next):
        prev = prev.next
        
    prev.next = ListNode(val = val)
    self.size += 1

def addAtIndex(self, index: int, val: int) -> None:
    if (index > self.size):
        return

    prev = self.head
    while (index > 0):
        prev = prev.next
        index -= 1

    temp = prev.next
    prev.next = ListNode(val = val, next = temp)
    self.size += 1


def deleteAtIndex(self, index: int) -> None:
    if (index < 0 or index >= self.size):
        return
    
    prev = self.head
    while (index > 0):
        prev = prev.next
        index -= 1
    
    prev.next = prev.next.next
    self.size -= 1
```

# 206. Reverse Linked List

<img width="766" alt="20230205173548" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/f600bdd6-06aa-4760-aa5c-40bd503dfed5">

### way(1) iterative approach

```PYTHON
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
    
    temp = None
    pre = None
    curr = head
    while(curr):
        temp = curr.next     # store unprocessed linked list
        curr.next = pre      # reverse linked list 
        pre = curr           # store processed linked list
        curr = temp          # prepare for next iteration (by setting curr as first node of unprocessed linked list)
    
    return pre
```

### way(2) recursive approach

```PYTHON
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:   
    def reverse(curr,pre):
        if (not curr):
            return pre

        unprocessed = curr.next            
        curr.next = pre

        return reverse(unprocessed,curr)

    return reverse(head,None)
```

# Reference
[代码随想录 - 链表](https://programmercarl.com/链表理论基础.html#单链表)
