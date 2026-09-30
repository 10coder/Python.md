
### Array Sorting
arr.sort() -> inline sorting
sorted(arr) -> returns sorted list without updating arr

arr.sort(reverse=True) -> inline reverse sorting
sorted(arr, reverse=True)

lets say we have negative values [-1, 7, -6, 4] sorts => [-6, -1, 4, 7]
but if we want to sort with absolute values(not considering signs like negative vals) like [-1, 4, -6, 7]?

sorted(arr, key = lambda x: abs(x))

```
lambda   x   :   abs(x)
  ↑      ↑        ↑
"define  input   what to
a func"  param    return
```
equivalent to 
```
def get_abs(x):
    return abs(x)
```

here key tells how they need to be transformed before sorting -> key is a built-in parameter of the sorted() function that dictates how the items in your list should be compared and ordered.

sorted() doesn't know or care what your objects are — it only knows how to compare the numbers/strings your key function hands it.

for each element, it calls the key function to get a "sort-by" value, and sorts based on that.

Question: how does this return with original values although the values transformed internally with key function paramter?
Ans: key is a temporary transformation function that updates values as per passed function and holds a map of original and transformed values after sorting the original values are retrieved for updated value and passed back.

<details>
  <summary>Additional internal working</summary>
Internally, Python does something conceptually like this (called the Schwartzian transform — decorate, sort, undecorate):

```
def sorted_manual(arr, key):
    # Step 1: decorate — pair each element with its key
    decorated = [(key(x), x) for x in arr]
    # decorated = [(5, -5), (3, 3), (1, -1), (8, 8), (2, -2)]

    # Step 2: sort — compare only the first item of each pair (the key)
    decorated.sort()  # sorts by key value, using x as tiebreaker

    # Step 3: undecorate — throw away the key, keep original elements
    return [x for (k, x) in decorated]
```

Why this is better than comparing directly

Without key, you'd need a comparator — a function that takes two elements and says which comes first (this is what old Python 2, Java, and C's qsort do):
```
# The old, clunkier way (Python 2 style)
def compare(a, b):
    return abs(a) - abs(b)
```
That gets called O(n log n) times during sorting
sorting n elements costs n key calls, not n log n key calls.

## Quick mental model

| Concept | Your job | Python's job |
|---|---|---|
| `key` | Tell it *what number/string to compare by* | Call your key function once per element, then sort by those values using Timsort |
| comparator (old style) | Tell it *which of two items comes first* | Call your function repeatedly during the sort — slower, more code |
</details>

```
arr = [3, 1, 4, 1, 5]
sorted(arr)                    # [1, 1, 3, 4, 5]
sorted(arr, reverse=True)      # [5, 4, 3, 1, 1]

arr = [-3, 1, -4, 2, -1]
sorted(arr, key=lambda x: abs(x))   # [1, -1, 2, -3, -4]

# Sort list in-place
arr.sort()                     # Modifies original list

# Sort by custom key
words = ["apple", "pie", "banana"]
sorted(words, key=len)         # ['pie', 'apple', 'banana']
```

### List Comprehension

To create a list from another iterable -> quick, consise way of doing this

**[expression for item in iterable]**
• expression: What you want to do to each item before adding it to the new list (e.g., x ** 2, str(x), or just x if you don't want to change it).
• item: The variable name representing the current element (like x).
• iterable: The collection you are looping through (like a list, tuple, or range())

The Traditional Way (4 lines)
```
numbers = [1, 2, 3, 4]
squares = []

for x in numbers:
    squares.append(x ** 2)
```

The List Comprehension Way (1 line)
```
squares = [x ** 2 for x in numbers]
```

**Adding Filters with if Conditions**
You can also add an optional if statement at the very end to filter out items you don't want.

```
numbers = [1, 2, 3, 4, 5, 6]

# Loop with filter: Only square 'x' IF 'x' is even
even_squares = [x ** 2 for x in numbers if x % 2 == 0]

print(even_squares)  # Output: [4, 16, 36]
```

**if/else conditions**
When you want to filter out items, the if goes at the end.
But when you want to change/replace items based on a condition, the if-else goes at the beginning.

The Challenge: You have a list of product prices. You want to create a new list of labels. If a price is 100 or more, label it "Expensive". Otherwise, label it "Cheap".

```
prices = [50, 120, 85, 200]
```

Solution
```
[ "Expensive" if x >= 100 else "Cheap" for x in prices ]
```


### Dictionaries
 In Python, dictionaries use square brackets []

for example way to extract list of vip guest names from this list of objects
 ```
guests = [
    {"name": "Alice", "is_vip": True},
    {"name": "Bob", "is_vip": False},
    {"name": "Charlie", "is_vip": True},
    {"name": "David", "is_vip": False}
]
 ```
this is wrong way of list comprehension [g.name() for g in guests if g.is_vip()]
instead 
To get the value of a key in a dictionary, write it like this:
• g["name"] instead of g.name()
• g["is_vip"] instead of g.is_vip()

Correct way:
[g["name"] for g in guests if g["is_vip"]]

