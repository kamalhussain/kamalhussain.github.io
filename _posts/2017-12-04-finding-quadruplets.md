---
layout: post
title:  "Finding quadralupets"
date:   2017-12-04 11:44:00
categories: programming
permalink: /finding-quadruplets/
---

Picking different combinations from an array of options is big part of life. Right? Let's do an interesting programming problem that does the exact same.
You are given with an unsorted array of integers. You need to figure out a set of 4 numbers from the array that will add up to a sum "s". 
There may be different combinations so you need to return the first set from the sorted array.

For these types of problems, there is always a brute-force solution. In this case this solution has the complexity of O(n^4), where is n is the
number of elements in the array. Here is the function which implements a brute-force solution:

```python
def find_array_quadruplet_brute_force(arr, s):
    n = len(arr)
    if n < 4:
        return []

    arr = sorted(arr)
    
    for i, v1 in enumerate(arr[:n - 3]):
        for j, v2 in enumerate(arr[i + 1:n - 2]):
            for k, v3 in enumerate(arr[i + 2 + j:n - 1]):
                for l, v4 in enumerate(arr[i + 3 + k:n]):
                    if (v1 + v2 + v3 + v4) == s:
                        return sorted([v1, v2, v3, v4])

    return []
```

Is there a better way? Yes, you can achieve the complexity of O(n^3) by tweaking the third and fourth loops in this solution. The idea is this.
Pick first two elements and then choose lowest and highest elements of the rest of the array. Adjust the positions of lowest and highest in such a
way that you converge to an answer. Sounds complex? Check out the code and may be it will make more sense.

```python
# input: arr = [ 3, 6, 3, 8, 9, 1, 2], s = 20
# output: [1, 2, 8, 9]
def find_array_quadruplet_better(arr, s):
    n = len(arr)
    if n < 4:
        return []

    arr = sorted(arr)
  
    for i, v1 in enumerate(arr[:n - 3]):

        for j, v2 in enumerate(arr[i + 1:n - 2]):
                low = i + 2 + j
                high = n - 1

                while low < high:
                    if ((arr[low] + arr[high]) > (s - (v1 + v2)) ):
                        high -= 1

                    elif ( (arr[low] + arr[high]) < (s - (v1 + v2)) ):
                        low += 1

                    else:
                        return [v1, v2, arr[low], arr[high]]

    return []
```

You can also use hash tables in the above solution instead of using low and high position parameters. That may be a topic for another post.
