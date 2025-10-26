## Java's Big Dilemma: Why Your Data Is "In a Box" and How to Make It Faster

**Based on the article "Go Primitive in Java, or Go in a Box" by Donald Raab**

If you work with Java, you use "collections" constantly: lists of names, sets of IDs, maps of prices.

But there is a hidden secret at the core of Java that, in certain situations, is **wasting memory** and **slowing down your code**. The creator of the **Eclipse Collections** library, Donald Raab, called this "going in a box"—and he has a powerful solution.

This article is for you, to help you understand what's happening, how **Eclipse Collections** solves the problem *today*, and what the future of Java holds.

---

### 1. Java's Current Problem: "Going in a Box" (Autoboxing)

In Java, there are two types of data:

1.  **Primitives (`int`, `double`, `boolean`):** Fast and light, stored directly in memory.
2.  **Objects (`String`, `Integer`, `Double`):** Flexible, but they have a "header" (metadata) that makes them heavy and they are accessed via references (pointers).

The **Standard Java Collections Framework** (like `ArrayList` or `HashSet`) can only store **Objects**. When you try to put a primitive into a collection, Java performs **Autoboxing**, automatically wrapping the primitive in a corresponding Object *Wrapper* class.

| Primitive (Light) | Object (The Heavy "Box") | The Cost |
| :---: | :---: | :---: |
| `int` (4 bytes) | `Integer` (approx. 16 bytes) | $4\times$ more memory, plus processing time overhead. |

This is the process of "**going in a box**" that Donald Raab criticizes, as it makes your code slow and memory-hungry in massive data processing scenarios.

---

### 2. The Present Solution: **Eclipse Collections**

Donald Raab, the author of the article that inspired us, refused to wait for an official solution and created the **Eclipse Collections** library (formerly known as GS Collections).

#### The Trick Behind the Curtain: Using Pure Primitive Arrays

To bypass autoboxing, Eclipse Collections uses a low-level trick:

1.  **The Principle:** Any Java programmer *could* create their own `MyIntList` class that, internally, uses a simple primitive array: `private int[] data;`. This would eliminate memory waste.
2.  **The "Do It Yourself" Problem:** However, doing this for every primitive type (`int`, `long`, `double`, `char`, etc.), for every collection type (`List`, `Set`, `Map`), and guaranteeing that all of them have the same rich set of features (`filter`, `map`, etc.) that modern Java requires, would be an **exhaustive and error-prone job**.

#### The Decisive Advantage of Eclipse Collections

The value of Eclipse Collections is that it has done this entire complex job for you, completely and professionally.

The library provides:

| Feature | Benefit |
| :--- | :--- |
| **Ready-Made Implementations** | It provides specialized classes like `IntList`, `DoubleSet`, or `LongObjectMap` that already use `int[]`, `double[]`, and `long[]` internally, ensuring maximum memory efficiency and speed. |
| **Rich, Consistent API** | Every primitive collection has a fluent, functional API. It's not limited to `add()` and `get()`; it offers powerful methods like `select()`, `reject()`, and `collect()`, which are consistent with object collections but operate on pure primitives. |
| **Maturity and Testing** | The library has over a decade of development and use in critical environments (like financial services), guaranteeing that it is stable, optimized, and free from the bugs a "hand-rolled" solution would have. |

In short, Eclipse Collections is not just a trick; **it is a complete framework** that allows developers to use modern and powerful Java collection syntax, but with the **speed and lightness of primitive types**.

---

### 3. The Future of Java: **Project Valhalla**

Java has acknowledged this performance issue and has a major project underway to solve it natively and permanently: **Project Valhalla**.

* **The Goal:** To bring "Value Classes" to Java. Practically, this means we will be able to create *classes* that Java treats internally with the efficiency of *primitives*, eliminating the difference between the two.
* **The Promise:** Finally allowing standard Java Collections to efficiently accept `List<int>` (Specialized Generics), solving the autoboxing problem across the entire ecosystem.

Project Valhalla is the future solution and the way the language will evolve. But, as Donald Raab emphasizes:

> **"I'm glad we got to work on supporting primitive collections in Eclipse Collections when I was in my early forties. Now I'm in my mid-fifties, and I have decided I'm getting too old to wait for language miracles to arrive."**

The final lesson is pragmatic: **you don't have to wait**. If your application needs high performance and low memory consumption today, Eclipse Collections offers a mature, tested, and complete solution to "**Go Primitive**".

***
**Reference:** This article is based on the ideas and solution presented by Donald Raab in his Medium article: **"Go Primitive in Java, or Go in a Box"** (available at `https://donraab.medium.com/go-primitive-in-java-or-go-in-a-box-c26f5c6d7574`).