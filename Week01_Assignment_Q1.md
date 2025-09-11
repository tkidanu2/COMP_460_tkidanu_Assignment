# Problem - 16 Weighted Median Algorithm Solution

## Problem Understanding
Given a set S of n items with values S[1..n] and weights W[1..n], we need to find the weighted median in O(n) time.


## Thought Process

### Initial Analysis
The key insight is that this is similar to finding a regular median, but instead of counting elements, we're counting weights. The weighted median is the element that "balances" the total weight. In this case, We can use a randomized selection algorithm (like QuickSelect) but modify it to work with cumulative weights instead of just counting elements.

### Algorithm Strategy
1. Use a divide-and-conquer approach similar to QuickSelect
2. Instead of partitioning by count, partition by cumulative weight
3. Recursively search the appropriate partition based on weight distribution

## Algorithm

### Main Function
```python
def WeightedMedian(S, W, n):
    return WeightedSelect(S, W, 1, n, TotalWeight(W, n)/2)
```

### Selection Function
```python
def WeightedSelect(S, W, left, right, target_weight):
    if left == right:
        return S[left]
    
    # Choose random pivot
    pivot_index = Random(left, right)
    pivot_value = S[pivot_index]
    
    # Partition around pivot
    partition_index = WeightedPartition(S, W, left, right, pivot_value)
    
    # Calculate cumulative weights
    left_weight = 0
    for i in range(left, partition_index):
        left_weight += W[i]
    
    pivot_weight = W[partition_index]
    
    # Decide which partition to recurse into
    if target_weight <= left_weight:
        return WeightedSelect(S, W, left, partition_index - 1, target_weight)
    elif target_weight <= left_weight + pivot_weight:
        return S[partition_index]
    else:
        return WeightedSelect(S, W, partition_index + 1, right, 
                            target_weight - left_weight - pivot_weight)
```

### Partition Function
```python
def WeightedPartition(S, W, left, right, pivot_value):
    # Find pivot element
    for i in range(left, right + 1):
        if S[i] == pivot_value:
            S[i], S[right] = S[right], S[i]
            W[i], W[right] = W[right], W[i]
            break
    
    # Partition elements by value
    store_index = left
    for i in range(left, right):
        if S[i] < pivot_value:
            S[i], S[store_index] = S[store_index], S[i]
            W[i], W[store_index] = W[store_index], W[i]
            store_index += 1
    
    # Place pivot in correct position
    S[store_index], S[right] = S[right], S[store_index]
    W[store_index], W[right] = W[right], W[store_index]
    return store_index
```

### Helper Function
```python
def TotalWeight(W, n):
    return
```

## Time Complexity Analysis

### Average Case
- **Time Complexity**: O(n)
- **Explanation**: Similar to QuickSelect, each recursive call processes approximately half of the remaining elements

### Worst Case
- **Time Complexity**: O(n²)
- **Explanation**: Though rare with random pivot selection, this can occur with consistently poor pivot choices

### Space Complexity
- **Complexity**: O(log n)
- **Explanation**: Required for the recursion stack in the average case