# Java DSA Learning Handbook

**Living revision document --- update this file as new topics and
problems are completed.**

-   **Language:** Java
-   **Learning approach:** Topic-by-topic, beginner-friendly
    explanations, visual pointer traces, then Java implementation
-   **Current progress:** Sliding Window and Two Pointers completed;
    Fast & Slow Pointers in progress
-   **Last updated:** 8 November 2026

------------------------------------------------------------------------

# Index

## Part I --- Learning Roadmap

1.  [How to use this handbook](#how-to-use-this-handbook)
2.  [Overall DSA roadmap](#overall-dsa-roadmap)
3.  [Progress tracker](#progress-tracker)

## Part II --- Completed Topics

4.  [Topic 1: Sliding Window](#topic-1-sliding-window)
    -   [Fixed-size window](#fixed-size-sliding-window)
    -   [Variable-size window](#variable-size-sliding-window)
    -   [Problems and Java
        patterns](#sliding-window-problems-and-java-patterns)
5.  [Topic 2: Two Pointers](#topic-2-two-pointers)
    -   [Opposite-direction pointers](#opposite-direction-pointers)
    -   [Same-direction pointers](#same-direction-pointers)
    -   [Array problems](#two-pointer-array-problems)
    -   [String problems](#two-pointer-string-problems)

## Part III --- Current Topic

6.  [Topic 3: Fast & Slow Pointers](#topic-3-fast--slow-pointers)
    -   [Linked-list foundation](#linked-list-foundation)
    -   [Problem 1: Find the middle
        node](#problem-1-find-the-middle-node)
    -   [Problem 2: Detect a cycle ---
        next](#problem-2-detect-a-cycle--next)

## Part IV --- Revision

7.  [Complexity quick reference](#complexity-quick-reference)
8.  [Common mistakes and lessons](#common-mistakes-and-lessons)
9.  [Future update template](#future-update-template)

------------------------------------------------------------------------

# How to use this handbook

This is a cumulative study document, not just a list of solutions. For
every problem, keep:

1.  **Problem statement** --- what needs to be solved.
2.  **Core idea** --- the pattern and why it works.
3.  **Walkthrough** --- pointer/window movement on an example.
4.  **Java implementation** --- readable, interview-ready code.
5.  **Complexity** --- time and auxiliary space.
6.  **Edge cases** --- inputs that can break an incomplete solution.

Try to understand the logic and trace it manually before memorising
code. Prefer writing the solution yourself and reviewing it.

# Overall DSA roadmap

The following is the topic sequence we are following. Topics will be
marked complete only after learning and practising them.

1.  Sliding Window
2.  Two Pointers
3.  Fast & Slow Pointers
4.  Merge Intervals
5.  Cyclic Sort
6.  In-place Reversal of Linked List
7.  Tree BFS
8.  Tree DFS
9.  Two Heaps
10. Subsets
11. Modified Binary Search
12. Bitwise XOR
13. Top K Elements
14. K-way Merge
15. 0/1 Knapsack
16. Unbounded Knapsack
17. Topological Sort
18. Union Find
19. Ordered Set / Monotonic Stack
20. Multi-dimensional Dynamic Programming:
    -   Longest Common Subsequence (LCS)
    -   Longest Increasing Subsequence (LIS)
    -   Palindromes
    -   Matrix Chain / Interval DP

# Progress tracker

  Topic                  Status        Progress
  ---------------------- ------------- ---------------------------------
  Sliding Window         Completed     5 problems
  Two Pointers           Completed     Array and string problems
  Fast & Slow Pointers   In progress   Middle of linked list completed
  Remaining topics       Not started   Follow roadmap above

------------------------------------------------------------------------

# Topic 1: Sliding Window

## Core idea

Sliding Window is useful when a problem asks about a **continuous part**
of an array or string: a subarray or substring.

Instead of recalculating every window from scratch, maintain a window
and update it as it moves.

Typical benefits: - Avoid repeated work. - Often reduce a brute-force
solution from O(N × K) or O(N²) to O(N). - Use two boundaries, commonly
`left` and `right`.

## Fixed-size sliding window

The window length `k` remains constant.

Example array:

``` text
[2, 1, 5, 1, 3, 2], k = 3

Window 1: [2, 1, 5]       sum = 8
Window 2:    [1, 5, 1]    sum = 7
Window 3:       [5, 1, 3] sum = 9
Window 4:          [1, 3, 2] sum = 6
```

When the window moves one position: - Add the new element entering from
the right. - Subtract the element leaving from the left.

General pattern:

``` java
int windowSum = 0;

// Build the first window.
for (int i = 0; i < k; i++) {
    windowSum += arr[i];
}

int maxSum = windowSum;

// Slide the window.
for (int right = k; right < arr.length; right++) {
    windowSum += arr[right];
    windowSum -= arr[right - k];
    maxSum = Math.max(maxSum, windowSum);
}
```

## Variable-size sliding window

The window length changes depending on a condition.

General pattern:

``` java
int left = 0;

for (int right = 0; right < arr.length; right++) {
    // Include arr[right] in the current window.

    while (/* window is invalid */) {
        // Remove arr[left] from the window.
        left++;
    }

    // Update the answer using the valid window.
}
```

Important distinction: - `right` expands the window. - `left` shrinks
the window. - The condition determines when to shrink or when to update
the answer.

## Sliding Window problems and Java patterns

### Problem 1 --- Maximum sum of K consecutive elements

**Goal:** Find the largest sum among all contiguous subarrays of size
`k`.

Example:

``` text
Array: [2, 1, 5, 1, 3, 2]
k = 3

Sums: 8, 7, 9, 6
Answer: 9
```

**Logic:** Calculate the first window sum, then slide by adding the
incoming value and subtracting the outgoing value.

**Complexity:** O(N) time, O(1) extra space.

### Problem 2 --- Average of every K consecutive elements

**Goal:** Return the average of each contiguous window of size `k`.

Example:

``` text
Array: [1, 3, 2, 6, -1, 4, 1, 8, 2]
k = 5

First window: [1, 3, 2, 6, -1]
Sum = 11
Average = 11 / 5 = 2.2
```

**Logic:** Use the same fixed-size window sum. Store
`windowSum / (double) k` for each complete window.

**Complexity:** O(N) time, O(N) output space.

### Problem 3 --- Maximum number of vowels in a substring of size K

**Goal:** Find the maximum number of vowels in any substring of length
`k`.

Example:

``` text
String: "abciiidef"
k = 3

"abc" -> 1 vowel
"bci" -> 1 vowel
"cii" -> 2 vowels
"iii" -> 3 vowels

Answer: 3
```

**Logic:** Treat the string like an array of characters. Add one when a
vowel enters the window and subtract one when a vowel leaves.

Vowel check:

``` java
private boolean isVowel(char ch) {
    return "aeiou".indexOf(Character.toLowerCase(ch)) >= 0;
}
```

**Complexity:** O(N) time, O(1) extra space.

### Problem 4 --- Minimum size subarray sum

**Goal:** Find the smallest length of a contiguous subarray whose sum is
at least a target.

Example:

``` text
Array: [2, 3, 1, 2, 4, 3]
Target = 7

[2, 3, 1, 2] -> sum 8, length 4
[3, 1, 2, 4] -> sum 10, length 4
[4, 3]       -> sum 7, length 2

Answer: 2
```

**Logic:** 1. Expand the window by adding `arr[right]`. 2. While the sum
is at least the target, record the current length. 3. Remove `arr[left]`
and increment `left` to try a shorter window.

Core update:

``` java
while (windowSum >= target) {
    minLength = Math.min(minLength, right - left + 1);
    windowSum -= arr[left];
    left++;
}
```

**Important:** This standard approach relies on non-negative array
values.

**Complexity:** O(N) time, O(1) extra space.

### Problem 5 --- Longest substring without repeating characters

**Goal:** Find the length of the longest substring containing no
repeated characters.

Example:

``` text
String: "tmmzuxt"

A longest valid substring is "mzuxt"
Length = 5
```

**Logic:** Use a variable-size window and a `HashSet<Character>`: -
Expand `right`. - If the new character is already present, remove
characters from the left until it is no longer present. - Add the new
character. - Update the maximum window length.

Core pattern:

``` java
Set<Character> seen = new HashSet<>();
int left = 0;
int maxLength = 0;

for (int right = 0; right < str.length(); right++) {
    char current = str.charAt(right);

    while (seen.contains(current)) {
        seen.remove(str.charAt(left));
        left++;
    }

    seen.add(current);
    maxLength = Math.max(maxLength, right - left + 1);
}
```

**Complexity:** O(N) average time, O(min(N, character-set-size)) space.

------------------------------------------------------------------------

# Topic 2: Two Pointers

## Core idea

Two Pointers uses two index variables or references to traverse data
efficiently.

Common arrangements:

1.  **Opposite direction:** one pointer starts at the beginning and the
    other at the end.
2.  **Same direction:** both move forward, often at different speeds or
    with different responsibilities.

Two Pointers is especially useful for sorted arrays, in-place
modifications, reversing, and palindrome checks.

## Opposite-direction pointers

Typical initialisation:

``` java
int left = 0;
int right = arr.length - 1;

while (left < right) {
    // Compare or process arr[left] and arr[right].
    left++;
    right--;
}
```

The pointers move toward each other. The exact movement depends on the
problem.

## Same-direction pointers

A common pattern uses: - `fast` to scan each element. - `slow` to mark
the next position where a useful element should be placed.

Example: remove duplicates from a sorted array.

``` text
Input:  [1, 1, 2, 2, 3]
slow:    ^
fast:    ^

After processing:
[1, 2, 3, _, _]
```

## Two Pointer array problems

### Problem 1 --- Two Sum in a sorted array

**Goal:** Find two values whose sum equals the target.

Example:

``` text
Array: [1, 3, 5, 7, 9, 11]
Target = 14

left = 1, right = 11 -> sum 12
Sum is too small: move left forward.

left = 3, right = 11 -> sum 14
Answer found.
```

**Logic:** Because the array is sorted: - If sum is too small, move
`left` right to increase the sum. - If sum is too large, move `right`
left to decrease the sum. - If equal, return the pair or indices.

``` java
public int[] twoSumSorted(int[] arr, int target) {
    if (arr == null || arr.length < 2) {
        return new int[] {-1, -1};
    }

    int left = 0;
    int right = arr.length - 1;

    while (left < right) {
        int sum = arr[left] + arr[right];

        if (sum == target) {
            return new int[] {left, right};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }

    return new int[] {-1, -1};
}
```

**Complexity:** O(N) time, O(1) extra space.

**Important:** This opposite-direction method requires sorted input. Do
not reject a target merely because it is zero or negative; valid arrays
may contain negative values.

### Problem 2 --- Remove duplicates from a sorted array

**Goal:** Modify the array in place so each distinct value appears once
at the beginning. Return the number of unique values.

Example:

``` text
Input:  [1, 1, 2, 2, 3]
Output prefix: [1, 2, 3]
Unique count: 3
```

**Logic:** - `fast` scans the array. - `slow` tracks the last unique
value's position. - When `arr[fast] != arr[slow]`, increment `slow` and
copy the new unique value there.

``` java
public int removeDuplicates(int[] arr) {
    if (arr == null || arr.length == 0) {
        return 0;
    }

    int slow = 0;

    for (int fast = 1; fast < arr.length; fast++) {
        if (arr[fast] != arr[slow]) {
            slow++;
            arr[slow] = arr[fast];
        }
    }

    return slow + 1;
}
```

**Complexity:** O(N) time, O(1) extra space.

### Problem 3 --- Move zeroes to the end

**Goal:** Move all zeroes to the end while preserving the relative order
of non-zero values.

Example:

``` text
Input:  [0, 1, 0, 3, 12]
Output: [1, 3, 12, 0, 0]
```

**Logic:** Use `fast` to scan and `slow` to identify the next position
for a non-zero value. One clear implementation is to copy non-zero
values forward, then fill the remaining positions with zeroes.

``` java
public void moveZeroes(int[] arr) {
    int slow = 0;

    for (int fast = 0; fast < arr.length; fast++) {
        if (arr[fast] != 0) {
            arr[slow] = arr[fast];
            slow++;
        }
    }

    while (slow < arr.length) {
        arr[slow] = 0;
        slow++;
    }
}
```

**Complexity:** O(N) time, O(1) extra space.

**Lesson:** After incrementing a placement pointer, be careful to write
to the intended index. An alternative is swapping `arr[slow]` and
`arr[fast]` when they differ.

### Problem 4 --- Container With Most Water

**Goal:** Find two vertical lines that, with the x-axis, hold the
maximum amount of water.

Example:

``` text
Heights: [1, 8, 6, 2, 5, 4, 8, 3, 7]

Area = width × shorter height
     = (right - left) × min(height[left], height[right])

Maximum area = 49
```

**Logic:** 1. Start with the widest container: `left = 0`,
`right = n - 1`. 2. Calculate the area. 3. Move the pointer at the
shorter line inward. Moving the taller line cannot improve the limiting
height, and it reduces width. 4. Continue until the pointers meet.

``` java
public int maxArea(int[] height) {
    int left = 0;
    int right = height.length - 1;
    int maxArea = 0;

    while (left < right) {
        int width = right - left;
        int currentArea = width * Math.min(height[left], height[right]);
        maxArea = Math.max(maxArea, currentArea);

        if (height[left] > height[right]) {
            right--;
        } else {
            left++;
        }
    }

    return maxArea;
}
```

**Complexity:** O(N) time, O(1) extra space.

## Two Pointer string problems

### Problem 1 --- Reverse a string

**Goal:** Reverse a character array in place.

Example:

``` text
Input:  H E L L O
Output: O L L E H
```

**Logic:** Swap the characters at `left` and `right`, then move both
pointers inward.

``` java
public void reverseString(char[] chars) {
    int left = 0;
    int right = chars.length - 1;

    while (left < right) {
        char temp = chars[left];
        chars[left] = chars[right];
        chars[right] = temp;

        left++;
        right--;
    }
}
```

**Complexity:** O(N) time, O(1) extra space.

### Problem 2 --- Check whether a string is a palindrome

**Goal:** Check whether a string reads the same forward and backward.

Examples:

``` text
"madam" -> true
"hello" -> false
```

**Logic:** Compare the characters at both ends. If they differ, it is
not a palindrome. Otherwise, move inward.

``` java
public boolean isPalindrome(String str) {
    if (str == null) {
        return false;
    }

    int left = 0;
    int right = str.length() - 1;

    while (left < right) {
        if (str.charAt(left) != str.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }

    return true;
}
```

**Complexity:** O(N) time, O(1) extra space.

### Problem 3 --- Valid Palindrome (ignore punctuation and case)

**Goal:** Check whether a string is a palindrome after ignoring
non-alphanumeric characters and case.

Examples:

``` text
"A man, a plan, a canal: Panama" -> true
"race a car"                     -> false
"!!!"                            -> true
""                               -> true
```

**Logic:** Move `left` and `right` past non-alphanumeric characters.
Compare the remaining characters without case sensitivity.

``` java
public boolean isValidPalindrome(String str) {
    if (str == null) {
        return false;
    }

    int left = 0;
    int right = str.length() - 1;

    while (left < right) {
        while (left < right &&
               !Character.isLetterOrDigit(str.charAt(left))) {
            left++;
        }

        while (left < right &&
               !Character.isLetterOrDigit(str.charAt(right))) {
            right--;
        }

        if (Character.toLowerCase(str.charAt(left)) !=
            Character.toLowerCase(str.charAt(right))) {
            return false;
        }

        left++;
        right--;
    }

    return true;
}
```

**Complexity:** O(N) time, O(1) extra space.

**Edge-case lesson:** The inner loops must also check `left < right`.
This prevents invalid access when the input contains only punctuation,
such as `"!!!"`. An empty string is considered a palindrome after
filtering.

### Problem 4 --- Two Sum II (sorted input)

This is another practice of the same opposite-direction pattern.

Example:

``` text
Array: [1, 2, 7, 11, 15]
Target = 9

1 + 15 = 16 -> too large, move right
1 + 11 = 12 -> too large, move right
1 + 7  = 8  -> too small, move left
2 + 7  = 9  -> found
```

``` java
public int[] twoSumII(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;

    while (left < right) {
        int sum = arr[left] + arr[right];

        if (sum == target) {
            return new int[] {left, right};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }

    return new int[] {-1, -1};
}
```

**Complexity:** O(N) time, O(1) extra space.

**Important correction from practice:** A loop that moves `right`
forward while sometimes incrementing `left` may work for one chosen
example, but it is not the reliable Two Sum II strategy. For a sorted
array, begin at both ends and move inward based on the sum.

------------------------------------------------------------------------

# Topic 3: Fast & Slow Pointers

## Core idea

Fast & Slow Pointers uses two references that move through a linked list
at different speeds:

-   `slow` moves one node at a time.
-   `fast` moves two nodes at a time.

This pattern can help find the middle of a linked list and detect cycles
without using extra collections.

## Linked-list foundation

A linked-list node stores: - A value. - A reference to the next node.

``` java
class Node {
    int data;
    Node next;

    Node(int data) {
        this.data = data;
        this.next = null;
    }
}
```

Create a sample linked list:

``` java
Node head = new Node(10);
head.next = new Node(20);
head.next.next = new Node(30);
head.next.next.next = new Node(40);
head.next.next.next.next = new Node(50);
```

Visual representation:

``` text
head
 ↓
10 → 20 → 30 → 40 → 50 → null
```

Pointer initialisation:

``` java
Node slow = head;
Node fast = head;
```

Movement:

``` java
slow = slow.next;             // one node
fast = fast.next.next;        // two nodes
```

Always check that `fast` and `fast.next` are not null before moving
`fast` two steps.

## Problem 1 --- Find the middle node

**Goal:** Return the middle node of a singly linked list.

Example with an odd number of nodes:

``` text
10 → 20 → 30 → 40 → 50 → null
               ↑
             middle
```

**Logic:** Start both pointers at the head. Move slow by one and fast by
two. When fast reaches the end, slow is at the middle.

``` java
public int findMiddle(Node head) {
    if (head == null) {
        return -1;
    }

    Node slow = head;
    Node fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow.data;
}
```

**Odd-length walkthrough:**

``` text
List: 10 → 20 → 30 → 40 → 50 → null

Start:      slow=10, fast=10
After 1:    slow=20, fast=30
After 2:    slow=30, fast=50
Next check: fast.next is null, so stop.

Answer: 30
```

**Even-length behaviour:**

``` text
List: 10 → 20 → 30 → 40 → null
```

There are two middle nodes: 20 and 30. With both pointers initially at
`head` and the loop condition shown above, the method returns **30**,
the second middle node.

``` text
Start:      slow=10, fast=10
After 1:    slow=20, fast=30
After 2:    slow=30, fast=null
Stop:       fast is null

Answer: 30
```

**Complexity:** O(N) time, O(1) extra space.

**Key interview point:** - Odd number of nodes: returns the exact
middle. - Even number of nodes: returns the second middle with this
implementation.

## Problem 2 --- Detect a cycle --- next

**Status:** Not yet practised. This is the next problem.

**Goal:** Determine whether a linked list contains a cycle.

Example:

``` text
10 → 20 → 30 → 40 → 50
          ↑         |
          └─────────┘
```

The `next` reference from node 50 points back to node 30 instead of
`null`. The list therefore loops forever if traversed using a single
pointer.

**Expected approach:** Floyd's Cycle Detection Algorithm: - Move `slow`
one node at a time. - Move `fast` two nodes at a time. - If the list has
a cycle, the pointers will eventually meet. - If `fast` or `fast.next`
becomes `null`, there is no cycle.

Core logic to study and implement next:

``` java
public boolean hasCycle(Node head) {
    Node slow = head;
    Node fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;

        if (slow == fast) {
            return true;
        }
    }

    return false;
}
```

**Important:** Compare node references (`slow == fast`), not the integer
values stored in the nodes. Two different nodes can contain the same
value.

**Complexity:** O(N) time, O(1) extra space.

------------------------------------------------------------------------

# Complexity quick reference

  Pattern / problem                     Typical time                 Extra space
  ----------------------------------- -------------- ---------------------------
  Fixed-size Sliding Window                     O(N)      O(1), excluding output
  Variable-size Sliding Window                  O(N)   Depends on data structure
  Two Sum in sorted array                       O(N)                        O(1)
  Remove duplicates in sorted array             O(N)                        O(1)
  Move zeroes                                   O(N)                        O(1)
  Container With Most Water                     O(N)                        O(1)
  Reverse character array                       O(N)                        O(1)
  Palindrome check                              O(N)                        O(1)
  Longest substring without repeats     O(N) average            O(character set)
  Find linked-list middle                       O(N)                        O(1)
  Detect linked-list cycle                      O(N)                        O(1)

`N` is the number of array elements, characters, or linked-list nodes,
as applicable. Output storage is excluded when describing auxiliary
space.

# Common mistakes and lessons

1.  **Do not move pointers without a reason.** Explain what the movement
    achieves: increasing a sum, decreasing a sum, skipping an invalid
    character, or placing a valid element.
2.  **Sorted-array Two Sum:** use one pointer at each end. If the sum is
    too small, move left; if too large, move right.
3.  **Remove duplicates:** compare the scanning value with the last
    unique value, then advance the placement pointer only when a new
    value is found.
4.  **Move zeroes:** keep the placement index and the scanning index
    separate. Write to the correct index after moving a pointer.
5.  **Palindrome edge cases:** empty and single-character strings are
    palindromes. When skipping punctuation, keep `left < right` in the
    inner loops.
6.  **Fast & Slow Pointers:** check `fast != null && fast.next != null`
    before moving fast two steps.
7.  **Linked-list equality:** use reference comparison (`slow == fast`)
    to check whether two pointers refer to the same node.
8.  **Do not over-restrict input:** for example, a sorted Two Sum target
    can be zero or negative if the array contains negative values.

# Future update template

Copy this section when adding a new problem.

## Problem N --- \[Problem name\]

**Status:** Not started / Practising / Completed

**Goal:**

\[Explain the problem in one or two sentences.\]

**Example:**

``` text
Input:
Output:
```

**Core idea:**

\[Explain the pattern and why it works.\]

**Walkthrough:**

\[Show each important pointer/window/state change.\]

**Java solution:**

``` java
// Add the final reviewed Java implementation here.
```

**Complexity:** - Time: - Auxiliary space:

## **Edge cases:**

-   

## **Lessons learned:**

------------------------------------------------------------------------

# Revision checklist

Use this checklist during revision:

-   [ ] Can I explain the pattern without looking at the solution?
-   [ ] Can I trace the example manually?
-   [ ] Can I write the Java code from memory?
-   [ ] Can I explain time and space complexity?
-   [ ] Have I tested empty, single-element, and boundary inputs where
    applicable?
-   [ ] Can I explain why each pointer moves?

**Current next step:** Continue Topic 3 with Problem 2 --- Detect Cycle
in Linked List.
