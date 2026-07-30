# Answers — Gym Locker Practice Test

## Task 1
```java
public static void initialiseLockers() {
    lockerBays = new int[3][];
    lockerBays[0] = new int[12]; // Bay A
    lockerBays[1] = new int[20]; // Bay B
    lockerBays[2] = new int[16]; // Bay C
}
```
Remember: the outer array size had to grow from `new int[2][]` to `new int[3][]` before adding
the new row — this is the most commonly missed step.

## Task 2
```java
private static void releaseLocker() {
    Scanner input = new Scanner(System.in);
    System.out.print("Enter bay number: ");
    int bay = input.nextInt() - 1;

    System.out.print("Enter locker number: ");
    int locker;
    try {
        locker = input.nextInt();
    } catch (Exception e) {
        System.out.println("Invalid input. Locker number must be an integer.");
        return;
    }
    locker = locker - 1;

    if (locker < 0 || locker >= lockerBays[bay].length) {
        System.out.println("Invalid locker number.");
    } else if (lockerBays[bay][locker] == 1) {
        lockerBays[bay][locker] = 0;
        System.out.println("Locker released successfully.");
    } else {
        System.out.println("This locker was not rented.");
    }
}
```
Notice `lockerBays[bay].length` is used instead of hardcoding `12`, `20`, or `16` — this
automatically works correctly for Bay A, B, or C, whichever was selected.

## Task 3
```java
public class Member {

    private String fullName;
    private String phoneNumber;
    private int membershipID;

    public Member(String fullName, String phoneNumber, int membershipID) {
        this.fullName = fullName;
        this.phoneNumber = phoneNumber;
        this.membershipID = membershipID;
    }

    public String getFullName() {
        return fullName;
    }
    public void setFullName(String fullName) {
        this.fullName = fullName;
    }

    public String getPhoneNumber() {
        return phoneNumber;
    }
    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }

    public int getMembershipID() {
        return membershipID;
    }
    public void setMembershipID(int membershipID) {
        this.membershipID = membershipID;
    }

    public void printMember() {
        System.out.println("Name: " + fullName + ", Phone: " + phoneNumber + ", Membership ID: " + membershipID);
    }

    // Encapsulation has been applied by declaring all attributes (fullName, phoneNumber,
    // membershipID) as private, preventing direct outside access. All access to this data is
    // instead controlled through the public getter and setter methods.
}
```
Note: this time the getter/setter for the ID **does** match the field name exactly
(`getMembershipID()`/`setMembershipID()`), unlike the Library test's `getID()`/`setID()` trap —
always read what the question actually asks for, rather than assuming every exam will use the
same shortened naming.

## Task 4
```java
private static Member[] members = new Member[50];
```

## Task 5
```java
private static void registerMember() {
    Scanner input = new Scanner(System.in);

    System.out.print("Enter full name: ");
    String fullName = input.nextLine();

    System.out.print("Enter phone number: ");
    String phoneNumber = input.nextLine();

    System.out.print("Enter membership ID: ");
    int membershipID = input.nextInt();

    Member member = new Member(fullName, phoneNumber, membershipID);

    boolean added = false;

    for (int i = 0; i < members.length; i++) {
        if (members[i] == null) {
            members[i] = member;
            added = true;
            break;
        }
    }

    if (added) {
        System.out.println("Member registered successfully.");
    } else {
        System.out.println("Member list is full. Cannot register more members.");
    }
}
```

## Task 6
Single new line for `getOption()`:
```java
System.out.println("| 4) Register member        |");
```

Complete `runMenu()`:
```java
public static void runMenu() {
    int option;
    boolean cont = true;
    while (cont) {
        option = getOption();
        switch (option) {
            case 0:
                cont = false;
                break;
            case 1:
                rentLocker();
                break;
            case 2:
                releaseLocker();
                break;
            case 3:
                showLockers();
                break;
            case 4:
                registerMember();
                break;
            default:
                System.out.println("Option not available.");
        }
    }
}
```

---

## Self-Grading Checklist
- [ ] Task 1: Did you resize the **outer** array (`new int[3][]`), not just add a new row line?
- [ ] Task 2: Did you use `try/catch` for the integer check, **before** doing `-1` or any range
      comparison?
- [ ] Task 2: Did you use `.length` instead of typing `12`/`20`/`16` anywhere?
- [ ] Task 3: Did you use the **exact** getter/setter names the question specified?
- [ ] Task 3: Is there an actual `//` comment at the end explaining encapsulation, in your own
      words?
- [ ] Task 4: Did you avoid `ArrayList`, using a plain array instead?
- [ ] Task 5: Since no counter was declared in Task 4, did you search for `null` slots instead of
      assuming a counter exists?
- [ ] Task 5: Did you `break;` out of the loop once a free slot was found and used?
- [ ] Task 6: Did you write **only one line** for the `getOption()` part, but the **complete
      method** for `runMenu()`, matching what was actually asked for each part?

If you can tick every box, you've genuinely internalised the pattern — not just memorised one
specific answer.
