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

Arrays provide **O(1)** access time because elements can be directly indexed, but searching or inserting in the middle is **O(n)** since elements must be shifted.

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
Write a LinkedList program to simulate “recently viewed items.” Add five items, remove the oldest when adding a sixth, and display the list.

---

## Part 3: Hash-Based Data Structures

### HashMap

A **HashMap** stores data as key–value pairs. Each key is unique and is assigned a hash code that determines where it is stored internally. This allows extremely fast lookups.

```java
import java.util.HashMap;

public class LearnHashMap {
  public static void main(String[] args) {
    HashMap<Integer, String> inventory = new HashMap<>();

    inventory.put(101, "Laptop");
    inventory.put(102, "Phone");
    inventory.put(103, "Tablet");

    System.out.println("All items: " + inventory);
    System.out.println("Item with ID 102: " + inventory.get(102));

    inventory.remove(103);
    System.out.println("Updated inventory: " + inventory);
  }
}
```

A HashMap provides **O(1)** average-time complexity for insertion and retrieval operations due to hashing, though in rare cases (hash collisions), performance can degrade to **O(n)**.

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


### HashSet

A **HashSet** stores unique elements without duplicates. Internally, it uses a HashMap to manage entries efficiently. HashSet is ideal for maintaining a list of distinct values, such as unique user IDs or product SKUs.

```java
import java.util.HashSet;

public class LearnHashSet {
  public static void main(String[] args) {
    HashSet<String> categories = new HashSet<>();
    categories.add("Electronics");
    categories.add("Clothing");
    categories.add("Electronics"); // duplicate ignored

    System.out.println("Categories: " + categories);
  }
}
```

HashSet provides **O(1)** insertion and lookup on average.

### 👨‍💻 Activity: Removing Duplicates **(3 minutes)**
Create a HashSet from an ArrayList of product names (with duplicates). Display the unique product names after conversion.

---

## Part 4: Comparison and Summary

| Data Structure | Type | Ordered | Allows Duplicates | Access | Insertion/Removal | Typical Use |
|----------------|------|----------|-------------------|---------|-------------------|-------------|
| Array | Linear | Yes | Yes | O(1) | O(n) | Fixed-size sequential data |
| ArrayList | Linear | Yes | Yes | O(1) | O(n) | Dynamic list with fast access |
| LinkedList | Linear | Yes | Yes | O(n) | O(1) at ends | Frequent insert/delete |
| HashMap | Hash-Based | No | Keys unique | O(1) | O(1) | Key–value mappings |
| HashSet | Hash-Based | No | No | O(1) | O(1) | Unique collections |

### Key Takeaways
- Arrays are fixed in size but fast for access.
- ArrayLists provide flexibility and are easy to use.
- LinkedLists allow fast insertion and removal but slower random access.
- HashMaps and HashSets rely on hashing for extremely fast lookups.
- Understanding Big O helps make performance-conscious design choices.

---

**End of Lesson 3.4**
