# 1005. Maximize Sum Of Array After K Negations
### sorting + greedy
```PYTHON
def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        nums.sort()
        minIndex = -1
        for index in range(len(nums)):
            if k == 0:
                break
        
            if nums[index] < 0:
                nums[index] = -nums[index]
                k -= 1
            elif nums[index] == 0:
                k = 0
                break
            else:
                if nums[index] > 0 and index > 0 and nums[index] > nums[index-1]:
                    minIndex = index-1
                else:
                    minIndex = index
                break

        # minIndex == - 1 and k > 0 which means the nums only contain negative integer
        # minIndex != -1 means the minIndex is points to smallest integer in the current nums array
        if ((minIndex == -1 and k > 0) or minIndex != -1)  and k % 2 == 1:
            nums[minIndex] = -nums[minIndex]
        
        return sum(nums)
```
### programmer Carl's version
```PYTHON
def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
    nums.sort(key=lambda x: abs(x))

    # makes k negative integers into positive integers
    for i in range(len(nums)-1, -1, -1):
        if k == 0: 
            return sum(nums)
        if nums[i] < 0:
            nums[i] = -nums[i]
            k -= 1

    if nums[0] == 0:
        # k >= 0 and nums[0] = 0
        return sum(nums)

    elif k % 2 == 1:
        # num[0] != 0 and k != 0 and k is odd number 
        nums[0] = -nums[0]

    return sum(nums)
```

# 134. Gas Station
### my version:
```PYTHON
def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
    if sum(gas) < sum(cost): return - 1

    start = 0
    gas_tank = 0
    for i in range(len(gas)):
        gas_tank += (gas[i] - cost[i])
        if gas_tank < 0:
            # 说明[start, i]区间都不能作为起始位置，因为这个区间选择任何一个位置作为起点，到i这里都会断油
            # 那么起始位置从i+1算起，再从0计算gas_tank
            gas_tank = 0
            start = i + 1

    return start 
```

# 135. Candy
Check the consecutive elements in two pass. For the forward pass, if the rating of a child > left child, increase the number of `distributed_candy[child]` by `1 + distributed_candy[previous child]`. For the backward pass, if the rating of a child > right child, the number of `distributed_candy[child]` is the maximum of its original distributed candy and the number of `distributed_candy[next child] + 1`.
        
Time complexity: O(n)\
Space complexity: O(n)
```PYTHON
def candy(self, ratings: List[int]) -> int:
    
    distributed_candy = [1] * len(ratings)
    
    # forward checking: check if right > left
    for i in range(1, len(ratings)):
        if ratings[i] > ratings[i - 1]:
            distributed_candy[i] = distributed_candy[i-1] + 1
    
    # backward checking: check if left > right
    for i in range(len(ratings)-2 , -1, -1):
        if ratings[i] > ratings[i + 1]:
            distributed_candy[i] = max(distributed_candy[i], distributed_candy[i+1] + 1)

    return sum(distributed_candy) 
```
