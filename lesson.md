# Lesson 3.4: Data Structures and Algorithms (Part 1)

## Lesson Overview
This lesson introduces fundamental data structures used in Java programming — Arrays, ArrayList, LinkedList, HashMap, and HashSet. Students will learn how these structures organize and store data efficiently, how to perform basic operations on them, and how to understand their performance using time complexity concepts. By mastering these foundational structures, learners gain the skills to design optimized programs that can handle data systematically.

**Module:** 3.4  
**Duration:** 3 hours  
**Prerequisites:** Basic Java syntax, variables, operators, loops, and control flow.

---

## Lesson Objectives

By the end of this lesson, students will be able to:
- Define and differentiate between linear and hash-based data structures.
- Implement and manipulate Arrays, ArrayLists, LinkedLists, HashMaps, and HashSets.
- Apply appropriate data structures for various problem types.
- Analyze the performance of operations using Big O notation.

---

## Part 1: Introduction to Data Structures

Data structures are containers that organize and store data efficiently so it can be accessed and modified effectively. Choosing the right data structure affects how quickly and efficiently a program can process data. For example, retrieving a record from a list of 10 items may seem trivial, but when the same operation scales to millions of records, efficiency becomes critical.

In Java, data structures are broadly divided into two types:
1. **Linear Data Structures** – Elements are arranged sequentially (Arrays, ArrayLists, LinkedLists).
2. **Hash-Based Data Structures** – Elements are stored using hash codes (HashMap, HashSet).

Each structure comes with trade-offs: some provide faster lookups, while others are easier to insert or remove elements.

---

## Part 2: Linear Data Structures

### Arrays

An **Array** is a fixed-size, indexed collection of elements of the same data type. It stores data in contiguous memory locations, which allows direct access using indices. Arrays are ideal for storing a known number of elements but cannot grow dynamically.

```java
public class LearnArrays {
  public static void main(String[] args) {
    int[] productPrices = {999, 499, 299, 199, 129};

    System.out.println("Array elements:");
    for (int i = 0; i < productPrices.length; i++) {
      System.out.println("Index " + i + ": " + productPrices[i]);
    }
  }
}
```

Arrays provide **O(1)** access time because elements can be directly indexed. Arrays are **fixed in size and cannot grow** — you cannot insert new elements. However, shifting values within the existing fixed-size array to place a value at a specific position is **O(n)** since all subsequent elements must move. Searching for a value without knowing its index is also **O(n)** as each element must be checked one by one.

### 👨‍💻 Activity: Working with Arrays **(5 minutes)**
Write a program that stores five product stock quantities in an array and prints:
1. All stock values.
2. The total stock count.
3. The highest and lowest stock values.

---

### ArrayList

An **ArrayList** is a resizable array implementation from the Java Collections Framework. Unlike arrays, its size grows automatically when elements are added. Internally, it still uses an array, but resizing is managed by Java.

```java
import java.util.ArrayList;

public class LearnArrayList {
  public static void main(String[] args) {
    ArrayList<String> productNames = new ArrayList<>();
    productNames.add("Laptop");
    productNames.add("Phone");
    productNames.add("Tablet");

    System.out.println("Product List:");
    for (String item : productNames) {
      System.out.println(item);
    }

    productNames.remove("Phone");
    System.out.println("After removal: " + productNames);
  }
}
```

ArrayLists provide **O(1)** access by index but slower insertion/removal in the middle (**O(n)**). They are preferred when frequent random access is needed.

### 👨‍💻 Activity: ArrayList Operations **(5 minutes)**
Create an ArrayList of five grocery items.  
- Add and remove items.
- Print the total count of items.
- Check whether a specific item exists using `.contains()`.

---

### LinkedList

A **LinkedList** stores elements as nodes, where each node holds data and a reference to the next node. It does not require contiguous memory, allowing faster insertions and deletions than arrays.

```java
import java.util.LinkedList;

public class LearnLinkedList {
  public static void main(String[] args) {
    LinkedList<String> recentlyViewed = new LinkedList<>();

    recentlyViewed.add("Mouse");
    recentlyViewed.add("Keyboard");
    recentlyViewed.add("Monitor");

    System.out.println("Recently viewed: " + recentlyViewed);

    recentlyViewed.addFirst("Charger");
    recentlyViewed.removeLast();

    System.out.println("Updated list: " + recentlyViewed);
  }
}
```

LinkedLists offer **O(1)** insertion and deletion at ends but **O(n)** access because traversal is sequential.

### 👨‍💻 Activity: Simulating Recent Items **(5 minutes)**
Write a LinkedList program to simulate "recently viewed items." Add five items, remove the oldest when adding a sixth, and display the list.

---

## Part 3: Hash-Based Data Structures

### HashMap, LinkedHashMap and TreeMap

A **HashMap** stores data as key–value pairs. Each key is unique and is assigned a hash code that determines where it is stored internally. This allows extremely fast lookups.

All three map types — **HashMap**, **LinkedHashMap**, and **TreeMap** — implement the **Map interface**. They all store key–value pairs and require unique keys, but differ in ordering and performance.

| | HashMap | LinkedHashMap | TreeMap |
|--|---------|---------------|---------|
| Order | No guaranteed order | Maintains insertion order | Sorted by key automatically |
| Speed | O(1) | O(1) | O(log n) |
| Use when | Speed matters | Order matters | Sorted keys needed |

---

#### HashMap — No Order, Fastest Lookup

```java
import java.util.HashMap;

// HashMap implements Map interface
HashMap<String, Integer> scores = new HashMap<>();
scores.put("Alice", 85);
scores.put("Bob", 92);
scores.put("Charlie", 78);

// getOrDefault — returns default value if key doesn't exist
System.out.println(scores.getOrDefault("David", 0)); // Output: 0

// putIfAbsent — only adds if key doesn't already exist
scores.putIfAbsent("Alice", 99); // won't update, Alice already exists
System.out.println(scores.get("Alice")); // Output: 85

System.out.println(scores); // order not guaranteed
```

> **Key rule:** Keys must be unique. Adding the same key again overwrites the existing value. Duplicate values are allowed.

---

#### LinkedHashMap — Insertion Order

```java
import java.util.LinkedHashMap;
import java.util.Map;

// LinkedHashMap implements Map interface
LinkedHashMap<String, Integer> scores = new LinkedHashMap<>();
scores.put("Alice", 85);
scores.put("Bob", 92);
scores.put("Charlie", 78);

// entrySet — iterate over key-value pairs
for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println(entry.getKey() + " scored " + entry.getValue());
}
// Output always in insertion order:
// Alice scored 85
// Bob scored 92
// Charlie scored 78
```

---

#### TreeMap — Sorted by Key

```java
import java.util.TreeMap;

// TreeMap implements Map interface
TreeMap<String, Integer> scores = new TreeMap<>();
scores.put("Charlie", 78);
scores.put("Alice", 85);
scores.put("Bob", 92);

// firstKey / lastKey — unique to TreeMap
System.out.println(scores.firstKey()); // Output: Alice
System.out.println(scores.lastKey());  // Output: Charlie

// headMap — returns all entries strictly less than given key
System.out.println(scores.headMap("Charlie")); // Output: {Alice=85, Bob=92}

System.out.println(scores); // Output: {Alice=85, Bob=92, Charlie=78} always alphabetical
```

---

#### Programming to the Map Interface

```java
// All three implement the same Map interface
// You can swap one for another without changing the rest of your code
Map<String, Integer> map1 = new HashMap<>();       // fast, no order
Map<String, Integer> map2 = new LinkedHashMap<>();  // fast, insertion order
Map<String, Integer> map3 = new TreeMap<>();        // sorted, slightly slower
```

### 👨‍💻 Activity: Working with HashMap **(3 minutes)**
Create a HashMap that stores three students and their scores (e.g. `"Alice" → 85`).
- Retrieve and print the score for one student using `.get()`.
- Update a student's score using `.put()` with the same key.
- Remove one student using `.remove()`.
- Print the final HashMap.

---

## Understanding Time Complexity and Big O Notation

**Time complexity** measures how the runtime of an algorithm grows relative to input size. It allows developers to predict the scalability of their code. The **Big O notation** expresses this growth using mathematical symbols.

| Big O | Name | Meaning | Example Operation |
|-------|------|----------|-------------------|
| O(1) | Constant | Takes the same time regardless of data size | Accessing an element by index in an array |
| O(n) | Linear | Time increases proportionally with data | Searching an unsorted list |
| O(log n) | Logarithmic | Time grows slowly even as data grows | Binary search in a sorted array |
| O(n²) | Quadratic | Time grows rapidly as input increases | Nested loops |

Understanding Big O helps programmers choose the right data structure. For example:
- Accessing an item in an **ArrayList** is O(1).
- Searching an **unsorted array** is O(n).
- Retrieving data from a **HashMap** is typically O(1).

---

### HashSet, LinkedHashSet and TreeSet

A **HashSet** stores unique elements without duplicates. Internally, it uses a HashMap to manage entries efficiently. HashSet is ideal for maintaining a list of distinct values, such as unique user IDs or product SKUs.

All three set types — **HashSet**, **LinkedHashSet**, and **TreeSet** — implement the **Set interface**. They all guarantee uniqueness but differ in ordering and performance.

| | HashSet | LinkedHashSet | TreeSet |
|--|---------|---------------|---------|
| Order | No guaranteed order | Maintains insertion order | Sorted automatically |
| Speed | O(1) | O(1) | O(log n) |
| Use when | Speed + uniqueness | Order + uniqueness | Sorted uniqueness |

---

#### HashSet — No Order, Fastest

```java
import java.util.HashSet;

// HashSet implements Set interface
HashSet<String> categories = new HashSet<>();
categories.add("Electronics");
categories.add("Clothing");
categories.add("Electronics"); // duplicate ignored

// contains — check if element exists O(1)
System.out.println(categories.contains("Clothing")); // true

// size
System.out.println(categories.size()); // 2

// removeIf — remove elements matching a condition
categories.removeIf(c -> c.startsWith("E"));
System.out.println(categories); // [Clothing]
```

---

#### LinkedHashSet — Insertion Order

```java
import java.util.LinkedHashSet;

// LinkedHashSet implements Set interface
LinkedHashSet<String> categories = new LinkedHashSet<>();
categories.add("Electronics");
categories.add("Clothing");
categories.add("Furniture");

// for-each iteration — output always in insertion order
for (String category : categories) {
    System.out.println(category);
}
// Electronics
// Clothing
// Furniture
```

---

#### TreeSet — Sorted

```java
import java.util.TreeSet;

// TreeSet implements Set interface
TreeSet<String> categories = new TreeSet<>();
categories.add("Furniture");
categories.add("Electronics");
categories.add("Clothing");

// first / last — unique to TreeSet
System.out.println(categories.first()); // Clothing
System.out.println(categories.last());  // Furniture

// headSet — elements strictly less than given value
System.out.println(categories.headSet("Furniture")); // [Clothing, Electronics]

// subSet — elements between two values
System.out.println(categories.subSet("Clothing", "Furniture")); // [Clothing, Electronics]

System.out.println(categories); // [Clothing, Electronics, Furniture] always sorted
```

---

#### Stream-Based Iteration — Modern Java Way

> 💡 **Preview:** Streams will be covered in detail in a later lesson. For now, observe how clean and readable this is compared to a traditional for loop.

```java
import java.util.HashSet;

HashSet<String> categories = new HashSet<>();
categories.add("Electronics");
categories.add("Clothing");
categories.add("Furniture");
categories.add("Sports");

// Simple stream iteration
categories.stream()
    .forEach(System.out::println);

// Filter — only categories starting with E or C
categories.stream()
    .filter(c -> c.startsWith("E") || c.startsWith("C"))
    .forEach(System.out::println);
```

---

#### Programming to the Set Interface

```java
// All three implement the same Set interface
Set<String> set1 = new HashSet<>();       // fast, no order
Set<String> set2 = new LinkedHashSet<>();  // fast, insertion order
Set<String> set3 = new TreeSet<>();        // sorted, slightly slower
```

### 👨‍💻 Activity: Removing Duplicates **(3 minutes)**
Create a HashSet from an ArrayList of product names (with duplicates). Display the unique product names after conversion.

---

## Part 4: Comparison and Summary

| Data Structure | Type | Ordered | Allows Duplicates | Access | Insertion/Removal | Typical Use |
|----------------|------|----------|-------------------|---------|-------------------|-------------|
| Array | Linear | Yes | Yes | O(1) | O(n) | Fixed-size sequential data |
| ArrayList | Linear | Yes | Yes | O(1) | O(n) | Dynamic list with fast access |
| LinkedList | Linear | Yes | Yes | O(n) | O(1) at ends | Frequent insert/delete |
| HashMap | Hash-Based | No | Keys unique | O(1) | O(1) | Key–value mappings, fastest lookup |
| LinkedHashMap | Hash-Based | Insertion order | Keys unique | O(1) | O(1) | Key–value mappings, order matters |
| TreeMap | Hash-Based | Sorted by key | Keys unique | O(log n) | O(log n) | Sorted key–value mappings |
| HashSet | Hash-Based | No | No | O(1) | O(1) | Unique collections, fastest |
| LinkedHashSet | Hash-Based | Insertion order | No | O(1) | O(1) | Unique collections, order matters |
| TreeSet | Hash-Based | Sorted | No | O(log n) | O(log n) | Sorted unique collections |

### Key Takeaways
- Arrays are fixed in size but fast for access.
- ArrayLists provide flexibility and are easy to use.
- LinkedLists allow fast insertion and removal but slower random access.
- HashMaps and HashSets rely on hashing for extremely fast lookups.
- LinkedHashMap and LinkedHashSet maintain insertion order at minimal performance cost.
- TreeMap and TreeSet automatically sort data but are slightly slower at O(log n).
- Understanding Big O helps make performance-conscious design choices.

---

**End of Lesson 3.4**