# Two Sum

## Approach 1: Nested Iteration (Brute Force)

You take an element, check every element after it to see if their sum matches target, and if not, move to the next index and repeat.

```java
public class Solution {
    public int[] twoSumBruteForce(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[] { i, j };
                }
            }
        }
        return new int[] {}; // Return empty array if no pair found
    }
}
```

- **Time Complexity:** $\mathcal{O}(n^2)$ because in the worst case, you are using two nested loops to check all possible pairs.
- **Space Complexity:** $\mathcal{O}(1)$ since you aren't storing any additional data structures.

## Approach 2: Complement Search using Hash Map

Your idea of subtracting the current number from the target (`complement = target - nums[i]`) is the exact key to solving this problem efficiently!

However, there are two subtle details to adjust in your original phrasing:

- **The Math Term:** We say *complement* or *difference* rather than "module" (modulus is remainder after division).
- **Lookup Efficiency:** Searching for the complement in a standard array linearly takes $\mathcal{O}(n)$ time, keeping total time at $\mathcal{O}(n^2)$. To make this fast ($\mathcal{O}(1)$ lookup), we use a Hash Map (`std::unordered_map` in C++) to store elements and their indices as we iterate.

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int[] twoSumHashMap(int[] nums, int target) {
        // Map to store: Key = number value, Value = index in array
        Map<Integer, Integer> numToIndex = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];

            // Check if the required complement already exists in our map
            if (numToIndex.containsKey(complement)) {
                return new int[] { numToIndex.get(complement), i };
            }

            // Store current number and its index
            numToIndex.put(nums[i], i);
        }

        return new int[] {}; // Return empty array if no pair found
    }
}
```

- **Time Complexity:** $\mathcal{O}(n)$ because we only traverse the array once and lookups in the hash map take $\mathcal{O}(1)$ average time.
- **Space Complexity:** $\mathcal{O}(n)$ to store the hash map elements.






## What is a HashMap

Map<Integer, Integer> numToIndex = new HashMap<>();

A HashMap is a data structure designed to store data in Key-Value pairs, allowing you to look up, insert, and delete items in constant time—$\mathcal{O}(1)$ average time complexity.

Think of it like a real-world dictionary: the Key is the word, and the Value is its definition. You use the key to immediately find its corresponding value without scanning through every item.

### How It Works Under the Hood

- **Hash Function:** When you pass a key into a HashMap, it runs through a mathematical function (a hash function) that converts the key into an integer array index (a "bucket").
- **Direct Storage:** The key-value pair is placed inside that specific array index (bucket).
- **Instant Lookup:** When you ask for the value associated with a key, the hash function runs on that key again, calculates the exact array index, and jumps directly to it—no looping required.
- **Collision Handling:** If two different keys produce the exact same array index (a hash collision), the HashMap stores both nodes in that bucket as a linked list (or balanced tree), chaining them together via a Next pointer.

### Key Properties

- **Unique Keys:** Every key in a HashMap must be unique. Inserting an existing key overwrites its previous value.
- **Unordered:** In standard HashMaps (like Java's HashMap or C++'s std::unordered_map), items are not stored in insertion order or sorted order.
- **Fast Lookup vs. Space:** It trades memory for speed—allocating extra memory for array buckets to achieve $\mathcal{O}(1)$ time.

## Important Things to Know About Java's HashMap

- **Wrapper Classes Required:** You cannot use primitive types (`int`, `char`, `double`) directly as keys or values. You must use their object wrapper counterparts (`Integer`, `Character`, `Double`).
- **Handling Nulls:** Java's HashMap allows one null key and multiple null values.
- **Key Equality Requirements:** If you use custom class objects as keys, you must override both `hashCode()` and `equals()` so the map can properly calculate buckets and locate matching keys.
- **Thread Safety:** Standard HashMap is not synchronized (not thread-safe). For concurrent/multi-threaded applications, you would use `java.util.concurrent.ConcurrentHashMap`.
