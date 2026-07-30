# Mock Test Question 6 — Fully Explained From Zero

## Decoding the question, sentence by sentence

> "Task: Include a method to save payment information in a text file."

**Plain English:** You need to write data out to an actual file on disk (not just print it to the
screen) — this is "File I/O" from your Zero-knowledge Part 6.

> "Add a method called saveToFile() to App.java (code in the link above) to save all the payments
> information from all the payments."

**Plain English:** Write a brand-new method, `saveToFile()`, that goes through **every** payment
that currently exists (again, using your `payments` array and `paymentCount` from Question 4),
and writes their data into a file.

> "For each payment, you should save the email and the payment amount."

**Plain English:** For every single payment, both pieces of information (email AND amount) need
to end up in the file — not just one of them.

> "Each line of the file should contain 1 payment transaction."

**Plain English:** If there are, say, 3 payments stored, the resulting file should have exactly
**3 lines** — one payment per line, not all of them squashed onto a single line, and not spread
across multiple lines per payment either.

> "You may refer to variables, methods, classes, or objects created in previous questions, to
> write saveToFile() method as required."

**Plain English:** Same as Question 5 — you're expected to use `payments`, `paymentCount`, and
`Payment`'s getters.

---

## Step 1 — Recognising this needs file-writing tools, and importing them

To write to a file in Java, you need one of the built-in file-writing classes. The simplest,
most reliable choice for this kind of task is `FileWriter`. Since it can throw a checked exception
(`IOException`) if something goes wrong, you also need to import that:

```java
import java.io.FileWriter;
import java.io.IOException;
```
These go at the **very top of `App.java`**, alongside the existing `import java.util.Scanner;`
line — they tell Java "bring in these additional built-in tools so I can use them in this file."

## Step 2 — Setting up the method and the try/catch wrapper

```java
private static void saveToFile() {
    try {

    } catch (IOException e) {
        System.out.println("An error occurred while saving.");
    }
}
```
Any code that creates or writes to a `FileWriter` **must** be wrapped in a `try/catch` block,
because Java forces you to handle this specific kind of risky operation (recall Zero Part 6 —
checked exceptions). If something goes wrong (e.g. the file can't be created), the `catch` block
runs instead of crashing your entire program.

## Step 3 — Creating the file writer

```java
FileWriter writer = new FileWriter("payments.txt");
```
This creates (or overwrites, if it already exists) a text file called `payments.txt` in your
project's folder, and gives you a `writer` object you can use to actually put text into it.

## Step 4 — Looping through every payment and writing one line each

```java
for (int i = 0; i < paymentCount; i++) {
    writer.write(payments[i].getEmail() + "," + payments[i].getPaymentAmount());
    writer.write(System.lineSeparator());
}
```
- `for (int i = 0; i < paymentCount; i++)` — the exact same safe-looping pattern from Question 5:
  only loop through the *real*, actually-created payments, never the full array capacity.
- `writer.write(payments[i].getEmail() + "," + payments[i].getPaymentAmount());` — builds one
  line of text by joining the email, a comma, and the payment amount together using `+`
  (concatenation). Because `getPaymentAmount()` returns a number and we're joining it with `+`
  next to a `String` (the comma), Java automatically converts the number into text form so they
  can be combined. This single `write` call puts that whole combined line of text into the file
  — but **without** moving to a new line yet.
- `writer.write(System.lineSeparator());` — writes a "start a new line" instruction into the
  file, so that the *next* payment (the next time around the loop) begins on a fresh line,
  rather than all payments running together as one giant line of text. This is exactly what
  satisfies "each line of the file should contain 1 payment transaction."

## Step 5 — Closing the file and confirming success

```java
writer.close();
System.out.println("Payments saved successfully.");
```
- `writer.close();` — finishes writing and safely releases the file. **Never forget this** — if
  you skip it, some or all of your data might not actually make it into the file, or the file
  might remain locked/incomplete.
- The success message only prints if everything above ran without throwing an exception.

---

## Step 6 — The complete final answer

```java
private static void saveToFile() {
    try {
        FileWriter writer = new FileWriter("payments.txt");

        for (int i = 0; i < paymentCount; i++) {
            writer.write(payments[i].getEmail() + "," + payments[i].getPaymentAmount());
            writer.write(System.lineSeparator());
        }

        writer.close();
        System.out.println("Payments saved successfully.");

    } catch (IOException e) {
        System.out.println("An error occurred while saving.");
    }
}
```
Don't forget: if the exam asks for "complete" code including imports, also mention/include:
```java
import java.io.FileWriter;
import java.io.IOException;
```

### Example of what the resulting file would actually look like
If 3 payments were made, `payments.txt` would contain:
```
alex@email.com,50
sam@email.com,80
jo@email.com,50
```
One transaction per line, exactly as required — email and amount together, separated by a comma.

---

## Why this matters for the real exam

The real Question 6 will ask for essentially the same `saveToFile()` structure, applied to
whatever object type you built in the Car Park version (e.g. `Ticket`/`Vehicle` with a
registration number and fee instead of email and amount). **The entire skeleton — import
statements, try/catch, FileWriter, looping only to the real count, one write+newline per record,
closing the writer — is identical every time.** Master this exact shape once, and you can adapt
it to any two fields they ask you to save.

## Quick self-check
1. Why must `FileWriter` code be wrapped in a `try/catch` block?
2. What would the resulting file look like if you forgot the `writer.write(System.lineSeparator());` line?
3. Why does the loop use `paymentCount` instead of `payments.length`?
4. What happens if you forget to call `writer.close()`?
5. What two `import` statements are needed for this exact version of `saveToFile()`?

*(1: because creating/writing to a file is a checked-exception-throwing operation
(`IOException`) — Java's compiler forces you to either catch it or declare it, and catching it
here prevents the whole program from crashing if saving fails. 2: all the payment records would
run together on a single, very long line of text, with no line breaks separating one transaction
from the next — failing the "one line per transaction" requirement. 3: because only
`paymentCount` slots actually contain real `Payment` objects; the rest of the 100-slot array is
still empty (`null`), and trying to read from a `null` slot would crash the program. 4: the data
may not be fully written to the file, and the file could remain incomplete or locked. 5:
`import java.io.FileWriter;` and `import java.io.IOException;`.)*
