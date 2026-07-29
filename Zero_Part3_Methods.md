# Java From Absolute Zero — Part 3: Methods

## 1. What is a method, and why do we need them?

A **method** is a named, reusable block of code that performs a specific task. Instead of
writing the same code over and over, you write it once inside a method, then **call** (use) that
method by name whenever you need it.

Think of a method like a recipe: it has a name ("Make Pancakes"), it might need ingredients
(**parameters**), and it might produce a result (a **return value**) — or it might just *do*
something without giving anything back (like printing to the screen).

---

## 2. Anatomy of a method — every word explained

```java
private static void showParkingLayout() {
    // code here
}
```

Reading left to right:

| Part | Name | Meaning |
|---|---|---|
| `private` | **Access modifier** | Controls *who* is allowed to call this method. `private` means "only code inside this same class can call it." (Full details below.) |
| `static` | **Static modifier** | Means this method belongs to the class itself, not to an individual object. (Full details below.) |
| `void` | **Return type** | Says what type of value this method gives back when it finishes. `void` = "nothing is given back." |
| `showParkingLayout` | **Method name** | The name you use to call this method later. Convention: starts lowercase, camelCase for multiple words. |
| `()` | **Parameter list** | The information the method needs as input. Empty here = needs nothing. |
| `{ }` | **Method body** | The actual code that runs when this method is called. |

### A method that takes input (parameters) and gives output (a return value)
```java
private static int addNumbers(int a, int b) {
    int sum = a + b;
    return sum;
}
```
- `int` (before the name) — this method's **return type**. It promises to give back an `int`
  when it finishes. Every path through the method **must** end in a `return` statement matching
  this type, or the compiler will refuse to compile it.
- `addNumbers` — the method's name.
- `(int a, int b)` — the **parameters**: this method needs two whole numbers to do its job,
  which will be temporarily called `a` and `b` while the method runs.
- `return sum;` — **immediately exits** the method, handing back the value of `sum` to whoever
  called it.

### Calling (using) a method
```java
int result = addNumbers(3, 4); // "arguments" 3 and 4 are passed in; result becomes 7
System.out.println(result);    // prints 7
```
The values `3` and `4` you pass in when calling are called **arguments**. Inside the method,
they get copied into the parameter variables `a` and `b`. This is a really important distinction:
- **Parameters** = the names used *inside* the method's own definition (`a`, `b`).
- **Arguments** = the actual values you supply *when calling* the method (`3`, `4`).

---

## 3. `void` vs returning a value

```java
public static void greet() {
    System.out.println("Hello!");
    // no return statement needed, because the return type is void
}

public static int square(int number) {
    return number * number; // MUST return something, because return type is int
}
```
- If the return type is `void`, the method **cannot** use `return someValue;` — it can only use
  a plain `return;` (with nothing after it) to exit early if needed, or just let the method finish
  naturally.
- If the return type is anything else (`int`, `double`, `String`, `boolean`, a class name, etc.),
  the method **must** return a value matching that type on every possible path through the code.

### Returning early (a common validation pattern)
```java
private static void parkCar() {
    if (row < 0 || row > 3) {
        System.out.println("Invalid row.");
        return; // exits the method immediately — nothing below this runs
    }
    // this code only runs if the row WAS valid
    System.out.println("Continuing...");
}
```
`return;` (with nothing after it, since this method is `void`) is a clean way to **stop a method
early** if some condition fails, so you don't need to wrap all the remaining code in an `else`.

---

## 4. Access Modifiers — `public` vs `private`

These control **who is allowed to call/use this method (or field, or class)**.

| Modifier | Who can access it |
|---|---|
| `public` | Anyone — any class, from anywhere in the program. |
| `private` | Only code written inside the **same class**. |
| *(none — "default")* | Only code in the same **package** (folder of related classes) — rarely tested, low priority. |
| `protected` | Same package, plus subclasses — advanced OOP topic, rarely needed at this level. |

**Why does it matter?** It's about **hiding internal details** so other code can't mess with
them by accident. In the Car Park project:
- `main()` and `initialiseParkingSlots()` are `public` — meant to be entry points others (like
  Java itself, calling `main`) need to reach.
- `getOption()`, `parkCar()`, `showParkingLayout()` are `private` — internal helper methods that
  only `App` itself needs to call; nothing outside this class should ever need to call them
  directly.

This same idea applies to **class fields** (variables), which is central to OOP — see Part 5.

---

## 5. `static` — one of the trickiest ideas for beginners

`static` means: **this belongs to the class itself, not to any individual object made from the
class.**

Imagine a class `Counter` that's meant to track "how many objects have been made so far" — that
count should be shared and the same for *everyone*, not something each individual object has its
own separate copy of. That's what `static` is for.

### Why is everything in `App.java` static?
```java
public static void main(String[] args) { ... }
```
`main` **must** be `static`, because when your program starts, **no objects exist yet** — Java
needs a way to start running code without first having to create an `App` object. Since `main` is
static, and static methods can only *directly* call other static things (without creating an
object), every other method in `App.java` (`parkCar()`, `showParkingLayout()`, etc.) and the
`parkingSlots` variable are **also** made `static`, purely so `main()` can reach them directly.

### The contrast: your own classes (like `Ticket`/`Vehicle`) are usually NOT static
```java
public class Ticket {
    private String registrationNumber; // NOT static — every Ticket object has its OWN copy

    public String getRegistrationNumber() { // NOT static — called on a specific object
        return registrationNumber;
    }
}
```
```java
Ticket t1 = new Ticket("AB12", 5.0);
Ticket t2 = new Ticket("CD34", 8.0);

t1.getRegistrationNumber(); // returns "AB12" — belongs to t1 specifically
t2.getRegistrationNumber(); // returns "CD34" — belongs to t2 specifically
```
Here, each `Ticket` object keeps its **own separate** `registrationNumber`. That's the whole
point of non-static (called "instance") fields and methods — every object gets its own copy of
the data, and methods operate on "whichever object you called them on."

**Simple rule for the exam:** `App.java`'s own methods/fields → `static`. Your new class's fields
and methods (getters, setters, print methods) → **not** `static`.

---

## 6. Method Overloading (good to recognise, rarely a whole question)

You can have multiple methods with the **same name** but **different parameters** — Java tells
them apart by how many/what type of arguments you pass.
```java
public static int add(int a, int b) { return a + b; }
public static double add(double a, double b) { return a + b; }
```
Calling `add(2, 3)` uses the first; `add(2.5, 3.5)` uses the second. You won't likely need to
*write* overloaded methods in this exam, but recognise the concept if you see it.

---

## Quick Self-Test
1. What's the difference between a method's **parameters** and the **arguments** used to call it?
2. Why must a method with return type `int` always use `return` with a value?
3. Why is `main()` always `static`?
4. If `Ticket` has a private field `parkingFee`, why do we still need a `public` getter for it?
5. What does `return;` (with nothing after it) do inside a `void` method?

**Next: Part 4 — Arrays (in full depth)**
