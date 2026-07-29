# Full Practice Lab Test  Car Park Management (Timed, 90 Minutes)

**Instructions (simulate real exam conditions):**
- Set a timer for 90 minutes.
- Do NOT use an IDE. Type your answers into a plain text editor (no autocomplete, no
  syntax highlighting-based hints beyond basic color, no run/compile button).
- Refer only to the Car Park Management code reference (`CarParkManagement_App_Code_Reference.pdf`)
  and your own memory/notes  not the internet, not this study pack, not AI.
- Write COMPLETE methods/classes wherever the question asks for one  not just the changed lines.
- Check your answers afterward against `6_Answers_And_Cheat_Sheet.md`.

*(These questions are intentionally phrased with different specific numbers/names than the
worked examples in file 3, so you practise applying the concept rather than recalling memorised
text.)*

---

## Question 1  5 Points
**Task: Resize the parking rows.**

Change the number of parking slots per row in the `4COSC010C_CarParkManagement` project as
follows:
- Row A should have 12 slots.
- Row B should have 24 slots.
- Row C should have 24 slots.
- Row D should have 12 slots.

Identify the method that needs to change, and rewrite the **complete method**.

---

## Question 2  15 Points
**Task: Validate parking row and slot numbers.**

Refer to the `parkCar()` method in the code reference document.

Add code to validate the inputs as follows:
- The parking row cannot be smaller than 1 (rows start at 1).
- The parking row cannot be larger than the number of rows in the car park (which is 4).
- The parking slot cannot be smaller than 1.
- The parking slot cannot be larger than the number of slots available in the selected row.

Write the complete, updated `parkCar()` method.

---

## Question 3  20 Points
**Task: Add a new class.**

Add a new class called `Vehicle` (assume it will be in a separate file `Vehicle.java`) with:
1. Two attributes, correctly declared:
   - `registrationNumber` (a `String`)
   - `parkingFee` (a suitable numeric type)
2. A constructor that takes both attributes as input.
3. Getters and setters for both attributes:
   - `getRegistrationNumber()` / `setRegistrationNumber()`
   - `getParkingFee()` / `setParkingFee()`
4. A method `printVehicle()` that prints the registration number and the parking fee to the
   output window.

Write the complete `Vehicle` class.

---

## Question 4  20 Points
**Task: Modify the main file to handle `Vehicle` objects.**

Modify `App.java` so it can handle objects of type `Vehicle`:

1. Create a new array of `Vehicle` objects (as a global/static variable) that can store 100
   vehicles.
2. Copy your updated `parkCar()` method from Question 2 and further update it to satisfy:
   - a. Prompt the user to enter the vehicle's registration number, and save it in a variable,
        **before** prompting for the row and slot numbers.
   - b. Work out the parking fee the user must pay, based on the selected row, according to this
        pricing plan  this should only be calculated **if the selected slot is available and the
        parking is successful**:
        - Row A: £4 per slot
        - Row B: £7 per slot
        - Row C: £7 per slot
        - Row D: £4 per slot
   - c. Create a `Vehicle` object storing the registration number and the fee, and store this
        object in the array from part 1  again, only if parking was successful.

Write your answer in this format:
```
// 1. Code for creating Vehicle object array
//===============Add code here=======
// 2. Code for updated parkCar() method
//===============Add code here=======
```

---

## Question 5  30 Points
**Task: Search vehicles by fee.**

Add a new method called `searchVehicles()` to `App.java` that:
- Asks the user to enter a fee amount.
- Searches all stored vehicles for that fee amount and prints each matching vehicle's
  registration number.
- If no vehicles match, prints a message stating that no vehicles were found for that amount.

Write the complete `searchVehicles()` method. You may use variables, methods, classes, or
objects created in previous questions.

---

## Question 6  10 Points
**Task: Save all vehicle records to a file.**

Add a method called `saveToFile()` to `App.java` that saves all vehicle information (registration
number and parking fee) to a text file, with **one vehicle per line**.

Write the complete `saveToFile()` method. You may use variables, methods, classes, or objects
created in previous questions.

---

## After You Finish
1. Time yourself  did you finish within 90 minutes? Which question took longest?
2. Mark your own answers using file 6.
3. For anything you got wrong or were unsure about, go back to the matching section in
   `2_Concept_Teaching_Full.md` and `3_CarParkManagement_Deep_Dive.md`.
4. Try re-doing the same 6 questions again in 2–3 days, from memory, to check retention  the
   real exam is a *different variation* of the same 6 skills, so fluency matters more than
   memorising this exact wording.
