# Problem 33- Rotated Sorted Array Algorithm Solution

## Problem Understanding

Given a sorted array of n distinct numbers that has been rotated k steps (where k is unknown and between 1 and n-1), we need to:
1. Find the unknown integer k (rotation amount)
2. Determine if the array contains a given number x

## Problem Components

### Part (a): Finding k
- This part solves the first requirement: finding the rotation amount (k) in the array
- It uses binary search to find where the array "breaks" its sorting
- Returns the position k where A[k] > A[k+1]

### Part (b): Searching for x
- This part solves the second requirement: finding if a given number x exists in the array
- It provides two different approaches to solve this part:
  1. Direct approach (`SearchInRotatedArray`): Modified binary search that handles rotation
  2. Alternative approach (`SearchInRotatedArrayAlternative`): Uses Part (a)'s result to first find k, then performs regular binary search

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