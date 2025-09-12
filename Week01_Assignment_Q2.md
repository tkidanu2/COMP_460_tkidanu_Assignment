# Problem 33- Rotated Sorted Array Algorithm Solution

## Problem Understanding

Given a sorted array of n distinct numbers that has been rotated k steps (where k is unknown and between 1 and n-1), we need to:
1. Find the unknown integer k (rotation amount)
2. Determine if the array contains a given number x

**Key Properties:**
- Original array A[1..n] was sorted in increasing order
- After k rotations, suffix A[k+1..n] comes first, followed by prefix A[1..k]
- Both parts maintain their sorted order
- Example: [9,13,16,18,19,23,28,31,37,42,1,3,4,5,7,8] has k=10

## Thought Process

### Initial Analysis
The rotated array has a unique property: there's exactly one position where a larger element is followed by a smaller element. This is the "rotation point" where the array was split.

### Key Insights
1. **Rotation Point**: The rotation occurs at position k, so A[k] > A[k+1]
2. **Binary Search Opportunity**: Even though the array isn't fully sorted, we can still use binary search
3. **Two Problems**: Finding k and searching for x are related but can be solved independently

## Part (a): Finding the Rotation Amount k

### Algorithm Strategy
Use binary search to find the rotation point - the position where the "break" in sorting occurs.

```python
def FindRotationAmount(A, n):
    # Handle edge case: array not rotated
    if A[1] <= A[n]:
        return 0
    
    left = 1
    right = n
    
    while left < right:
        mid = (left + right) // 2
        
        if A[mid] > A[mid + 1]:
            return mid
        
        if A[mid] >= A[1]:
            left = mid + 1
        else:
            right = mid
    
    return left - 1
```

## Part (b): Searching for Element x

### Algorithm Strategy
Use modified binary search that accounts for the rotation.

```python
def SearchInRotatedArray(A, n, x):
    left = 1
    right = n
    
    while left <= right:
        mid = (left + right) // 2
        
        if A[mid] == x:
            return mid
        
        if A[left] <= A[mid]:
            if A[left] <= x < A[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:
            if A[mid] < x <= A[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1
```

### Alternative Approach

```python
#Uses Part (a)'s result to first find k, then performs regular binary search
def SearchInRotatedArrayAlternative(A, n, x):
    k = FindRotationAmount(A, n)
    
    if k > 0 and A[1] <= x <= A[k]:
        return BinarySearch(A, 1, k, x)
    
    if A[k+1] <= x <= A[n]:
        return BinarySearch(A, k+1, n, x)
    
    return -1

def BinarySearch(A, left, right, x):
    while left <= right:
        mid = (left + right) // 2
        if A[mid] == x:
            return mid
        elif A[mid] < x:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

## Time Complexity Analysis

### Part (a)
- **Time Complexity**: O(log n)
- **Space Complexity**: O(1)
- Binary search with constant time operations at each level

### Part (b)
- **Time Complexity**: O(log n) for both approaches
- **Space Complexity**: O(1)
- Single binary search pass or two sequential O(log n) operations

## Example Walkthrough

For array [9,13,16,18,19,23,28,31,37,42,1,3,4,5,7,8] with k=10:

1. **Finding k:**
   - Compare A[6]=23 with A[1]=9: 23>9, search right
   - Compare A[9]=37 with A[1]=9: 37>9, search right
   - Compare A[11]=3 with A[1]=9: 3<9, search left
   - Find A[10]=42 > A[11]=1, so k=10

2. **Searching for x=4:**
   - Start with mid=6: A[6]=23, left half sorted
   - Since 4 < 9, search right half
   - Continue until finding 4 at position 13