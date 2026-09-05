# Contains Duplicate

## Approach 1: Nested Iteration (Brute Force)

You take an element, check every element after it to see if any of them matches its value, and if not, move to the next index and repeat.

```java
public class Solution {
    public boolean containsDuplicateBruteForce(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] == nums[j]) {
                    return true; // Found duplicate early
                }
            }
        }
        return false; // Only return false AFTER checking all pairs
    }
}
```

- **Time Complexity:** $\mathcal{O}(n^2)$ because in the worst case, you are using two nested loops to check all possible pairs.
- **Space Complexity:** $\mathcal{O}(1)$ since you aren't storing any additional data structures.

## Approach 2: Sorting

You sort the array first so that any duplicate values end up next to each other, then make a single pass checking each element against its neighbor.

```java
import java.util.Arrays;

public class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums); // O(n log n)

        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }

        return false;
    }
}
```

- **Time Complexity:** $\mathcal{O}(n \log n)$ because the sort dominates the runtime, while the linear scan afterward only adds $\mathcal{O}(n)$.
- **Space Complexity:** $\mathcal{O}(1)$ (or $\mathcal{O}(\log n)$ depending on the sort algorithm's internal recursion stack) since sorting is done in place and no extra data structure is used.

## Approach 3: Hash Set

You walk through the array once, trying to add each element to a Hash Set; if an element fails to add because it's already in the set, you've found a duplicate.

- **Lookup Efficiency:** Just like the Hash Map, a Hash Set gives $\mathcal{O}(1)$ average time for checking membership and inserting, since we only care about presence, not any associated value.

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> seen = new HashSet<>();

        for (int num : nums) {
            // .add() returns false if the item is already present in the set
            if (!seen.add(num)) {
                return true; // Duplicate found!
            }
        }

        return false; // Loop finished without finding any duplicates
    }
}
```

- **Time Complexity:** $\mathcal{O}(n)$ because we only traverse the array once and each insertion/lookup in the hash set takes $\mathcal{O}(1)$ average time.
- **Space Complexity:** $\mathcal{O}(n)$ to store the elements we've seen in the hash set.

## What is a HashSet

Set<Integer> seen = new HashSet<>();

A HashSet is a data structure designed to store a collection of unique elements, allowing you to check for membership, insert, and delete items in constant time—$\mathcal{O}(1)$ average time complexity.

Think of it like a bag that refuses to hold two of the same item: you toss things in, and it only cares whether something is already inside, not where it goes or what order it's in.

### How It Works Under the Hood

- **Backed by a HashMap:** In Java, `HashSet` is actually implemented internally using a `HashMap`. Every element you add becomes a key in that hidden map, paired with a constant dummy value.
- **Hash Function:** When you pass an element into a HashSet, it runs through the same hash function machinery as a HashMap, converting the element into an array index (a "bucket").
- **Instant Lookup:** When you ask whether an element exists, the hash function runs on it again, jumps directly to the corresponding bucket, and checks it—no looping through the whole collection required.
- **Collision Handling:** If two different elements hash to the same bucket (a hash collision), the HashSet stores both in that bucket as a linked list (or balanced tree), just like a HashMap does.

### Key Properties

- **Uniqueness:** A HashSet cannot contain duplicate elements. Calling `.add()` on a value that's already present does nothing and returns `false`, which is exactly what makes it useful for detecting duplicates.
- **Unordered:** Elements are not stored in insertion order or sorted order.
- **Fast Lookup vs. Space:** Like a HashMap, it trades memory for speed—allocating extra memory for array buckets to achieve $\mathcal{O}(1)$ time for `add`, `contains`, and `remove`.

## Important Things to Know About Java's HashSet

- **Wrapper Classes Required:** You cannot use primitive types (`int`, `char`, `double`) directly as elements. You must use their object wrapper counterparts (`Integer`, `Character`, `Double`).
- **Handling Nulls:** Java's HashSet allows a single `null` element.
- **Key Equality Requirements:** If you use custom class objects as elements, you must override both `hashCode()` and `equals()` so the set can properly calculate buckets and detect duplicates.
- **Thread Safety:** Standard HashSet is not synchronized (not thread-safe). For concurrent/multi-threaded applications, you would use `Collections.synchronizedSet(...)` or `java.util.concurrent.ConcurrentHashMap.newKeySet()`.

## Core Operations & Complexity

| Operation | Time Complexity (Average) | Description |
| --- | --- | --- |
| Add | $\mathcal{O}(1)$ | Inserts an element; returns `false` if it was already present |
| Contains | $\mathcal{O}(1)$ | Checks if a given element exists in the set |
| Remove | $\mathcal{O}(1)$ | Deletes an element from the set |
