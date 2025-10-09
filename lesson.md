# 3.4 Data Structures and Algorithms — Part 1

## Lesson Overview
Data structures and algorithms form the foundation of efficient software development. A **data structure** defines how data is organized, while an **algorithm** defines the logical steps used to process that data. In modern backend systems, choosing the right structure directly impacts performance and scalability.

In this lesson, we focus on **linear data structures** (Array, ArrayList, LinkedList) and **hash-based data structures** (HashMap, HashSet) using a consistent **Product Inventory** theme — where each `Product` has an `id`, `name`, `price`, and `category`. You’ll learn how to create, manipulate, and compare these structures, and how simple algorithms (searching, counting, deduplication) operate on them.

**Duration:** 3 hours  
**Module:** 3 – Backend Development : Server-Side Programming 
**Lesson:** 3.4  
**Prerequisites:** Java basics (variables, data types), control flow, and methods.

---

## Lesson Objectives
By the end of this lesson, you will be able to:
- **Explain** how Arrays, ArrayLists, LinkedLists, HashMaps, and HashSets organize data.
- **Implement** and **manipulate** these structures using Java 21 features where relevant (`var`, `List.of`, `Map.of`).
- **Apply** simple algorithms (search, count, deduplicate) on these structures.
- **Compare** linear vs. hash-based structures and **choose** the appropriate one for a given use case.

---

## Part 1: Introduction to Data Structures

### What Are Data Structures?
A **data structure** is a strategy for organizing and storing data so that operations like search, insert, update, and delete can be performed efficiently. Different structures optimize for different operations; there is no one-size-fits-all. In a **Product Inventory** system, you might need fast lookup by product ID, quick additions/removals, or protection against duplicates — each need points to a different structure.

### Types Covered in This Lesson
- **Linear structures** — Arrays, ArrayLists, LinkedLists (ordered, sequential access/iteration).
- **Hash-based structures** — HashMaps, HashSets (fast lookups using hashing).

> We’ll practice with a `Product` type: `id` (int), `name` (String), `price` (double), `category` (String).

---

## Part 2: Linear Data Structures

# Linear Data Structures

A **linear data structure** is a type of data structure in which elements are **arranged sequentially**, one after another, and each element is connected to its previous and next element (except the first and last ones).

In a linear data structure, **data elements are stored in a single level (one-dimensional order)**, and they can be traversed in a single run (usually from the first element to the last). The memory allocation for these structures can be **contiguous (arrays)** or **non-contiguous (linked lists)**, but the logical order remains linear.

---

## Key Characteristics
- Elements are **arranged in a sequential order**.  
- Every element has a **unique predecessor and successor**, except the first and last.  
- Traversal is typically **one-way (linear)**.  
- Examples: **Array, Linked List, Stack, Queue**

---

### Arrays
Arrays store elements **contiguously in memory** and provide **indexed** (0-based) random access. They are memory-efficient and fast to read by index, but their size is **fixed**. Insertions/removals in the middle require shifting, which is O(n). Use arrays when the number of elements is known and static (e.g., fixed list of price caps).


#### Example — Product Prices Using Arrays
```java
public class ArrayPricesDemo {
  public static void main(String[] args) {
    double[] prices = { 999.0, 499.5, 299.0, 199.99 };

    // Access by index
    System.out.println("First price: $" + prices[0]);

    // Update
    prices[2] = 279.0;

    // Traverse
    for (int i = 0; i < prices.length; i++) {
      System.out.println("Price[" + i + "] = $" + prices[i]);
    }
  }
}
```

#### Simple Linear Search (Array)
```java
public static int indexOfPrice(double[] prices, double target) {
  for (int i = 0; i < prices.length; i++) {
    if (prices[i] == target) {
      return i;
    }
  }
  return -1; // not found
}
```

#### 👨‍💻 Activity — Array Search
- Create an array of product prices.  
- Ask the user to enter a price.  
- Print the index if found; otherwise print “Price not found.”

---

### ArrayList
An **ArrayList** is a **resizable array**. It keeps elements in order and grows automatically as new items are added. It provides O(1) access by index but slower insertions/removals in the middle due to shifting. ArrayLists are widely used because they are flexible, ordered, and simple to manipulate.

#### Model Class (Product.java)
```java
public class Product {
  public int id;
  public String name;
  public double price;
  public String category;

  public Product(int id, String name, double price, String category) {
    this.id = id;
    this.name = name;
    this.price = price;
    this.category = category;
  }

  @Override
  public String toString() {
    return id + " - " + name + " ($" + price + ", " + category + ")";
  }
}
```

#### Example — Managing a Product Catalog with ArrayList
```java
import java.util.*;

public class ArrayListCatalogDemo {
  public static void main(String[] args) {
    var inventory = new ArrayList<Product>(List.of(
      new Product(101, "Laptop Pro", 1299.0, "Electronics"),
      new Product(102, "Wireless Mouse", 29.99, "Accessories"),
      new Product(103, "Mechanical Keyboard", 89.5, "Accessories")
    ));

    // Add new product
    inventory.add(new Product(104, "4K Monitor", 299.0, "Electronics"));

    // Iterate
    for (var p : inventory) {
      System.out.println(p);
    }

    // Search and update
    for (var p : inventory) {
      if (p.id == 102) {
        p.price = 34.99;
      }
    }

    // Remove discontinued items
    inventory.removeIf(p -> p.price < 30);

    System.out.println("Updated Inventory:");
    inventory.forEach(System.out::println);
  }
}
```

#### 👨‍💻 Activity — ArrayList Manipulation
- Create an ArrayList of 5–6 `Product`s.  
- Add, remove, and update some products.  
- Print the inventory before and after.

---

### LinkedList
A **LinkedList** stores elements in **nodes**. Each node holds data and a reference to the next (and in Java’s `LinkedList`, also previous) node. Insertion/removal at the **head or tail** is efficient; random access by index is **slower**. Use it when you frequently add/remove at ends or use queue/deque patterns.

#### Example — Recently Viewed Products (as a deque)
```java
import java.util.LinkedList;

public class LinkedListRecentDemo {
  public static void main(String[] args) {
    var recent = new LinkedList<String>();

    recent.addFirst("Laptop Pro");
    recent.addFirst("4K Monitor");
    recent.addFirst("Wireless Mouse"); // most recent at front

    System.out.println("Recent views: " + recent);

    // keep only the last 5
    while (recent.size() > 5) {
      recent.removeLast();
    }

    recent.addFirst("Mechanical Keyboard");
    System.out.println("Updated list: " + recent);
  }
}
```

#### 👨‍💻 Activity — Recent History Tracker
Create a `LinkedList<String>` to store up to the 5 most recently viewed products.  
When a 6th item is added, remove the oldest one. Print after each operation.

---

## Part 3: Hash-Based Data Structures

### HashMap
A **HashMap** stores key–value pairs and uses **hashing** to determine the storage bucket. Under typical conditions, `put/get/containsKey/remove` are **O(1)** on average. Collisions are handled internally (linked lists or tree bins). Use `HashMap` for **fast lookup by key**, e.g., `id → Product`.

#### Example — Inventory by Product ID
```java
import java.util.*;

public class HashMapInventoryDemo {
  public static void main(String[] args) {
    var inventory = new HashMap<Integer, Product>(Map.of(
      101, new Product(101, "Laptop Pro", 1299.0, "Electronics"),
      102, new Product(102, "Wireless Mouse", 29.99, "Accessories"),
      103, new Product(103, "Keyboard", 89.5, "Accessories")
    ));
    // Map.of returns an immutable map, but we wrap it in a new HashMap to make it mutable.

    // Add or update
    inventory.put(104, new Product(104, "4K Monitor", 299.0, "Electronics"));
    inventory.put(102, new Product(102, "Wireless Mouse", 34.99, "Accessories")); // updated

    // Retrieve and print
    System.out.println("Fetch by ID 104: " + inventory.get(104));

    // Iterate entries
    for (var e : inventory.entrySet()) {
      System.out.println(e.getKey() + " => " + e.getValue());
    }

    // Remove
    inventory.remove(103);
  }
}
```

#### Algorithm — Count by Category (Frequency Map)
```java
import java.util.*;

public static Map<String, Integer> countByCategory(List<Product> products) {
  var countMap = new HashMap<String, Integer>();
  for (var p : products) {
    countMap.put(p.category, countMap.getOrDefault(p.category, 0) + 1);
  }
  return countMap;
}
```

#### 👨‍💻 Activity — Category Counter
Create a list of 8+ products (mixed categories). Build a `Map<String,Integer>` of **category → count** and print a summary sorted by category.

---

### HashSet
A **HashSet** stores **unique elements** and is backed by a `HashMap` internally (values are dummy). It is optimized for **fast membership checks** and **deduplication**. It does **not** maintain order. Use `HashSet` when you care about uniqueness — e.g., set of categories or SKUs.

#### Example — Unique Categories
```java
import java.util.*;

public class HashSetCategoriesDemo {
  public static void main(String[] args) {
    var categories = List.of(
      "Electronics", "Accessories", "Home", "Accessories", "Home", "Beauty"
    );

    var unique = new HashSet<>(categories);
    System.out.println("Unique categories: " + unique);

    // Membership check
    System.out.println("Has 'Electronics'? " + unique.contains("Electronics"));
  }
}
```

#### Deduplicate Product Names with HashSet
```java
import java.util.*;

public static List<String> uniqueNames(List<String> names) {
  return new ArrayList<>(new HashSet<>(names));
}
```

#### 👨‍💻 Activity — Deduplication
Given a list of product names (with duplicates), remove duplicates using a `HashSet` and print the unique names.  
**Bonus:** Convert back to a List and sort alphabetically.

---

## Part 4: Algorithms on Data Structures
An **algorithm** is a finite, ordered sequence of steps that transforms inputs into outputs. It should be **finite**, **deterministic**, **effective**, and have defined **input** and **output**. Algorithms are what make data structures *useful* — they define how we search, count, or modify data.


### Example A — Find Product by ID (List)
```java
import java.util.*;

public static Product findById(List<Product> list, int id) {
  for (var p : list) {
    if (p.id == id) return p;
  }
  return null;
}
```

### Example B — Count by Category (List → HashMap)
```java
import java.util.*;

public static Map<String, Integer> summarizeCategories(List<Product> list) {
  var counts = new HashMap<String, Integer>();
  for (var p : list) {
    counts.put(p.category, counts.getOrDefault(p.category, 0) + 1);
  }
  return counts;
}
```

### Example C — Remove Discontinued (ArrayList)
```java
import java.util.*;

public static void removeDiscontinued(List<Product> inventory) {
  inventory.removeIf(p -> p.price <= 0.0); // treat <=0 as discontinued
}
```

---

## Part 5: Comparison and Summary

### Quick Comparison

| Data Structure | Category   | Ordered | Duplicates | Key Strength                    | Typical Use Case                         |
|----------------|------------|---------|------------|----------------------------------|------------------------------------------|
| Array          | Linear     | Yes     | Yes        | Fast index access; compact       | Fixed-size lists (e.g., static price tiers) |
| ArrayList      | Linear     | Yes     | Yes        | Resizable; easy to use; indexed  | Dynamic catalogs; general list scenarios |
| LinkedList     | Linear     | Yes     | Yes        | Fast add/remove at ends          | Recent history; queues/deques            |
| HashMap        | Hash-based | No      | Keys uniq. | Fast key lookup; key→value map   | `id → Product`, category counts          |
| HashSet        | Hash-based | No      | No         | Uniqueness; fast membership      | Unique categories; deduplication         |

### Summary
- **Linear structures** preserve order and support straightforward iteration; use **ArrayList** for most lists, **Array** for fixed-size, and **LinkedList** for frequent end-insert/remove patterns.  
- **Hash-based structures** provide **fast lookup**: use **HashMap** for key→value associations and **HashSet** for uniqueness.  
- Algorithms such as **search**, **count**, and **deduplicate** are natural operations over these structures and prepare you for deeper topics like sorting, searching efficiency (Big-O), and trees/graphs in later lessons.

---


