# Answers to Self-Test Questions — Parts 1–6

## Part 1 — Fundamentals
1. `=` assigns a value into a variable; `==` compares two values and gives back `true`/`false`.
2. `int` is designed to store **whole numbers only**, for efficiency and precision — decimals
   need `double` instead.
3. A semicolon `;`.
4. They mark the start/end of a "block" of code that belongs together (a class body, a method
   body, an if/loop body, etc.).
5. Because `Scanner` is a pre-built tool that lives in Java's `java.util` package, not
   automatically available — `import` tells Java to bring it in so you can use it in this file.

## Part 2 — Control Flow
1. In an `if/else if/else` chain, only **one** branch ever runs (the first true one), and the
   rest are skipped. Separate `if` statements are checked completely independently, so more than
   one could run.
2. Because array indexes go from `0` to `length - 1`; using `i < array.length` as the condition
   stops the loop exactly at the last valid index, never trying `array.length` itself (which
   would be out of bounds).
3. It causes "fall-through" — execution continues into the next `case`'s code as well, instead
   of stopping, usually producing unintended extra output/behaviour.
4. When you need the loop body to run **at least once** before checking whether to continue —
   e.g. "ask for input, then keep re-asking while it's invalid."
5. The **inner** loop completes fully (all its iterations) for every single iteration of the
   outer loop.

## Part 3 — Methods
1. Parameters are the placeholder names used inside the method's own definition; arguments are
   the actual values supplied when the method is called.
2. Because the return type is a promise to the rest of the program that calling this method will
   give back a value of that type — the compiler enforces this promise.
3. Because when the program starts, no objects exist yet, so Java needs a way to run code
   directly from the class itself, without first creating an object.
4. Because the field is `private` — a public getter is the only way for outside code (in a
   different class) to read that value safely, while still protecting it from being changed
   directly/incorrectly.
5. It immediately exits the method early, running no further code inside it — since the method
   is `void`, it can't hand back a value, so it's followed by nothing.

## Part 4 — Arrays
1. `0` (the default value for numeric types).
2. Because `int[4][20]` forces **every** row to have exactly 20 columns — rows can't have
   individually different lengths in a rectangular array.
3. `ArrayIndexOutOfBoundsException`.
4. It measures the length of the array stored at that specific row — since it's a jagged array,
   different rows can have different lengths.
5. It creates a brand-new array in memory with 18 slots, each holding the default `int` value
   `0`, and returns a reference to it (which then gets assigned somewhere, e.g. into a row of a
   jagged array).

## Part 5 — OOP
1. A class is the blueprint/template describing what data and behaviour something will have; an
   object is an actual instance created from that blueprint, with real data filled in.
2. Because it's a special method whose entire purpose is different from a normal method (setting
   up a new object) — Java identifies it as a constructor precisely because it matches the class
   name and has no return type; giving it a return type would make it a regular method instead.
3. The field never actually gets updated — the parameter is assigned to itself, meaning the
   object silently keeps whatever value it had before (often the default, like `null` or `0`).
4. Because it lets you fully control and validate access to the data (encapsulation) — even
   though right now the getter/setter just directly reads/writes the field, you could later add
   checks (e.g. "reject negative fees") inside the setter without breaking any code that calls it.
5. Because `Ticket`'s fields belong to individual **objects**, and no object exists until you
   explicitly create one with `new Ticket(...)` — a static method has no automatic object to
   refer to.

## Part 6 — Exceptions & File I/O
1. Checked exceptions (like `IOException`) must be explicitly caught or declared, enforced by the
   compiler; unchecked exceptions (like `NullPointerException`) are not enforced by the compiler
   and will simply crash the program at runtime if not handled.
2. Because file operations can throw a checked exception (`IOException`/`FileNotFoundException`),
   which Java requires you to handle before the code will even compile.
3. It finishes writing and safely releases the file so other programs (or your own program later)
   can use it; forgetting it can leave data un-saved or the file inaccessible.
4. `.write()` does not automatically move to a new line — you must add one manually (e.g. with
   `System.lineSeparator()`); `.println()` automatically adds a new line after what it writes.
5. `import java.io.FileWriter;` and `import java.io.IOException;`
