# 435. Non-overlapping Intervals
```PYTHON
def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:

    # sort according the start of interval
    intervals.sort(key = lambda x: x[0])

    result  = 0
    last_end = intervals[0][1]
    for start, end in intervals[1:]:
        if start >= last_end:
            # not overlapping, update last_end
            last_end = end
        else:
            # overlapping
            result += 1
            # update last_end in this way is functionally equal to delete the interval with larger endpoints
            last_end = min(last_end, end)
        
    return result
```

# 763. Partition Labels
```PYTHON
def partitionLabels(self, s: str) -> List[int]:
        # store alphabet as key and the position it appeared as value
        appeared = {}
        for i in range(len(s)):
            if s[i] in appeared.keys():
                appeared[s[i]].append(i)
            else:
                appeared[s[i]] = [i]
        
        # for each alphabet, only store the first index and last index it appeared
        # for alphabet only appeared once, set last index into first index
        intervals = []
        for idx_list in appeared.values():
            if len(idx_list) >= 2:
                intervals.append([idx_list[0],idx_list[-1]])
            elif len(idx_list) == 1:
                intervals.append([idx_list[0],idx_list[0]])
        
        # remove overlapping intervals
        # by append the last index of non-overlapping intervals into results
        result = []
        last_end = intervals[0][1]
        for interval in intervals:
            start = interval[0]
            end = interval[1]

            if start <= last_end:
                last_end = max(last_end, end)
            else: 
                result.append(last_end)
                last_end = end
        result.append(last_end)

        # transform result array from last index of non-overlapping intervals
        # into integers representing the size of the intervals
        for i in range(len(result)-1, -1, -1):
            if i == 0:
                result[i] = result[i] + 1
            else:
                result[i] = result[i] - result[i-1]
        
        return result
```

# 56. Merge Intervals
```PYTHON
def merge(self, intervals: List[List[int]]) -> List[List[int]]:
    intervals.sort(key = lambda x: x[0])

    result = []
    last_start = intervals[0][0]
    last_end = intervals[0][1]
    for start, end in intervals[1:]:
        if start <= last_end:
            last_end = max(last_end, end)
        else:
            result.append([last_start,last_end])
            last_start = start
            last_end = end

    result.append([last_start,last_end])

    return result
```