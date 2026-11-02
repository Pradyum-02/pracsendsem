
# DESIGN AND ANALYSIS OF ALGORITHMS (DAA)
## Complete Practical Journal – Python

**Programming Language:** Python 3  
**Execution Environment:** Linux / Ubuntu  
**Total Practicals:** 5

---

# INDEX

| Sr. No. | Practical Name | Algorithm / Technique |
|---|---|---|
| 1 | Fibonacci Series (Recursive and Non-Recursive) | Iteration and Recursion |
| 2 | Huffman Coding | Greedy Algorithm |
| 3 | Fractional Knapsack | Greedy Algorithm |
| 4 | 0/1 Knapsack | Dynamic Programming |
| 5 | N-Queen Problem | Backtracking |

---

# PRACTICAL 1 – FIBONACCI SERIES

## 1. Problem Statement

Write a program using non-recursive and recursive methods to calculate Fibonacci numbers and analyze their time and space complexity.

## 2. Theory

The Fibonacci series is a sequence in which each number is the sum of the previous two numbers.

The series starts with:

`0, 1, 1, 2, 3, 5, 8, 13, 21, 34 ...`

Mathematically:

F(n) = F(n-1) + F(n-2)

Where:
- First number = 0
- Second number = 1
- Every subsequent number is the sum of the previous two numbers.

There are two methods used in this practical:

1. **Non-Recursive Method:** Uses loops to generate the series.
2. **Recursive Method:** Uses a function that calls itself to generate the series.

## 3. Algorithm

### A. Non-Recursive Algorithm

1. Start.
2. Accept the number of terms from the user.
3. Initialize two variables `a = 0` and `b = 1`.
4. Create an empty list to store the Fibonacci series.
5. Repeat the following steps n times:
   - Append `a` to the list.
   - Update `a = b`.
   - Update `b = a + b` using the previous values.
6. Display the Fibonacci series.
7. Stop.

### B. Recursive Algorithm

1. Start.
2. Accept the number of terms.
3. Check the base conditions:
   - If n is less than or equal to 0, return an empty list.
   - If n is 1, return `[0]`.
   - If n is 2, return `[0, 1]`.
4. Otherwise, call the same function with n-1.
5. Add the sum of the last two elements to the returned list.
6. Return the updated list.
7. Display the Fibonacci series.
8. Stop.

## 4. Python Code

**Filename:** `practical1.py`

```python
# Non-recursive Fibonacci
def fibonacci(n):
    fib_series = []
    a = 0
    b = 1

    for i in range(n):
        fib_series.append(a)
        a, b = b, a + b

    return fib_series


# Recursive Fibonacci
def fibonacci_recursive(n):
    if n <= 0:
        return []
    elif n == 1:
        return [0]
    elif n == 2:
        return [0, 1]
    else:
        fib_series = fibonacci_recursive(n - 1)
        fib_series.append(fib_series[-1] + fib_series[-2])
        return fib_series


# Main program
n = int(input("Enter total numbers to print in fibonacci series:\t"))

# Non-recursion
print("Fibonacci Series (non-recusive):\t", fibonacci(n))

# Recursion
print("Fibonacci Series (recusive):\t\t", fibonacci_recursive(n))
```

## 5. Sample Output

```text
Enter total numbers to print in fibonacci series: 7

Fibonacci Series (non-recusive): [0, 1, 1, 2, 3, 5, 8]

Fibonacci Series (recusive): [0, 1, 1, 2, 3, 5, 8]
```

## 6. How to Execute in Linux

Step 1: Open the terminal.

Step 2: Create the Python file.

```bash
nano practical1.py
```

Step 3: Paste the code and save it.

- Press Ctrl + O and Enter.
- Press Ctrl + X to exit.

Step 4: Execute the program.

```bash
python3 practical1.py
```

Step 5: Enter the number of terms.

```text
7
```

The Fibonacci series will be displayed using both methods.

## 7. Explanation of the Code

### Part A – Non-Recursive Function

```python
def fibonacci(n):
```

Defines a function named `fibonacci` that accepts the number of terms.

```python
fib_series = []
```

Creates an empty list to store Fibonacci numbers.

```python
a = 0
b = 1
```

Initializes the first two Fibonacci numbers.

```python
for i in range(n):
```

Executes the loop n times.

```python
fib_series.append(a)
```

Adds the current Fibonacci number to the list.

```python
a, b = b, a + b
```

Updates the variables to generate the next Fibonacci number.

The old value of b becomes the new a, and the sum of the old a and b becomes the new b.

```python
return fib_series
```

Returns the complete Fibonacci series.

### Part B – Recursive Function

```python
def fibonacci_recursive(n):
```

Defines a recursive function.

A recursive function is a function that calls itself.

**Base Conditions:**

```python
if n <= 0:
    return []
```

Returns an empty list if the number of terms is zero or negative.

```python
elif n == 1:
    return [0]
```

Returns the first Fibonacci number.

```python
elif n == 2:
    return [0, 1]
```

Returns the first two Fibonacci numbers.

**Recursive Condition:**

```python
fib_series = fibonacci_recursive(n - 1)
```

Calls the same function with one less term.

```python
fib_series.append(fib_series[-1] + fib_series[-2])
```

Adds the sum of the last two elements to generate the next Fibonacci number.

```python
return fib_series
```

Returns the updated Fibonacci series.

### Part C – Main Program

```python
n = int(input("Enter total numbers to print in fibonacci series:\t"))
```

Accepts the number of terms from the user.

```python
print("Fibonacci Series (non-recusive):\t", fibonacci(n))
```

Calls the non-recursive function and displays the result.

```python
print("Fibonacci Series (recusive):\t\t", fibonacci_recursive(n))
```

Calls the recursive function and displays the result.

## 8. Time and Space Complexity

| Method | Time Complexity | Space Complexity |
|---|---|---|
| Non-Recursive | O(n) | O(n) |
| Recursive | O(n) | O(n) |

Note: Both implementations construct and return the complete Fibonacci list. The iterative method uses O(1) auxiliary space apart from the output list.

## 9. Viva Questions and Answers

**Q1. What is Fibonacci series?**

A sequence in which every number is the sum of the previous two numbers.

**Q2. What is recursion?**

Recursion is a programming technique in which a function calls itself.

**Q3. What is iteration?**

Iteration is the repeated execution of statements using loops.

**Q4. What is a base condition?**

A condition that stops recursive function calls.

**Q5. Which method is more memory efficient?**

The iterative method because it does not require a recursive call stack.

**Q6. What is the time complexity of Fibonacci using iteration?**

O(n).

**Q7. What is the purpose of append()?**

It adds an element to the end of a list.

---

# PRACTICAL 2 – HUFFMAN CODING

## 1. Problem Statement

Implement Huffman Coding to generate variable-length binary codes for characters based on their frequencies. Construct a Huffman tree, display the codes and encode the given text.

## 2. Theory

Huffman Coding is a lossless data compression algorithm.

It assigns:
- Shorter binary codes to frequently occurring characters.
- Longer binary codes to less frequently occurring characters.

For example, consider the input:

`MEOWMEOW`

The algorithm calculates the frequency of each character and constructs a binary tree.

The tree is used to generate unique binary codes for each character.

**Important Concepts:**
- Greedy Algorithm
- Binary Tree
- Priority Queue
- Min Heap
- Tree Traversal

## 3. Algorithm

1. Start.
2. Accept the input string from the user.
3. Calculate the frequency of each character.
4. Insert each character and its frequency into a min heap.
5. Remove the two nodes with the smallest frequencies.
6. Create a new parent node with the sum of their frequencies.
7. Insert the parent node back into the heap.
8. Repeat until only one node remains.
9. The remaining node becomes the root of the Huffman tree.
10. Traverse the tree:
    - Add 0 when moving left.
    - Add 1 when moving right.
11. Generate and display Huffman codes.
12. Encode the original text using the generated codes.
13. Stop.

## 4. Python Code

**Filename:** `practical2.py`

```python
import heapq
from itertools import count


# Huffman Node
class HuffmanNode:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None


# Generate Huffman Codes
def generateCodes(node, code):
    if node is None:
        return

    if node.left is None and node.right is None:
        huffmanCodes[node.char] = code if code else "0"
        return

    generateCodes(node.left, code + "0")
    generateCodes(node.right, code + "1")


# Build Huffman Tree
def buildHuffmanTree():
    pq = []
    serial = count()

    for ch, freq in sorted(freqMap.items()):
        heapq.heappush(
            pq,
            (freq, next(serial), HuffmanNode(ch, freq))
        )

    while len(pq) > 1:

        left = heapq.heappop(pq)[2]
        right = heapq.heappop(pq)[2]

        parent = HuffmanNode('-', left.freq + right.freq)

        parent.left = left
        parent.right = right

        heapq.heappush(
            pq,
            (parent.freq, next(serial), parent)
        )

    root = pq[0][2]

    huffmanCodes.clear()
    generateCodes(root, "")


# Display Codes
def displayCodes():
    if not huffmanCodes:
        print("No Huffman codes generated yet!")
        return

    print("\nCharacter\tFrequency\tHuffman Code")

    for ch, code in huffmanCodes.items():
        print(ch, "\t\t", freqMap[ch], "\t\t", code)


# Encode Text
def encodeText():
    if not huffmanCodes:
        print("Please build Huffman Tree first!")
        return

    encoded = ""

    for ch in inputText:
        encoded += huffmanCodes[ch]

    print("\nEncoded Text:", encoded)


# Main Program
freqMap = {}
huffmanCodes = {}
inputText = ""

while True:

    print("\n=== Huffman Encoding Menu ===")
    print("1. Enter input string (auto frequency)")
    print("2. Enter characters with frequencies manually")
    print("3. Build Huffman Tree & display codes")
    print("4. Encode the text")
    print("5. Exit")

    choice = int(input("Enter your choice: "))

    if choice == 1:

        inputText = input("Enter the text: ")
        freqMap.clear()
        huffmanCodes.clear()

        for ch in inputText:
            freqMap[ch] = freqMap.get(ch, 0) + 1

    elif choice == 2:

        freqMap.clear()
        huffmanCodes.clear()
        inputText = ""

        n = int(input("Enter number of characters: "))

        for i in range(n):

            ch = input("Character " + str(i + 1) + ": ")[0]

            f = int(input("Frequency of " + ch + ": "))

            freqMap[ch] = f
            inputText += ch * f

    elif choice == 3:

        if not freqMap:
            print("Please enter input first!")
        else:
            buildHuffmanTree()
            displayCodes()

    elif choice == 4:

        if inputText == "":
            print("Enter input text first!")
        elif not huffmanCodes:
            print("Please build Huffman Tree first!")
        else:
            encodeText()

    elif choice == 5:

        print("Exiting program. Goodbye!")
        break

    else:
        print("Invalid choice!")
```

## 5. Sample Output

For the input `meowmeow`, one valid execution is:

```text
=== Huffman Encoding Menu ===
1. Enter input string (auto frequency)
2. Enter characters with frequencies manually
3. Build Huffman Tree & display codes
4. Encode the text
5. Exit

Enter your choice: 1
Enter the text: meowmeow

=== Huffman Encoding Menu ===
1. Enter input string (auto frequency)
2. Enter characters with frequencies manually
3. Build Huffman Tree & display codes
4. Encode the text
5. Exit

Enter your choice: 3

Character       Frequency       Huffman Code
e               2               00
m               2               01
o               2               10
w               2               11

=== Huffman Encoding Menu ===
1. Enter input string (auto frequency)
2. Enter characters with frequencies manually
3. Build Huffman Tree & display codes
4. Encode the text
5. Exit

Enter your choice: 4

Encoded Text: 0100101101001011

=== Huffman Encoding Menu ===
1. Enter input string (auto frequency)
2. Enter characters with frequencies manually
3. Build Huffman Tree & display codes
4. Encode the text
5. Exit

Enter your choice: 5
Exiting program. Goodbye!
```

Note: Huffman codes may differ when characters have equal frequencies because multiple valid trees are possible. The output above corresponds to the deterministic tie-breaking used in this Python implementation.

## 6. How to Execute in Linux

Step 1: Open the terminal.

Step 2: Create the Python file.

```bash
nano practical2.py
```

Step 3: Paste the code and save it.

Step 4: Execute the program.

```bash
python3 practical2.py
```

No external Python packages are required. `heapq` and `itertools` are built-in Python modules.

## 7. Explanation of the Code

### Part A – Importing heapq

```python
import heapq
```

Imports the heap module.

It provides a priority queue that helps us retrieve the node with the smallest frequency.

```python
from itertools import count
```

Creates a counter used to break ties between nodes having equal frequencies.

### Part B – HuffmanNode Class

```python
class HuffmanNode:
```

Creates a class representing a node of the Huffman tree.

```python
self.char = char
self.freq = freq
```

Stores the character and its frequency.

```python
self.left = None
self.right = None
```

Stores the left and right child nodes.

### Part C – generateCodes()

```python
def generateCodes(node, code):
```

Generates binary codes by traversing the Huffman tree.

```python
if node is None:
    return
```

Stops if the current node does not exist.

```python
if node.left is None and node.right is None:
```

Checks whether the current node is a leaf node.

```python
huffmanCodes[node.char] = code
```

Stores the binary code for the character.

```python
generateCodes(node.left, code + "0")
```

Moves left and appends 0.

```python
generateCodes(node.right, code + "1")
```

Moves right and appends 1.

### Part D – buildHuffmanTree()

```python
pq = []
```

Creates an empty priority queue.

```python
heapq.heappush(pq, ...)
```

Inserts character nodes into the min heap.

```python
while len(pq) > 1:
```

Repeats until only one node remains.

```python
left = heapq.heappop(pq)[2]
right = heapq.heappop(pq)[2]
```

Extracts the two nodes having the smallest frequencies.

```python
parent = HuffmanNode('-', left.freq + right.freq)
```

Creates a parent node whose frequency is the sum of the two child frequencies.

```python
parent.left = left
parent.right = right
```

Connects both child nodes to the parent.

```python
heapq.heappush(pq, ...)
```

Inserts the parent node back into the priority queue.

Finally, the remaining node becomes the root of the Huffman tree.

### Part E – displayCodes()

Displays:
- Character
- Frequency
- Huffman code

### Part F – encodeText()

```python
for ch in inputText:
    encoded += huffmanCodes[ch]
```

Replaces every character with its corresponding binary code.

### Part G – Main Menu

The program provides five options:

| Option | Function |
|---|---|
| 1 | Enter text and calculate frequencies automatically |
| 2 | Enter characters and frequencies manually |
| 3 | Build Huffman tree and display codes |
| 4 | Encode the text |
| 5 | Exit |

The frequency calculation is performed using:

```python
freqMap[ch] = freqMap.get(ch, 0) + 1
```

It counts the occurrence of every character.

## 8. Time and Space Complexity

Let:
- n = Length of input text.
- k = Number of distinct characters.

| Operation | Time Complexity |
|---|---|
| Frequency Calculation | O(n) |
| Building Huffman Tree | O(k log k) |
| Generating Codes | O(k) |
| Encoding Text | O(n) |

Space Complexity: O(n + k), including the input and encoded strings, tree and code map.

## 9. Viva Questions and Answers

**Q1. What is Huffman Coding?**

Huffman Coding is a lossless data compression algorithm that assigns variable-length binary codes according to character frequency.

**Q2. What is a Greedy Algorithm?**

An algorithm that makes the locally optimal choice at each step.

**Q3. What is a Priority Queue?**

A data structure that retrieves elements according to their priority.

**Q4. What is a leaf node?**

A node that does not have any children.

**Q5. Why do we use 0 and 1?**

To represent left and right traversal in the Huffman tree.

**Q6. Is Huffman Coding lossless?**

Yes. The original text can be reconstructed using the corresponding Huffman codes and tree.

**Q7. What is a Min Heap?**

A binary heap in which the smallest element has the highest priority and is available at the root.

---

# PRACTICAL 3 – FRACTIONAL KNAPSACK

## 1. Problem Statement

Write a program to solve the Fractional Knapsack problem using a Greedy approach. Given a set of items with their values and weights, maximize the total value placed in a knapsack of fixed capacity. Fractions of items can be selected.

## 2. Theory

Fractional Knapsack is an optimization problem in which fractions of items can be selected to maximize the total value within a given capacity.

The algorithm uses the value-to-weight ratio.

Formula:

Ratio = Value / Weight

The item with the highest ratio is selected first.

Example:

| Item | Value | Weight | Ratio |
|---|---:|---:|---:|
| 1 | 3 | 5 | 0.6 |
| 2 | 6 | 3 | 2.0 |
| 3 | 3 | 6 | 0.5 |
| 4 | 3 | 6 | 0.5 |
| 5 | 2 | 6 | 0.33 |

Knapsack capacity = 7.

We select:
- Item 2 completely: Value = 6, Weight = 3.
- 80% of item 1: Value = 2.4, Weight = 4.

Maximum total value = 8.40.

This practical uses the Greedy Algorithm.

## 3. Algorithm

1. Start.
2. Accept the number of items.
3. Accept the value and weight of each item.
4. Calculate the value-to-weight ratio for each item.
5. Sort the items in descending order of their ratios.
6. Accept the knapsack capacity.
7. Initialize total value to zero.
8. Select items in sorted order:
   - If the complete item fits, select it.
   - Otherwise, calculate the fraction that fits.
   - Add the corresponding value.
9. Stop when the capacity becomes zero.
10. Display the maximum total value.
11. Stop.

## 4. Python Code

**Filename:** `practical3.py`

```python
# Fractional Knapsack

class Item:
    def __init__(self, value, weight):
        self.value = value
        self.weight = weight
        self.ratio = value / weight


items = []

while True:

    print("\n=== Fractional Knapsack Menu ===")
    print("1. Enter items")
    print("2. Solve fractional knapsack")
    print("3. Exit")

    choice = int(input("Enter your choice: "))

    if choice == 1:

        items.clear()

        n = int(input("Enter number of items: "))

        for i in range(n):

            v = int(input("Item " + str(i + 1) + " value: "))
            w = int(input("Item " + str(i + 1) + " weight: "))

            items.append(Item(v, w))

    elif choice == 2:

        if not items:
            print("Please enter items first!")
            continue

        capacity = int(input("Enter knapsack capacity: "))

        # Sort by value-to-weight ratio
        items.sort(key=lambda x: x.ratio, reverse=True)

        totalValue = 0.0
        remaining = capacity

        print("\nItems taken (value/weight fraction):")

        for item in items:

            if remaining == 0:
                break

            if item.weight <= remaining:

                print(str(item.value) + "/" +
                      str(item.weight) + " -> Full")

                totalValue += item.value
                remaining -= item.weight

            else:

                fraction = remaining / item.weight

                print(str(item.value) + "/" +
                      str(item.weight) + " -> " +
                      str(fraction * 100) + "%")

                totalValue += item.value * fraction
                remaining = 0

        print("Maximum total value in knapsack: %.2f" % totalValue)

    elif choice == 3:

        print("Exiting program. Goodbye!")
        break

    else:
        print("Invalid choice!")
```

## 5. Sample Output

```text
=== Fractional Knapsack Menu ===
1. Enter items
2. Solve fractional knapsack
3. Exit

Enter your choice: 1
Enter number of items: 5

Item 1 value: 3
Item 1 weight: 5

Item 2 value: 6
Item 2 weight: 3

Item 3 value: 3
Item 3 weight: 6

Item 4 value: 3
Item 4 weight: 6

Item 5 value: 2
Item 5 weight: 6

=== Fractional Knapsack Menu ===
1. Enter items
2. Solve fractional knapsack
3. Exit

Enter your choice: 2
Enter knapsack capacity: 7

Items taken (value/weight fraction):
6/3 -> Full
3/5 -> 80.0%

Maximum total value in knapsack: 8.40

=== Fractional Knapsack Menu ===
1. Enter items
2. Solve fractional knapsack
3. Exit

Enter your choice: 3
Exiting program. Goodbye!
```

## 6. How to Execute in Linux

Step 1: Open the terminal.

Step 2: Create the file.

```bash
nano practical3.py
```

Step 3: Paste the code and save.

Step 4: Execute.

```bash
python3 practical3.py
```

## 7. Explanation of the Code

### Part A – Item Class

```python
class Item:
```

Creates a class named Item.

```python
self.value = value
self.weight = weight
```

Stores the value and weight of the item.

```python
self.ratio = value / weight
```

Calculates the value-to-weight ratio.

### Part B – Creating Items

```python
items = []
```

Creates an empty list to store the items.

```python
items.append(Item(v, w))
```

Creates an item object and adds it to the list.

### Part C – Sorting

```python
items.sort(key=lambda x: x.ratio, reverse=True)
```

Sorts the items in descending order according to their value-to-weight ratios.

`reverse=True` ensures that the highest ratio comes first.

### Part D – Initializing Variables

```python
totalValue = 0.0
remaining = capacity
```

- `totalValue` stores the total value collected.
- `remaining` stores the remaining capacity of the knapsack.

### Part E – Checking Whether an Item Fits

```python
if item.weight <= remaining:
```

Checks whether the complete item fits into the knapsack.

If it fits:

```python
totalValue += item.value
remaining -= item.weight
```

Adds its complete value and reduces the remaining capacity.

If it does not fit:

```python
fraction = remaining / item.weight
```

Calculates the fraction of the item that can fit.

```python
totalValue += item.value * fraction
```

Adds the proportional value.

```python
remaining = 0
```

The knapsack is now full.

### Part F – Displaying the Result

```python
print("Maximum total value in knapsack: %.2f" % totalValue)
```

Displays the maximum value with two decimal places.

## 8. Time and Space Complexity

Let n be the number of items.

| Operation | Complexity |
|---|---|
| Calculating ratios | O(n) |
| Sorting items | O(n log n) |
| Selecting items | O(n) |
| Overall Time | O(n log n) |
| Space | O(n) |

## 9. Viva Questions and Answers

**Q1. What is the Fractional Knapsack problem?**

An optimization problem in which fractions of items can be selected to maximize the total value.

**Q2. Which technique is used?**

Greedy Algorithm.

**Q3. What is the selection criterion?**

Value-to-weight ratio.

**Q4. Why do we sort items?**

To select the items having the highest value-to-weight ratio first.

**Q5. Can we take half an item?**

Yes. Fractional Knapsack allows taking fractions.

**Q6. What is the time complexity?**

O(n log n).

**Q7. What is the difference between Fractional and 0/1 Knapsack?**

Fractional Knapsack allows partial items and uses a Greedy approach. 0/1 Knapsack does not allow partial items and commonly uses Dynamic Programming.

---

# PRACTICAL 4 – 0/1 KNAPSACK USING DYNAMIC PROGRAMMING

## 1. Problem Statement

Write a program to solve the 0/1 Knapsack problem using Dynamic Programming. Given a set of items with their values and weights, find the maximum value that can be placed inside a knapsack of a given capacity. Each item can either be selected completely or not selected at all.

## 2. Theory

The 0/1 Knapsack problem is an optimization problem.

Each item has two possibilities:

- 1 = Select the item.
- 0 = Do not select the item.

Unlike Fractional Knapsack, fractions of items are not allowed.

This practical uses Dynamic Programming.

Dynamic Programming solves a complex problem by dividing it into smaller subproblems and storing their results.

Example:

| Item | Value | Weight |
|---|---:|---:|
| 1 | 45 | 32 |
| 2 | 6 | 3 |
| 3 | 7 | 3 |

Knapsack capacity = 7.

We can select item 2 and item 3.

Total weight = 3 + 3 = 6.

Total value = 6 + 7 = 13.

Maximum value = 13.

### Dynamic Programming Formula

Let dp[i][w] represent the maximum value obtainable using the first i items with capacity w.

If the current item fits:

dp[i][w] = max(
    value[i-1] + dp[i-1][w-weight[i-1]],
    dp[i-1][w]
)

If the item does not fit:

dp[i][w] = dp[i-1][w]

Base condition:

If i = 0 or w = 0, then dp[i][w] = 0.

## 3. Algorithm

1. Start.
2. Accept the number of items.
3. Accept the value and weight of each item.
4. Accept the knapsack capacity.
5. Create a DP table of size (n+1) × (W+1).
6. Initialize the first row and first column with zero.
7. For every item and capacity:
   - If the item does not fit, copy the previous row's value.
   - Otherwise, compare the include and exclude choices.
   - Store the maximum value.
8. Display the DP table.
9. Display dp[n][W] as the maximum value.
10. Stop.

## 4. Python Code

**Filename:** `practical4.py`

```python
# Build DP Table
def buildKnapsackDP(values, weights, n, W):

    dp = [[0 for w in range(W + 1)]
          for i in range(n + 1)]

    for i in range(n + 1):

        for w in range(W + 1):

            if i == 0 or w == 0:
                dp[i][w] = 0

            elif weights[i - 1] <= w:

                dp[i][w] = max(
                    values[i - 1] +
                    dp[i - 1][w - weights[i - 1]],

                    dp[i - 1][w]
                )

            else:
                dp[i][w] = dp[i - 1][w]

    return dp


# Print DP Table
def printDPTable(dp, n, W):

    print("    ", end="")

    for w in range(W + 1):
        print("%4d" % w, end="")

    print()

    for i in range(n + 1):

        print("%2d " % i, end="")

        for w in range(W + 1):
            print("%4d" % dp[i][w], end="")

        print()


# Main Program
values = None
weights = None
n = 0

while True:

    print("\n=== 0-1 Knapsack Menu ===")
    print("1. Enter items (values & weights)")
    print("2. Solve 0-1 Knapsack")
    print("3. Exit")

    choice = int(input("Enter your choice: "))

    if choice == 1:

        n = int(input("Enter number of items: "))

        values = []
        weights = []

        for i in range(n):

            v = int(input("Item " + str(i + 1) + " value: "))
            w = int(input("Item " + str(i + 1) + " weight: "))

            values.append(v)
            weights.append(w)

    elif choice == 2:

        if values is None or weights is None or n == 0:
            print("Please enter items first!")
            continue

        capacity = int(input("Enter knapsack capacity: "))

        dp = buildKnapsackDP(values, weights, n, capacity)

        print("\nDP Table:")

        printDPTable(dp, n, capacity)

        print("Maximum value that can be put in knapsack:",
              dp[n][capacity])

    elif choice == 3:

        print("Exiting program. Goodbye!")
        break

    else:
        print("Invalid choice!")
```

## 5. Sample Output

```text
=== 0-1 Knapsack Menu ===
1. Enter items (values & weights)
2. Solve 0-1 Knapsack
3. Exit

Enter your choice: 1

Enter number of items: 3

Item 1 value: 45
Item 1 weight: 32

Item 2 value: 6
Item 2 weight: 3

Item 3 value: 7
Item 3 weight: 3

=== 0-1 Knapsack Menu ===
1. Enter items (values & weights)
2. Solve 0-1 Knapsack
3. Exit

Enter your choice: 2

Enter knapsack capacity: 7

DP Table:

       0   1   2   3   4   5   6   7
 0     0   0   0   0   0   0   0   0
 1     0   0   0   0   0   0   0   0
 2     0   0   0   6   6   6   6   6
 3     0   0   0   7   7   7  13  13

Maximum value that can be put in knapsack: 13

=== 0-1 Knapsack Menu ===
1. Enter items (values & weights)
2. Solve 0-1 Knapsack
3. Exit

Enter your choice: 3

Exiting program. Goodbye!
```

## 6. How to Execute in Linux

Step 1: Open the terminal.

Step 2: Create the Python file.

```bash
nano practical4.py
```

Step 3: Paste the code and save it.

Step 4: Execute the program.

```bash
python3 practical4.py
```

## 7. Explanation of the Code

### Part A – buildKnapsackDP()

```python
def buildKnapsackDP(values, weights, n, W):
```

Defines a function that calculates the Dynamic Programming table.

Parameters:

- `values`: List containing item values.
- `weights`: List containing item weights.
- `n`: Number of items.
- `W`: Knapsack capacity.

### Part B – Creating the DP Table

```python
dp = [[0 for w in range(W + 1)]
      for i in range(n + 1)]
```

Creates a two-dimensional list initialized with zeros.

If n = 3 and W = 7, the table size is:

(n + 1) × (W + 1) = 4 × 8

The extra row represents zero items.

The extra column represents zero capacity.

### Part C – Nested Loops

```python
for i in range(n + 1):
```

Iterates through all items.

```python
for w in range(W + 1):
```

Iterates through all possible capacities.

### Part D – Base Condition

```python
if i == 0 or w == 0:
    dp[i][w] = 0
```

If there are no items or the capacity is zero, the maximum value is zero.

### Part E – Checking Whether an Item Fits

```python
elif weights[i - 1] <= w:
```

Checks whether the current item fits into the available capacity.

If it fits, we have two choices.

**Choice 1: Include the item**

```python
values[i - 1] + dp[i - 1][w - weights[i - 1]]
```

Adds the value of the current item to the best result for the remaining capacity.

**Choice 2: Exclude the item**

```python
dp[i - 1][w]
```

Uses the result from the previous row without selecting the current item.

```python
dp[i][w] = max(choice1, choice2)
```

Stores whichever choice gives the maximum value.

### Part F – If the Item Does Not Fit

```python
else:
    dp[i][w] = dp[i - 1][w]
```

If the current item's weight exceeds the available capacity, it cannot be selected.

Therefore, the previous row's result is copied.

### Part G – Returning the DP Table

```python
return dp
```

Returns the completed DP table.

### Part H – printDPTable()

This function displays the complete DP table.

```python
print("%4d" % w, end="")
```

Displays each capacity with proper spacing.

```python
print("%4d" % dp[i][w], end="")
```

Displays each calculated DP value.

### Part I – Main Program

The program provides three menu options:

| Option | Function |
|---|---|
| 1 | Enter items and their values and weights |
| 2 | Solve 0/1 Knapsack |
| 3 | Exit |

```python
dp = buildKnapsackDP(values, weights, n, capacity)
```

Calls the function to calculate the DP table.

```python
dp[n][capacity]
```

Displays the maximum possible value.

## 8. Understanding the DP Table

The DP table stores the maximum value obtainable for different numbers of items and different capacities.

| Items Considered | Capacity 3 | Capacity 6 | Capacity 7 |
|---|---:|---:|---:|
| 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 0 |
| 2 | 6 | 6 | 6 |
| 3 | 7 | 13 | 13 |

For capacity 6, we can select:

- Item 2: Value 6, Weight 3.
- Item 3: Value 7, Weight 3.

Total value = 13.

Therefore:

dp[3][6] = 13.

## 9. Time and Space Complexity

Let:
- n = Number of items.
- W = Knapsack capacity.

| Complexity | Value |
|---|---|
| Time Complexity | O(n × W) |
| Space Complexity | O(n × W) |

The program uses two nested loops to fill the DP table.

## 10. Viva Questions and Answers

**Q1. What is Dynamic Programming?**

A technique that solves a problem by dividing it into smaller subproblems and storing their results.

**Q2. What is 0/1 Knapsack?**

A problem where every item is either selected completely or rejected.

**Q3. Can we take half an item?**

No.

**Q4. What is a DP table?**

A table that stores the results of smaller subproblems.

**Q5. What does dp[i][w] represent?**

The maximum value obtainable using the first i items with capacity w.

**Q6. Why do we use max()?**

To compare the result of including and excluding an item.

**Q7. What is the time complexity?**

O(n × W).

---

# PRACTICAL 5 – N-QUEEN PROBLEM

## 1. Problem Statement

Write a program to solve the N-Queen problem using Backtracking. Place N queens on an N × N chessboard such that no two queens attack each other. Display the resulting chessboard configuration.

## 2. Theory

The N-Queen problem is a classic Backtracking problem.

The objective is to place N queens on an N × N chessboard such that no two queens attack each other.

A queen can attack another queen if both are in the same:

1. Row (horizontal).
2. Column (vertical).
3. Diagonal.

For example, for N = 4, one valid arrangement is:

```text
. Q . .
. . . Q
Q . . .
. . Q .
```

### What is Backtracking?

Backtracking is a problem-solving technique in which we try to build a solution step by step.

If a particular choice does not lead to a valid solution, we go back to the previous step and try another choice.

In the N-Queen problem:

1. Place a queen in a safe position.
2. Move to the next row.
3. Check whether another queen can be placed.
4. If no safe position exists, go back.
5. Remove the previous queen and try another position.
6. Continue until all queens are placed.

## 3. Algorithm

1. Start.
2. Accept the number of queens N.
3. Initialize the column and diagonal tracking arrays.
4. Start placing queens from row 0.
5. Check every column in the current row.
6. If the position is safe:
   - Mark the column as occupied.
   - Mark both diagonals as occupied.
   - Store the queen's position.
7. Recursively move to the next row.
8. If the next row cannot be completed:
   - Remove the previously placed queen.
   - Reset the column and diagonal markers.
   - Try another position.
9. If all queens are successfully placed, display the board.
10. Otherwise, display "No solution exists."
11. Stop.

## 4. Python Code

**Filename:** `practical5.py`

```python
# Function to place queens
def placeQueens(i, cols, leftDiagonal, rightDiagonal, cur):

    n = len(cols)

    if i == n:
        return True

    for j in range(n):

        if (cols[j] or
            rightDiagonal[i + j] or
            leftDiagonal[i - j + n - 1]):

            continue

        cols[j] = 1
        rightDiagonal[i + j] = 1
        leftDiagonal[i - j + n - 1] = 1

        cur.append(j)

        if placeQueens(i + 1, cols,
                       leftDiagonal, rightDiagonal, cur):

            return True

        # Backtracking
        cur.pop()

        cols[j] = 0
        rightDiagonal[i + j] = 0
        leftDiagonal[i - j + n - 1] = 0

    return False


# N Queen Function
def nQueen(n):

    if n <= 0:
        return None

    cols = [0] * n

    leftDiagonal = [0] * (n * 2)

    rightDiagonal = [0] * (n * 2)

    cur = []

    board = [['.' for _ in range(n)] for _ in range(n)]

    if placeQueens(0, cols, leftDiagonal,
                   rightDiagonal, cur):

        for i in range(n):
            board[i][cur[i]] = 'Q'

        return board

    else:
        return None


# Print Chessboard
def printBoard(board):

    if board:
        for row in board:
            print(" ".join(row))

    else:
        print("No solution exists.")


# Main Program
n = int(input("Enter the number of queens:\t"))

board = nQueen(n)

printBoard(board)
```

## 5. Sample Output

For N = 4:

```text
Enter the number of queens: 4

. Q . .
. . . Q
Q . . .
. . Q .
```

For N = 2:

```text
Enter the number of queens: 2

No solution exists.
```

For N = 3:

```text
Enter the number of queens: 3

No solution exists.
```

## 6. How to Execute in Linux

Step 1: Open the terminal.

Step 2: Create the file.

```bash
nano practical5.py
```

Step 3: Paste the code and save.

Step 4: Execute.

```bash
python3 practical5.py
```

Step 5: Enter the number of queens.

```text
4
```

The chessboard arrangement will be displayed.

## 7. Explanation of the Code

### Part A – placeQueens() Function

```python
def placeQueens(i, cols, leftDiagonal, rightDiagonal, cur):
```

Defines a recursive function that attempts to place queens on the chessboard.

Parameters:

| Parameter | Purpose |
|---|---|
| i | Current row |
| cols | Tracks occupied columns |
| leftDiagonal | Tracks one diagonal direction |
| rightDiagonal | Tracks the other diagonal direction |
| cur | Stores queen positions |

### Part B – Base Condition

```python
if i == n:
    return True
```

If the current row becomes equal to N, it means all queens have been successfully placed.

The function returns True.

This is the stopping condition for recursion.

### Part C – Checking Columns

```python
for j in range(n):
```

Checks every column in the current row.

For N = 4, it checks columns 0, 1, 2 and 3.

### Part D – Checking Diagonals

```python
if (cols[j] or
    rightDiagonal[i + j] or
    leftDiagonal[i - j + n - 1]):
```

Checks whether the current position is already occupied by another queen's column or diagonal.

The diagonal indexes are:

rightDiagonal = i + j

leftDiagonal = i - j + n - 1

Adding n - 1 ensures that the left diagonal index is not negative.

If the position is unsafe:

```python
continue
```

Skips the current position and tries the next column.

### Part E – Placing a Queen

```python
cols[j] = 1
rightDiagonal[i + j] = 1
leftDiagonal[i - j + n - 1] = 1
```

Marks the current column and both diagonals as occupied.

```python
cur.append(j)
```

Stores the column position of the queen.

### Part F – Recursive Call

```python
if placeQueens(i + 1, cols,
               leftDiagonal, rightDiagonal, cur):
```

Calls the same function for the next row.

If all queens can be placed, the function returns True.

### Part G – Backtracking

```python
cur.pop()
```

Removes the last queen position.

```python
cols[j] = 0
rightDiagonal[i + j] = 0
leftDiagonal[i - j + n - 1] = 0
```

Marks the column and diagonals as free again.

Why?

Because the previously selected position did not lead to a valid solution.

The algorithm removes that choice and tries another position.

This process is called Backtracking.

### Part H – nQueen() Function

```python
cols = [0] * n
```

Creates a list to track occupied columns.

```python
leftDiagonal = [0] * (n * 2)
rightDiagonal = [0] * (n * 2)
```

Creates two lists to track occupied diagonals.

```python
cur = []
```

Stores the column position of every queen.

```python
board = [['.' for _ in range(n)] for _ in range(n)]
```

Creates an N × N chessboard filled with dots.

```python
if placeQueens(0, cols, leftDiagonal, rightDiagonal, cur):
```

Calls the recursive function to find a valid arrangement.

```python
board[i][cur[i]] = 'Q'
```

Places a queen at its recorded position.

### Part I – printBoard()

```python
if board:
```

Checks whether a valid chessboard exists.

```python
for row in board:
    print(" ".join(row))
```

Prints each row of the chessboard.

If no solution exists:

```python
print("No solution exists.")
```

Displays the message.

## 8. Time and Space Complexity

| Complexity | Value |
|---|---|
| Time Complexity | O(N!) worst-case bound |
| Auxiliary Space | O(N) |
| Chessboard Storage | O(N²) |

The algorithm tries different queen positions and uses backtracking to eliminate invalid arrangements.

## 9. Viva Questions and Answers

**Q1. What is the N-Queen problem?**

Placing N queens on an N × N chessboard such that no two queens attack each other.

**Q2. Which technique is used?**

Backtracking.

**Q3. What is Backtracking?**

A technique that removes a previous choice when it cannot lead to a valid solution.

**Q4. How many directions can a queen move?**

Horizontally, vertically and diagonally.

**Q5. What is the purpose of cur?**

It stores the column positions of the placed queens.

**Q6. What does pop() do?**

Removes the last element from a list.

**Q7. Why do we use two diagonal arrays?**

To track both diagonal directions and quickly check whether a position is safe.

**Q8. Does every N have a solution?**

No. For positive N, solutions exist for N = 1 and all N greater than or equal to 4. There are no solutions for N = 2 and N = 3.

---

# FINAL DAA REVISION SHEET

## 1. Important Algorithms

| Practical | Algorithm / Technique | Main Concept |
|---|---|---|
| Fibonacci | Iteration and Recursion | Loops and function calls |
| Huffman Coding | Greedy | Min Heap and Binary Tree |
| Fractional Knapsack | Greedy | Value/Weight Ratio |
| 0/1 Knapsack | Dynamic Programming | DP Table |
| N-Queen | Backtracking | Recursion and undoing choices |

## 2. Time Complexity Comparison

| Practical | Time Complexity |
|---|---|
| Fibonacci – Iterative | O(n) |
| Fibonacci – Recursive Series | O(n) |
| Huffman Coding | O(k log k + n) |
| Fractional Knapsack | O(n log n) |
| 0/1 Knapsack | O(n × W) |
| N-Queen | O(N!) worst-case bound |

## 3. Greedy vs Dynamic Programming vs Backtracking

| Greedy | Dynamic Programming | Backtracking |
|---|---|---|
| Makes the best local choice | Solves and stores subproblems | Tries choices and reverses failed ones |
| Does not generally reconsider choices | Reuses stored results | Undoes previous choices |
| Huffman Coding | 0/1 Knapsack | N-Queen |
| Fractional Knapsack | | |

## 4. Fractional vs 0/1 Knapsack

| Fractional Knapsack | 0/1 Knapsack |
|---|---|
| Fractions are allowed | Fractions are not allowed |
| Uses Greedy Algorithm | Uses Dynamic Programming |
| Sorts by value-to-weight ratio | Compares include and exclude choices |
| O(n log n) | O(n × W) |

## 5. Five Important Definitions

**1. Recursion**

A programming technique in which a function calls itself.

**2. Greedy Algorithm**

An algorithm that chooses the locally optimal option at each step.

**3. Dynamic Programming**

A method of solving problems by dividing them into smaller subproblems and storing their results.

**4. Backtracking**

A technique that undoes a previous choice when it does not lead to a valid solution.

**5. Time Complexity**

A measure of how an algorithm's execution time grows as the input size increases.

## 6. Linux Command Reference

All five practicals use Python 3.

### Practical 1

```bash
nano practical1.py
python3 practical1.py
```

### Practical 2

```bash
nano practical2.py
python3 practical2.py
```

### Practical 3

```bash
nano practical3.py
python3 practical3.py
```

### Practical 4

```bash
nano practical4.py
python3 practical4.py
```

### Practical 5

```bash
nano practical5.py
python3 practical5.py
```

### Checking Python Installation

```bash
python3 --version
```

If Python 3 is not installed on Ubuntu:

```bash
sudo apt update
sudo apt install python3
```

No C++ compiler or Java installation is required for these five practicals.

---

# IMPORTANT EXAM PREPARATION NOTES

1. Understand the difference between recursion and iteration.
2. Remember that Huffman Coding uses a Min Heap and Greedy Algorithm.
3. Remember that Fractional Knapsack uses the value-to-weight ratio.
4. Understand how the DP table is constructed in 0/1 Knapsack.
5. Understand how backtracking works in N-Queen.
6. Practice executing every program in Linux.
7. Learn the time and space complexity of each practical.
8. Be prepared to explain important variables and functions.
9. Practice the sample inputs and outputs.
10. Do not just memorize the code; understand the logic behind it.

---

# END OF DAA PRACTICAL JOURNAL

**Programming Language:** Python 3  
**Operating System:** Linux / Ubuntu  
**Subject:** Design and Analysis of Algorithms  
**Total Practicals:** 5

