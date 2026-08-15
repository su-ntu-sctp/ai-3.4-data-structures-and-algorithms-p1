# Lesson 3.4: Data Structures and Algorithms (Part 1)

## Lesson Overview
This lesson introduces the data structures you will use every day in Java — Arrays, ArrayList, LinkedList, ArrayDeque, HashMap, and HashSet, along with their ordered and sorted variants. Students will learn how these structures organise and store data, how to choose the right one for a given problem, and how modern Java (Java 17 and 21) has simplified working with them. By the end of this lesson, learners will be able to select and use the appropriate data structure with confidence.

**Module:** 3.4
**Duration:** 3 hours
**Prerequisites:** Basic Java syntax, variables, operators, loops, control flow, classes and objects.

---

## Lesson Objectives

By the end of this lesson, students will be able to:
- Describe the Java Collections Framework and the difference between an interface and an implementation.
- Differentiate between linear and hash-based data structures.
- Implement and manipulate Arrays, ArrayLists, LinkedLists, ArrayDeques, HashMaps, and HashSets.
- Use records to store data safely in Sets and as Map keys.
- Apply the appropriate data structure for a given problem.

---

## Part 1: Introduction to Data Structures

Data structures are containers that organise and store data so it can be accessed and modified efficiently. Choosing the right data structure affects how quickly your program can process data. Retrieving a record from a list of 10 items is trivial, but the same operation across millions of records is where the choice starts to matter.

Data structures fall into **three categories**:

| Category | Structures | How data is stored |
|----------|-----------|-------------------|
| **Linear** | Array, ArrayList, LinkedList, ArrayDeque | Sequentially, one after another |
| **Hash-Based** | HashMap, HashSet and their variants | By hash code, no sequence |
| **Non-Linear** | Tree, Binary Tree, Graph | Hierarchically or as a network |

Hash-based structures are their own category. They are neither linear nor non-linear, because elements are placed according to their hash code rather than by position or by hierarchy.

**This lesson covers linear and hash-based structures. Non-linear structures are covered in Lesson 3.5.**

---

### A Quick Word on Performance

Throughout this lesson you will see notations like **O(1)** and **O(n)**. These are shorthand for how an operation behaves as your data grows:

- **O(1)** — the same speed regardless of how much data you have. Like flipping to a bookmarked page. The size of the book does not matter.
- **O(n)** — the time grows in proportion to the data. Double the data, double the time. Like checking every locker in a hallway one by one.
- **O(log n)** — grows, but very slowly. Like finding a word in a dictionary, where each guess eliminates half the remaining pages.

That is all you need for today. This is vocabulary, not mathematics — we use it to compare structures quickly. Lesson 3.5 goes into this in more depth.

---

## Part 2: The Java Collections Framework

Java already provides a ready-made toolbox of data structures. You do not need to build a resizable list or a hash table yourself — Java has written, tested, and optimised them for you. This toolbox is called the **Java Collections Framework**, often shortened to JCF.

Everything in this lesson except plain arrays comes from this framework.

### Interfaces and Implementations

The Collections Framework is organised into two layers.

**Interfaces** describe *what a structure does*. `List`, `Set`, `Queue`, and `Map` are interfaces. Think of an interface as a category of behaviour:

- `List` — holds elements in order, duplicates allowed
- `Set` — holds unique elements only
- `Queue` — holds elements for processing in a particular order
- `Map` — holds key and value pairs

**Classes** are the actual implementations you create objects from. `ArrayList`, `LinkedList`, `HashSet`, `TreeMap`, `ArrayDeque` are all classes. These are what you import and instantiate.

> **Note:** Interfaces are covered fully in a later lesson. For now, the practical rule is enough: an interface is the category, a class is the thing you actually create with `new`.

### The Structure

```
Iterable
   └── Collection
         ├── List   →  ArrayList, LinkedList
         ├── Set    →  HashSet, LinkedHashSet, TreeSet
         └── Queue  →  ArrayDeque, LinkedList

Map (separate from Collection)
   └── HashMap, LinkedHashMap, TreeMap
```

Notice that `Map` sits outside the `Collection` tree. This is deliberate — a `Collection` holds single elements, while a `Map` holds pairs of elements. They are related in purpose but not in structure.

### Declaring by Interface

In professional Java code, you declare the variable using the **interface** and create the object using the **class**:

```java
List<String> products = new ArrayList<>();
Map<String, Integer> stock = new HashMap<>();
Set<String> categories = new HashSet<>();
```

The advantage is flexibility. If you later decide `LinkedList` suits your access pattern better, you change one word at the point of creation. Every other line of code that uses that variable continues to work unchanged.

You will see this pattern in every production Java codebase.

### Modern Shorthand: `var`

Since Java 10, you can let Java infer the type of a local variable:

```java
var products = new ArrayList<String>();
```

This is common in modern codebases. It is only for local variables inside methods, and the type must still be clear from the right hand side. We will use it occasionally so you recognise it when you see it.

---

## Part 3: Linear Data Structures

### Arrays

An **Array** is a fixed-size, indexed collection of elements of the same data type. It stores data in contiguous memory, which allows direct access by index.

```java
int[] productPrices = {999, 499, 299, 199, 129};

for (int i = 0; i < productPrices.length; i++) {
    System.out.println("Index " + i + ": " + productPrices[i]);
}
```

Arrays give **O(1)** access because the position of any element can be calculated directly. The trade-off is rigidity — the size is fixed at creation and cannot grow. Searching for a value without knowing its index requires checking each element, which is **O(n)**.

**Two things worth knowing:**

Arrays use `.length` — a property, with no brackets. Collections use `.size()` — a method, with brackets. This catches people out regularly.

An array is an object. When you write `new int[5]`, an array object is created on the **heap**, and the variable holding it stores a reference to that object. This is the same model used for every object in Java.

---

### ArrayList

An **ArrayList** is a resizable list. Internally it is still backed by an array, but Java manages the growing for you.

When the internal array fills up, Java creates a new larger array (roughly 1.5 times the size) and copies the elements across. This copy is **O(n)**, but it happens rarely. Averaged across many additions, `add()` is described as **O(1) amortised**.

If you know roughly how many elements you will add, you can avoid repeated resizing:

```java
List<String> products = new ArrayList<>(10000);
```

#### Core Operations

```java
List<String> products = new ArrayList<>();
products.add("Laptop");
products.add("Phone");
products.add("Tablet");

System.out.println(products.get(0));        // Laptop
System.out.println(products.size());        // 3
System.out.println(products.contains("Phone")); // true

products.remove("Phone");
System.out.println(products);               // [Laptop, Tablet]
```

#### Beyond the Basics

These are the methods you will actually reach for in real code.

```java
List<String> products = new ArrayList<>();
products.add("Laptop");
products.add("Phone");
products.add("Tablet");
products.add("Monitor");

// set — replace the element at a position
products.set(1, "Smartphone");
System.out.println(products);          // [Laptop, Smartphone, Tablet, Monitor]

// add at a specific index — shifts everything after it to the right
products.add(2, "Keyboard");
System.out.println(products);          // [Laptop, Smartphone, Keyboard, Tablet, Monitor]

// indexOf — find the position of a value, returns -1 if not present
System.out.println(products.indexOf("Tablet"));   // 3
System.out.println(products.indexOf("Printer"));  // -1

// subList — a view of part of the list
System.out.println(products.subList(0, 3));  // [Laptop, Smartphone, Keyboard]

// addAll — append another collection
products.addAll(List.of("Mouse", "Webcam"));

// removeIf — remove everything matching a condition
products.removeIf(p -> p.startsWith("M"));
System.out.println(products);

// sort the list alphabetically
Collections.sort(products);
System.out.println(products);

// reverse order
Collections.sort(products, Collections.reverseOrder());
```

> **Note on `removeIf()`:** the part inside the brackets, `p -> p.startsWith("M")`, is a **lambda**. Read it as "for each element p, is it true that p starts with M". Lambdas are covered properly in a later lesson. For now, recognise the shape.

#### Creating Lists

```java
// Empty and mutable
List<String> a = new ArrayList<>();

// Fixed and unchangeable — Java 9 onwards
List<String> b = List.of("Laptop", "Phone");

// Mutable copy of a fixed list
List<String> c = new ArrayList<>(List.of("Laptop", "Phone"));
```

`List.of()` creates an **immutable** list. Calling `.add()` on it throws an `UnsupportedOperationException` at runtime. This is intentional — immutable collections are safer to pass around because nothing can modify them unexpectedly. When you need to modify, wrap it in `new ArrayList<>()` as shown above.

---

### 👨‍💻 Activity 1: Managing a Product List **(10 minutes)**

Create an `ArrayList` of product names and use it to do the following:

1. Add at least six products to the list.
2. Replace the product at index 2 with a different name using `set()`.
3. Insert a new product at index 0 so it becomes the first item.
4. Print the position of one specific product using `indexOf()`.
5. Print only the first three products using `subList()`.
6. Remove all products whose name is longer than 7 characters using `removeIf()`.
7. Sort the remaining list alphabetically and print it.

---

### LinkedList

A **LinkedList** stores elements as **nodes**. Each node holds the data and a reference to the next node. The nodes do not need to sit next to each other in memory — Java follows the chain of references.

```java
LinkedList<String> recentlyViewed = new LinkedList<>();

recentlyViewed.add("Mouse");
recentlyViewed.add("Keyboard");
recentlyViewed.add("Monitor");

recentlyViewed.addFirst("Charger");
recentlyViewed.removeLast();

System.out.println(recentlyViewed);   // [Charger, Mouse, Keyboard]
```

`addFirst()` and `removeLast()` are **O(1)** — adding or removing at either end only requires updating a couple of references, with no shifting.

The trade-off is access. Getting the element at position 5 means starting at the head and walking node by node, which is **O(n)**.

> **Note:** LinkedList checks whether your index is nearer the head or the tail and traverses from the closer side. So `get(0)` and the last element are effectively instant. It is the middle that costs you.

---

### ArrayDeque

A **Deque** (pronounced "deck") is a double-ended queue — you can add and remove from both ends. `ArrayDeque` is the standard implementation.

```java
Deque<String> recentlyViewed = new ArrayDeque<>();

recentlyViewed.addLast("Mouse");
recentlyViewed.addLast("Keyboard");
recentlyViewed.addLast("Monitor");

System.out.println(recentlyViewed.peekFirst());   // Mouse — look without removing
System.out.println(recentlyViewed.pollFirst());   // Mouse — remove and return
System.out.println(recentlyViewed);               // [Keyboard, Monitor]

recentlyViewed.addFirst("Charger");
System.out.println(recentlyViewed);               // [Charger, Keyboard, Monitor]
```

**Common methods:**

| Method | What it does |
|--------|-------------|
| `addFirst()` / `addLast()` | Add to the front or the back |
| `peekFirst()` / `peekLast()` | Look at an element without removing it |
| `pollFirst()` / `pollLast()` | Remove and return an element |
| `push()` / `pop()` | Stack behaviour — add and remove from the front |

#### Which One Should You Use?

This matters in real code:

- **`Stack`** is a legacy class from Java 1.0. It is synchronised, which makes it slower, and it is no longer recommended. Do not use it in new code.
- **`LinkedList`** works as a queue, but because its nodes are scattered in memory, the processor cannot cache them efficiently. In practice it is usually slower than the alternative.
- **`ArrayDeque`** is the current recommendation for stack, queue, and deque behaviour. It is backed by an array, which the processor handles far better.

The practical rule: **if you need a stack or a queue, reach for `ArrayDeque`.**

---

## Part 4: Hash-Based Data Structures

### HashMap, LinkedHashMap and TreeMap

A **HashMap** stores data as **key and value** pairs. Each key is unique. The key is passed through a hash function which produces a number, and that number determines where the value is stored internally. This is what makes lookups fast regardless of how much data you have.

All three map types implement the `Map` interface. They store key and value pairs and require unique keys. They differ in ordering and speed.

| | HashMap | LinkedHashMap | TreeMap |
|--|---------|---------------|---------|
| Order | No guaranteed order | Insertion order | Sorted by key |
| Speed | O(1) | O(1) | O(log n) |
| Use when | Speed matters | Order matters | Sorted keys needed |

---

#### HashMap

```java
Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 85);
scores.put("Bob", 92);
scores.put("Charlie", 78);

// getOrDefault — returns a fallback if the key is missing
System.out.println(scores.getOrDefault("David", 0));   // 0

// putIfAbsent — only adds if the key is not already there
scores.putIfAbsent("Alice", 99);
System.out.println(scores.get("Alice"));               // 85, unchanged

System.out.println(scores);   // order not guaranteed
```

> **Key rule:** keys must be unique. Putting the same key again **overwrites** the existing value rather than creating a second entry. Values may repeat freely.

#### Iterating a Map

```java
for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println(entry.getKey() + " scored " + entry.getValue());
}
```

`entrySet()` gives you each pair as a `Map.Entry`, so you get both the key and the value in one pass. You can also use `keySet()` for keys only or `values()` for values only.

#### `computeIfAbsent()`

This one is worth learning properly because it appears constantly in real code.

Suppose you want to group products by category. Without `computeIfAbsent()` you have to check whether the list exists first:

```java
Map<String, List<String>> byCategory = new HashMap<>();

// The long way
if (!byCategory.containsKey("Electronics")) {
    byCategory.put("Electronics", new ArrayList<>());
}
byCategory.get("Electronics").add("Laptop");
```

With `computeIfAbsent()`, that becomes one line:

```java
Map<String, List<String>> byCategory = new HashMap<>();

byCategory.computeIfAbsent("Electronics", k -> new ArrayList<>()).add("Laptop");
byCategory.computeIfAbsent("Electronics", k -> new ArrayList<>()).add("Phone");
byCategory.computeIfAbsent("Furniture", k -> new ArrayList<>()).add("Desk");

System.out.println(byCategory);
// {Electronics=[Laptop, Phone], Furniture=[Desk]}
```

Read it as: "get the list for this key, and if there isn't one yet, create an empty list first — then add to it."

This pattern — a Map whose values are Lists — is extremely common. Any time you need to group things, this is the idiom.

---

#### LinkedHashMap

```java
Map<String, Integer> scores = new LinkedHashMap<>();
scores.put("Alice", 85);
scores.put("Bob", 92);
scores.put("Charlie", 78);

for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println(entry.getKey() + " scored " + entry.getValue());
}
// Always in insertion order:
// Alice scored 85
// Bob scored 92
// Charlie scored 78
```

---

#### TreeMap

```java
TreeMap<String, Integer> scores = new TreeMap<>();
scores.put("Charlie", 78);
scores.put("Alice", 85);
scores.put("Bob", 92);

System.out.println(scores.firstKey());          // Alice
System.out.println(scores.lastKey());           // Charlie
System.out.println(scores.headMap("Charlie"));  // {Alice=85, Bob=92}

System.out.println(scores);   // {Alice=85, Bob=92, Charlie=78} always sorted
```

`firstKey()`, `lastKey()`, and `headMap()` exist only on TreeMap, because only TreeMap guarantees an order to work with.

By default TreeMap sorts by **natural order** — alphabetical for Strings, numerical for numbers.

---

### 👨‍💻 Activity 2: Grouping Products by Category **(10 minutes)**

Create a `Map<String, List<String>>` to group product names under their category.

1. Using `computeIfAbsent()`, add at least six products across three categories.
2. Print the whole map.
3. Print just the products in one category using `get()`.
4. Use `getOrDefault()` to look up a category that does not exist, and show that it returns an empty list rather than `null`.
5. Change your `HashMap` to a `LinkedHashMap` and run it again. Describe what changed in the output.

---

### HashSet, LinkedHashSet and TreeSet

A **HashSet** stores unique elements. Duplicates are silently ignored. Internally a HashSet is built on top of a HashMap — each element you add becomes a key in that map, and the value is an unused placeholder. That is the whole mechanism.

| | HashSet | LinkedHashSet | TreeSet |
|--|---------|---------------|---------|
| Order | No guaranteed order | Insertion order | Sorted |
| Speed | O(1) | O(1) | O(log n) |
| Use when | Speed and uniqueness | Order and uniqueness | Sorted uniqueness |

---

#### HashSet

```java
Set<String> categories = new HashSet<>();
categories.add("Electronics");
categories.add("Clothing");
categories.add("Electronics");   // duplicate, silently ignored

System.out.println(categories.contains("Clothing"));   // true
System.out.println(categories.size());                 // 2

categories.removeIf(c -> c.startsWith("E"));
System.out.println(categories);                        // [Clothing]
```

Notice that adding a duplicate does not throw an error. Uniqueness is enforced automatically — you never need to check `contains()` before adding.

#### Set Operations

```java
Set<String> allCategories = new LinkedHashSet<>(
    List.of("Electronics", "Clothing", "Furniture", "Sports"));

Set<String> activeCategories = Set.of("Clothing", "Sports");

// retainAll — keep only what appears in both
Set<String> active = new LinkedHashSet<>(allCategories);
active.retainAll(activeCategories);
System.out.println(active);        // [Clothing, Sports]

// removeAll — remove everything that appears in the other set
Set<String> inactive = new LinkedHashSet<>(allCategories);
inactive.removeAll(activeCategories);
System.out.println(inactive);      // [Electronics, Furniture]

// addAll — merge, duplicates ignored
Set<String> combined = new LinkedHashSet<>(allCategories);
combined.addAll(Set.of("Books", "Clothing"));
System.out.println(combined);
```

`retainAll()`, `removeAll()`, and `addAll()` give you set intersection, difference, and union without writing any loops.

---

#### TreeSet

```java
TreeSet<String> categories = new TreeSet<>(
    List.of("Furniture", "Electronics", "Clothing"));

System.out.println(categories.first());                    // Clothing
System.out.println(categories.last());                     // Furniture
System.out.println(categories.headSet("Furniture"));       // [Clothing, Electronics]
System.out.println(categories.subSet("Clothing", "Furniture")); // [Clothing, Electronics]

System.out.println(categories);   // [Clothing, Electronics, Furniture] always sorted
```

---

## Part 5: Working With Your Own Objects

Everything so far has used Strings and numbers. Real applications store your own objects — products, customers, orders. This introduces something you need to know about.

### The Problem

Suppose you write a simple class to hold a product:

```java
class Product {
    String id;
    String name;
    double price;

    Product(String id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }
}
```

Now put two identical products into a Set:

```java
Set<Product> products = new HashSet<>();
products.add(new Product("P1", "Laptop", 999.0));
products.add(new Product("P1", "Laptop", 999.0));   // exactly the same data

System.out.println(products.size());   // 2 — the duplicate was NOT removed
```

The Set contains both. Same id, same name, same price — but Java treated them as two different things.

**Why?** Because by default, Java compares objects by their **memory address**, not by their contents. Two separately created objects live at two different addresses, so Java considers them different. The Set's uniqueness check never had a chance.

To fix this properly, a class must define two methods: `equals()`, which says what makes two objects equal, and `hashCode()`, which produces the number the Set and Map use to place the object. Writing these correctly by hand is fiddly and easy to get wrong.

### The Modern Solution: Records

Since Java 16, you can declare a **record** instead. A record is a class whose only job is to hold data. Java generates the constructor, the getters, `toString()`, `equals()`, and `hashCode()` for you, correctly.

```java
record Product(String id, String name, double price) { }
```

That single line replaces the whole class above. Now:

```java
Set<Product> products = new HashSet<>();
products.add(new Product("P1", "Laptop", 999.0));
products.add(new Product("P1", "Laptop", 999.0));

System.out.println(products.size());   // 1 — duplicate correctly removed
```

The duplicate disappears, because the record's generated `equals()` compares the actual values.

### Using Records

```java
record Product(String id, String name, double price) { }

Product laptop = new Product("P1", "Laptop", 999.0);

System.out.println(laptop.name());    // Laptop  — note: name(), not getName()
System.out.println(laptop.price());   // 999.0
System.out.println(laptop);           // Product[id=P1, name=Laptop, price=999.0]
```

Records are **immutable** — once created, the values cannot be changed. This is a feature, not a limitation. It makes them safe to use as Map keys and Set elements, because a key whose value can change underneath you causes very hard bugs.

> **The practical rule:** if you are storing your own objects in a `Set`, or using them as keys in a `Map`, make them a `record`. You get correct behaviour for free.

---

### 👨‍💻 Activity 3: The Product Catalogue **(15 minutes)**

1. Create a `record Product(String id, String name, double price)`.
2. Create a `List<Product>` and add at least six products. Include **two entries that are exactly identical**.
3. Print the list and confirm the duplicate is present.
4. Create a `Set<Product>` from your list. Print it and confirm the duplicate has been removed automatically.
5. Build a `Map<String, Product>` where the key is the product id, so any product can be looked up instantly by id. Print the product for one id.
6. Group your products into a `Map<String, List<Product>>` by a category of your choosing, using `computeIfAbsent()`.

---

## 🔵 Optional: Sorting With Comparators

> Cover this if time allows.

TreeMap and TreeSet sort by natural order. To sort by something else, you supply a **Comparator**.

```java
record Product(String id, String name, double price) { }

List<Product> products = new ArrayList<>(List.of(
    new Product("P1", "Laptop", 999.0),
    new Product("P2", "Phone", 499.0),
    new Product("P3", "Tablet", 499.0)
));

// Sort by price, lowest first
products.sort(Comparator.comparing(Product::price));

// Sort by price, then by name where prices are equal
products.sort(Comparator.comparing(Product::price)
                        .thenComparing(Product::name));

// Highest price first
products.sort(Comparator.comparing(Product::price).reversed());
```

`Product::price` is a **method reference**. It is shorthand for "take each product and get its price". Comparators and method references are covered fully in a later lesson — for now, recognise the pattern, because this is how sorting is written in modern Java.

---

## 🔵 Optional: Sequenced Collections (Java 21)

> Cover this if time allows.

Java 21 added a shared set of methods to collections that have a defined order — `List`, `LinkedHashMap`, and `LinkedHashSet`.

```java
List<String> products = new ArrayList<>(List.of("Laptop", "Phone", "Tablet"));

System.out.println(products.getFirst());   // Laptop
System.out.println(products.getLast());    // Tablet
System.out.println(products.reversed());   // [Tablet, Phone, Laptop]
```

Previously you had to write `products.get(0)` and `products.get(products.size() - 1)`. These methods make the intent clearer and work consistently across ordered collections.

---

## Part 6: Comparison and Summary

| Data Structure | Type | Ordered | Duplicates | Fast at | Slower at |
|----------------|------|---------|------------|---------|-----------|
| Array | Linear | Yes | Yes | Access by index | Fixed size, cannot grow |
| ArrayList | Linear | Yes | Yes | Access by index | Inserting in the middle |
| LinkedList | Linear | Yes | Yes | Adding and removing at the ends | Access by index |
| ArrayDeque | Linear | Yes | Yes | Adding and removing at both ends | Access by index |
| HashMap | Hash-Based | No | Keys unique | Lookup by key | No ordering |
| LinkedHashMap | Hash-Based | Insertion order | Keys unique | Lookup by key, keeps order | Slightly more memory |
| TreeMap | Hash-Based | Sorted by key | Keys unique | Sorted key access | Slower than HashMap |
| HashSet | Hash-Based | No | No | Checking membership | No ordering |
| LinkedHashSet | Hash-Based | Insertion order | No | Membership, keeps order | Slightly more memory |
| TreeSet | Hash-Based | Sorted | No | Sorted unique access | Slower than HashSet |

---

### Choosing a Data Structure

Ask these questions in order:

1. **Do I look things up by a key?** → Use a `Map`.
2. **Do I need every element to be unique?** → Use a `Set`.
3. **Do I need to add and remove from the ends?** → Use `ArrayDeque`.
4. **Otherwise** → Use a `List`, and `ArrayList` unless you have a reason not to.

Then, if you chose a Map or a Set:

- No ordering needed → `HashMap` / `HashSet`
- Insertion order needed → `LinkedHashMap` / `LinkedHashSet`
- Sorted order needed → `TreeMap` / `TreeSet`

**Sensible defaults:** `ArrayList` and `HashMap` cover the large majority of everyday cases. Choose something else only when you have a specific reason.

---

### Key Takeaways
- The Java Collections Framework provides ready-made, tested data structures. Declare by interface, create by class.
- Arrays are fixed size and fast for indexed access. ArrayList adds flexibility on top of an array.
- LinkedList and ArrayDeque are built for adding and removing at the ends. Prefer `ArrayDeque` over `Stack`.
- HashMap and HashSet use hashing for fast lookup, at the cost of ordering.
- LinkedHashMap and LinkedHashSet preserve insertion order. TreeMap and TreeSet keep data sorted.
- Use a `record` for objects stored in Sets or used as Map keys — it gives you correct `equals()` and `hashCode()` automatically.
- Default to `ArrayList` and `HashMap`, and change only when you have a reason.

---

**End of Lesson 3.4**