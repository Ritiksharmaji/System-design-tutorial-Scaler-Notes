### ========== Leacture-13-LLD-Design-Patterns-and-Registry ====
## 1. Prototype Design Pattern (Complete Guide) ===
Great 🔥 — now let’s master the **Prototype Design Pattern** in a **deep + interview-ready + real-world way**.

---

# 🚀 Prototype Design Pattern (Complete Guide)

---

## 🧠 1. What is Prototype Pattern?

👉 **Definition:**

> Prototype pattern is used to **create new objects by copying (cloning) an existing object**, instead of creating from scratch.

---

## 🎯 2. Core Idea (VERY IMPORTANT)

👉 Instead of:

```java
new Object(); ❌
```

👉 We do:

```java
object.clone(); ✅
```

---

## 🔥 3. Why Do We Need It?

---

### ❌ Problem Without Prototype

```java
User user1 = new User("Ritik", "Admin");

// Need similar object
User user2 = new User("Ritik", "Admin"); // duplicate creation
```

👉 Issues:

* Expensive creation ❌
* Repeated logic ❌
* Hard for complex objects ❌

---

### ✅ Solution with Prototype

```java
User user2 = user1.clone();
```

✔ Fast
✔ Reusable
✔ Clean

---

## ⚡ 4. Real-Life Analogy

---

👉 Think of **photocopy machine 🖨️**

* Original document = Prototype
* Copies = New objects

✔ You don’t rewrite document every time

---

## 🧱 5. Types of Cloning (VERY IMPORTANT)

---

## 🔹 1. Shallow Copy

👉 Copies only references

```java
User u2 = u1.clone();
```

👉 If object contains:

```java
List<String> roles;
```

❌ Both objects share same list

---

## 🔹 2. Deep Copy

👉 Copies everything (new memory)

✔ Fully independent object

---

## ⚔️ Difference

| Feature     | Shallow Copy | Deep Copy |
| ----------- | ------------ | --------- |
| Memory      | Shared       | Separate  |
| Safe        | ❌            | ✅         |
| Performance | Fast         | Slower    |

---

## 🏗️ 6. Java Implementation (Step-by-Step)

---

### 🔹 Step 1: Implement `Cloneable`

```java
class User implements Cloneable {
    String name;
    String role;

    public User(String name, String role) {
        this.name = name;
        this.role = role;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone(); // shallow copy
    }
}
```

---

### 🔹 Step 2: Use Clone

```java
User u1 = new User("Ritik", "Admin");
User u2 = (User) u1.clone();

System.out.println(u1 == u2); // false
```

✔ Different objects
✔ Same data

---

## 🔥 7. Deep Copy Example (IMPORTANT)

```java
class User implements Cloneable {
    String name;
    List<String> roles;

    public User(String name, List<String> roles) {
        this.name = name;
        this.roles = roles;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        List<String> newRoles = new ArrayList<>(this.roles);
        return new User(this.name, newRoles);
    }
}
```

✔ Now objects are independent

---

## 🧠 8. Prototype Registry (VERY ADVANCED)

👉 Store pre-created objects and clone them

```java
import java.util.HashMap;
import java.util.Map;

class PrototypeRegistry {
    private static Map<String, User> map = new HashMap<>();

    static {
        map.put("ADMIN", new User("Default", List.of("READ", "WRITE")));
        map.put("USER", new User("Default", List.of("READ")));
    }

    public static User get(String type) throws CloneNotSupportedException {
        return (User) map.get(type).clone();
    }
}
```

---

### ✅ Usage

```java
User admin = PrototypeRegistry.get("ADMIN");
```

✔ Fast object creation
✔ No repeated logic

---

## 🎯 9. When to Use Prototype

---

### ✅ Use When:

✔ Object creation is expensive
✔ Object has many configurations
✔ Need duplicate objects
✔ Runtime object creation required

👉 Example:

* Game characters
* UI components
* Database objects
* Cache objects

---

## 🚫 10. When NOT to Use

---

### ❌ Avoid When:

✔ Object is simple
✔ No cloning needed
✔ Deep copy is too complex

---

## ⚠️ 11. Disadvantages

---

🔸 Deep copy is complex
🔸 Cloning can break encapsulation
🔸 Java `clone()` is tricky (checked exception)
🔸 Debugging harder

---

## 🔥 12. Real Industry Use Cases

---

### 🧩 Used In:

* Spring (Bean scopes / prototypes)
* Hibernate (entity duplication)
* Game engines (character cloning)
* UI frameworks

---

## 🧠 13. Interview Trick Questions

---

### ❓ Q1: Difference between Prototype and Builder?

👉 Answer:

* Prototype = copy existing object
* Builder = create object step-by-step

---

### ❓ Q2: Why `Cloneable` is bad?

👉 Because:

* No `clone()` method inside interface
* Requires casting
* Throws checked exception

---

### ❓ Q3: Better alternative?

👉 Copy constructor:

```java
public User(User other) {
    this.name = other.name;
}
```

✔ Cleaner than clone

---

## ⚡ 14. Final Summary

---

### 🔑 Key Points:

✔ Prototype = cloning
✔ Avoids expensive creation
✔ Supports shallow & deep copy
✔ Useful in performance-heavy systems

---

## 🧠 Memory Trick

👉 Remember:

> **Factory = Create new**
> **Builder = Build step-by-step**
> **Prototype = Copy existing**

---

## 🚀 Pro Insight (Advanced)

👉 In real systems:

* Use **Prototype + Factory** together
* Factory returns cloned objects

```java
User user = UserFactory.get("ADMIN"); // internally clone
```

---

If you want next 🔥
I can give:

✅ Prototype vs Clone vs Copy Constructor (very tricky)
✅ Real Spring Boot example using prototype scope
✅ Deep copy interview coding questions

Just tell me 👍


## ======== 2. Prototype vs Clone vs Copy Constructor (very tricky) ====
Great 🔥 — this is a **very tricky but high-level interview topic**.
Most people confuse these three, but once you get this, your **OOP clarity becomes strong**.

---

# 🚀 Prototype vs Clone vs Copy Constructor (Ultimate Breakdown)

---

## 🧠 1. First Understand the Relationship

👉 This is the key:

* **Prototype Pattern** → Design Pattern (concept)
* **Clone()** → Java mechanism to implement Prototype
* **Copy Constructor** → Alternative to clone (cleaner way)

---

## 🎯 2. One-Line Difference

| Concept          | Meaning                                |
| ---------------- | -------------------------------------- |
| Prototype        | Copy object using existing instance    |
| clone()          | Built-in Java method for copying       |
| Copy Constructor | Create new object using another object |

---

## 🔥 3. Deep Understanding

---

## 🏗️ 3.1 Prototype Pattern (Concept)

👉 It says:

> "Instead of creating object from scratch, copy an existing one."

✔ It doesn't care HOW you copy
✔ Can use:

* clone()
* copy constructor
* serialization

---

## ⚙️ 3.2 clone() Method (Java Way)

---

### ✅ Example

```java
class User implements Cloneable {
    String name;

    User(String name) {
        this.name = name;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone(); // shallow copy
    }
}
```

### Usage:

```java
User u1 = new User("Ritik");
User u2 = (User) u1.clone();
```

---

### ⚠️ Problems with clone()

❌ Requires `Cloneable` interface
❌ `Cloneable` is marker (no method inside)
❌ Throws checked exception
❌ Shallow copy by default
❌ Needs casting
❌ Breaks encapsulation

👉 That’s why many developers avoid it

---

## 🧱 3.3 Copy Constructor (Better Way)

---

### ✅ Example

```java
class User {
    String name;

    User(String name) {
        this.name = name;
    }

    // Copy Constructor
    User(User other) {
        this.name = other.name;
    }
}
```

### Usage:

```java
User u1 = new User("Ritik");
User u2 = new User(u1);
```

---

### ✔ Advantages

✔ No interface required
✔ No exception
✔ Full control (deep copy easily)
✔ Clean & readable

---

## ⚔️ 4. Full Comparison Table (INTERVIEW GOLD)

| Feature     | Prototype      | clone()        | Copy Constructor |
| ----------- | -------------- | -------------- | ---------------- |
| Type        | Design Pattern | Java method    | Java constructor |
| Purpose     | Copy object    | Implement copy | Implement copy   |
| Ease        | Conceptual     | Hard           | Easy             |
| Deep Copy   | Depends        | Manual         | Easy             |
| Exception   | ❌              | ✔              | ❌                |
| Casting     | ❌              | ✔              | ❌                |
| Readability | High           | Low            | High             |
| Flexibility | High           | Low            | High             |

---

## 🔥 5. Shallow vs Deep Copy (Important Here)

---

### ❌ clone() default → Shallow Copy

```java
List<String> roles;
```

👉 Both objects share same list ❌

---

### ✅ Copy Constructor → Deep Copy

```java
User(User other) {
    this.name = other.name;
    this.roles = new ArrayList<>(other.roles);
}
```

✔ Fully independent object

---

## 🧠 6. When to Use What

---

### 🏗️ Use Prototype Pattern When:

✔ Object creation is expensive
✔ Need many similar objects
✔ Runtime duplication needed

---

### ⚙️ Use clone() When:

✔ Legacy systems
✔ Need quick shallow copy
✔ Performance critical

👉 But rarely recommended in modern Java

---

### 🧱 Use Copy Constructor When:

✔ You want clean code
✔ Need deep copy
✔ Full control required

👉 ⭐ **Best practice in real projects**

---

## 🚫 7. When NOT to Use

---

### ❌ Avoid clone() when:

* Complex objects
* Deep copy required
* Clean architecture needed

---

### ❌ Avoid Copy Constructor when:

* Too many nested objects (can become large code)

---

## 🔥 8. Real Industry Insight

---

👉 In real-world (Spring Boot, Microservices):

| Approach          | Usage                       |
| ----------------- | --------------------------- |
| clone()           | Rare ❌                      |
| Copy Constructor  | Common ✅                    |
| Prototype Pattern | Used in caching / templates |

---

## 🧠 9. Interview Killer Answer

If interviewer asks:

👉 **"Clone vs Copy Constructor?"**

Say:

> "clone() is a built-in Java mechanism that performs shallow copy and has several design issues like checked exceptions and casting. Copy constructor is a cleaner and more flexible approach where we explicitly control how the object is copied, making it preferred in modern Java development."

---

## ⚡ 10. Real Example Comparison

---

### clone()

```java
User u2 = (User) u1.clone();
```

---

### Copy Constructor

```java
User u2 = new User(u1);
```

👉 Cleaner, safer, better ✅

---

## 🚀 11. Final Summary

---

👉 Remember this:

* **Prototype = Concept**
* **clone() = Old Java way**
* **Copy Constructor = Best modern way**

---

## 🔥 Pro Tip (Very Advanced)

👉 You can combine:

```java
User user = userPrototype.copy(); // internally copy constructor
```

✔ Clean + Scalable design

---

If you want next 🔥
I can give:

✅ Deep copy interview coding questions
✅ Real Spring Boot prototype scope example
✅ Serialization-based cloning (advanced)

Just tell me 👍


### ========== 