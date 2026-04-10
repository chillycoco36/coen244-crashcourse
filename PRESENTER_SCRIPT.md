# COEN 244 CRASH COURSE — PRESENTER SCRIPT v2
## 3 Hours • Read this top to bottom, follow the time markers

> **How to use this script:** Each section starts with a `[TIME]` marker. The bold lines in `> "quotes"` are what you say out loud. The `[brackets]` tell you what to do on the website. Stick to the timing — if you're falling behind, skip the "skim quickly" parts.

---

# OPENING [0:00 — 0:03] (3 min)

**[Open the website on http://localhost:3000 — show the hero section]**

> "Hey everyone, welcome. My name's Chris, and for the next 3 hours we're going to crash-course COEN 244 together. The goal here is simple: get you ready for the final."
>
> "Quick intro to how this works. We're splitting the time in half. First hour and a half — we go through every topic on the syllabus. Theory, examples, the patterns you need to recognize. Second hour and a half — we sit down and solve a full mock final, question by question."
>
> "Everything you see on this website was built from real past exams: 2004, 2011, 2017, plus solution exams I got my hands on. So when I say 'this comes up every year' — I mean it literally."

**[Scroll to the "Exam Strategy" box (right after Part 1 header)]**

> "Look at this table real quick. This is the priority order. Polymorphism and Inheritance together make up about 50% of the final. Operator overloading is another 15-20%. If you only remember three things from today, make it those three."
>
> "I'm going to go fast in some places and slow in others. Stop me whenever you have a question — that's what we're here for."

**[Pause briefly, then click on "COEN 243 Review" in the nav bar]**

---

# PART 1 — THEORY [0:03 — 1:30]

---

## SECTION 0: COEN 243 REVIEW — POINTERS [0:03 — 0:15] (12 min)

**[You should now be on the COEN 243 Review section]**

> "Before we touch anything 244-specific, we need to make sure pointers are crystal clear. If pointers don't make sense, nothing in this course will. So we're spending 10 minutes on this."

### Variables, I/O, Functions [0:03 — 0:05] (2 min — skim)

**[Scroll past Variables, Input/Output, Control Structures quickly]**

> "Variables, cin, cout, if/else, for loops — you know all this from 243. I'm scrolling past."

**[Stop on the "Functions" section]**

> "One thing I want to highlight here: pass by value versus pass by reference. Look at these two cards side by side."
>
> "Pass by value — function gets a COPY. Whatever you do inside the function, the original variable doesn't change."
>
> "Pass by reference — you put an ampersand in the parameter. Now the function has direct access to the original variable. Whatever you do inside, IT CHANGES THE ORIGINAL."
>
> "Why does this matter? Because in 244, we pass objects to functions ALL the time. And objects are big — copying them is slow. So we always pass them by reference, usually as `const reference` so they can't be accidentally modified."

### Pointers — The Big One [0:05 — 0:13] (8 min)

**[Scroll to "POINTERS — The Most Important COEN 243 Topic" box]**

> "OK. Pointers. A pointer is a variable that holds a memory address. That's it. It's not magic, it's just a variable that stores an address instead of a number."

**[Scroll to "Declaring & Using Pointers"]**

> "Two operators. Burn these into your brain."
>
> "**Ampersand `&`** means 'give me the address of'. So `&x` is 'the address where x lives in memory.'"
>
> "**Star `*`** means 'go to that address and give me what's there'. We call this dereferencing."

**[Point to the code block with `int x = 42; int* ptr = &x;`]**

> "Look at this code. We have x equals 42. Then `int* ptr = &x;` — ptr now stores the address of x."
>
> "When we print ptr, we get the address — something like 0x7fff. When we print `*ptr`, we get 42. Because `*ptr` means 'go to the address ptr is holding, and give me the value there.'"
>
> "Now the cool part — `*ptr = 100`. This goes to x's address and changes the value to 100. We just modified x THROUGH the pointer."

**[Scroll to the memory visualization box below it]**

> "If you're visual, look at this diagram. x lives at address 0x1000 with value 42. ptr lives at address 0x2000 — and the VALUE that ptr holds is 0x1000 — x's address. So `*ptr` means 'go to 0x1000' and you get 42. Make sense?"

**[Scroll to "Dynamic Memory Allocation (new / delete)"]**

> "This is the most important part of this whole section. Pay attention."
>
> "`new` allocates memory on the heap. `delete` frees it. Every `new` MUST have a matching `delete`. Otherwise you have a memory leak."
>
> "If you allocate an array with `new int[10]`, you free it with `delete[] arr` — with the brackets. Forget the brackets, you get undefined behavior."
>
> "Why is this important for 244? Because almost every class you write in this course will have a `char*` or a dynamic array as a data member. And that means you NEED a destructor, you NEED a copy constructor, and you NEED to overload operator=. We call this the Rule of Three. It comes up on EVERY final."

**[Scroll to "Common Pointer Mistakes"]**

> "Real quick — four mistakes I want you to memorize and never make:"
>
> "**One — uninitialized pointer.** You declare `int* ptr` but don't assign it. Then you do `*ptr = 42`. CRASH. ptr is pointing to garbage. Always initialize to nullptr or to a real address."
>
> "**Two — memory leak.** You `new` something, then reassign the pointer without deleting the old memory. The old memory is now lost forever."
>
> "**Three — dangling pointer.** You delete the memory, then try to use the pointer. The memory is gone, the pointer still has the old address. Undefined behavior."
>
> "**Four — wrong delete.** You used `new[]` but you delete without brackets. Always match: `new` with `delete`, `new[]` with `delete[]`."

### Wrap up Section 0 [0:13 — 0:15] (2 min)

> "Quick check — does anyone want me to go over anything in pointers again before we move on? No? Good."
>
> "If you want to test yourself later, there are practice exercises at the bottom of this section with full solutions. Try them after class."

**[Click "Classes" in the nav bar]**

---

## SECTION 1: CLASSES [0:15 — 0:27] (12 min)

**[You should now be on the Classes section]**

> "Welcome to 244. This is where it starts. Classes are the foundation of literally everything in this course."

### Exam Alert + What is a Class [0:15 — 0:18] (3 min)

**[Point to the EXAM ALERT box at the top]**

> "Read this exam alert. Classes never get a standalone question on the final, but they're the foundation of EVERY question. You need to know constructors, destructors, deep copy, static members, and the five predefined functions cold."
>
> "Quick definition — a class is a blueprint for objects. It bundles data and functions together. An object is an INSTANCE of a class."

**[Scroll to "What is a Class?" — show the Time class example]**

> "Look at this Time class. Four parts to every class:"
>
> "**Constructor** — runs when you create an object. Same name as the class, no return type. Initializes the data members."
>
> "**Data members** — the variables. These are usually `private`. That's encapsulation — hiding the data."
>
> "**Member functions** — what the object can do. Usually `public`. These are the methods."
>
> "**Destructor** — runs when the object is destroyed. Tilde plus class name. Used to clean up dynamic memory."

### Encapsulation + Pointers to Objects [0:18 — 0:20] (2 min)

**[Scroll to "Encapsulation & Access Specifiers"]**

> "Three access levels:"
> "- `private` — only the class itself can access these"
> "- `public` — anyone can access these"
> "- `protected` — the class AND derived classes (we'll cover this in inheritance)"
>
> "Look at the warning box — `t.hour = 14` is a COMPILE ERROR. Because hour is private. You'd have to use `setTime(...)` instead."

**[Scroll to "Pointers to Objects & Dynamic Memory"]**

> "Two ways to create objects:"
> "- Stack: `Time t;` — automatic, destroyed when it goes out of scope"
> "- Heap: `Time* pT = new Time();` — manual, you must `delete pT` later"
>
> "Stack object: use the dot operator. `t.printTime()`."
> "Heap object via pointer: use the arrow operator. `pT->printTime()`."

### Copy Constructor [0:20 — 0:24] (4 min — go SLOW)

**[Scroll to "Copy Constructor & Deep Copy" — this is critical]**

> "OK pay attention here. This is the #1 thing students mess up on the exam."
>
> "A copy constructor is a special constructor that takes a `const` reference to another object of the same class. Its job is to make a copy."
>
> "The compiler gives you a default copy constructor for free. BUT — and this is the big BUT — the default does a SHALLOW copy. It just copies the data members value-for-value."
>
> "If your class has a `char*`, the default just copies the pointer. Now both objects point to the same memory. If one deletes it, the other has a dangling pointer. Disaster."

**[Point to the Student copy constructor code]**

> "So when your class has dynamic memory, you write your OWN copy constructor that does a DEEP COPY. Look at this pattern — you'll see it everywhere:"
>
> "1. Allocate new memory: `new char[strlen(other.fname) + 1]`. The +1 is for the null terminator."
> "2. Copy the data: `strcpy(fname, other.fname)`."
>
> "Memorize this two-line pattern. You'll write it five times on the exam."

> "When does the copy constructor get called? Three situations:"
> "1. `Student s2(s1);` or `Student s2 = s1;` — explicit copy"
> "2. Passing an object by VALUE to a function"
> "3. Returning an object by VALUE from a function"

### Static Members + this Pointer [0:24 — 0:26] (2 min)

**[Scroll to "Static Members"]**

> "Static members belong to the CLASS, not to objects. All objects share them."
>
> "Classic example — a counter that tracks how many objects exist. Increment in the constructor, decrement in the destructor."
>
> "Two rules:"
> "1. Initialize OUTSIDE the class: `int Account::count = 0;`"
> "2. Call static functions with the class name: `Account::getCount()` — no object needed."

**[Scroll past "this Pointer" briefly]**

> "`this` is a pointer to the current object. Every member function has it implicitly. Used when there's a name collision between parameters and data members."

### Practice Question [0:26 — 0:27] (1 min — quick)

**[Click on the "BankAccount Class" practice question to expand it]**

> "Quick look — BankAccount with `char* ownerName` and a static count. Constructor does the deep copy pattern, copy constructor does the same, destructor does delete[] and decrements count."
>
> "I'm not going to solve this on screen — but expand the solution if you want to see it. This exact pattern shows up on every exam."

**[Click "Composition" in the nav]**

---

## SECTION 2: COMPOSITION [0:27 — 0:35] (8 min)

### What is Composition [0:27 — 0:30] (3 min)

**[On the Composition section]**

> "Composition is simple: a class CONTAINS objects of another class as data members. We call this 'has-a'. A Car has-a Engine. A Date has-a Time."
>
> "It's the opposite of inheritance, which is 'is-a'. Don't mix them up."

**[Scroll to "What is Composition?" with the Date example]**

> "Look at this Date class. It has int day, month, year — but it ALSO has a `Time time` data member. Time is a whole separate class. That's composition."
>
> "Important: Date can't directly access Time's private members. Date has to use Time's public functions."

### Construction Order + Initializer List [0:30 — 0:33] (3 min — important)

**[Scroll to "Construction & Destruction Order"]**

> "Here's a rule that gets tested:"
> "**Construction order:** member objects are constructed FIRST, before the composite class's constructor body even runs."
> "**Destruction order:** the REVERSE. Composite body finishes, then member destructors run."

**[Scroll to "Member Initializer List" — this is key]**

> "Now this is critical. The member initializer list. Two cases where you MUST use it:"
> "1. The member class has no default constructor"
> "2. You have a `const` data member or a reference data member"

**[Show the comparison code]**

> "Look at the comparison. Without the initializer list, the compiler tries to call `Engine()` first — which doesn't exist — then assigns. Compile error."
>
> "WITH the initializer list, you write `: engine(hp, type)` after the constructor signature. This calls Engine's parameterized constructor directly."
>
> "Memorize the syntax: `Car::Car(...) : member1(value), member2(value) { body }`."

### Dynamic Arrays + Practice [0:33 — 0:35] (2 min)

**[Scroll to "Dynamic Arrays of Objects" — keep brief]**

> "Quick reference. Four ways to have arrays of objects:"
> "1. `Time arr[40]` — static array"
> "2. `Time* ptr = new Time[size]` — dynamic array of objects"
> "3. `Time* arr[40]` — static array of pointers"
> "4. `Time** arr = new Time*[size]` — dynamic array of pointers"
>
> "The last one is what we use for polymorphism. You'll see it constantly. `Type**` — pointer to pointer."

**[Click "Inheritance" in the nav]**

---

## SECTION 3: INHERITANCE [0:35 — 0:47] (12 min)

### Exam Alert + Basics [0:35 — 0:38] (3 min)

**[On the Inheritance section, point to the exam alert]**

> "Read this. Inheritance is never tested alone — it's always combined with polymorphism in the big 25-30 mark question. So we're learning the foundation here."

**[Scroll to "What is Inheritance?"]**

> "Inheritance — you create a new class FROM an existing class. The new class inherits everything the parent has. This is 'is-a'. Student IS-A Person. Circle IS-A Shape."
>
> "Three types of inheritance — public, protected, private. **In this course, you basically only ever use public.** With public inheritance:"
> "- Public members stay public"
> "- Protected members stay protected"
> "- Private members are NOT directly accessible — you have to go through getters"

### Constructor Chaining [0:38 — 0:43] (5 min — slowly!)

**[Scroll to "Constructors & Destructors in Inheritance"]**

> "This is the most important part. Order matters."
>
> "**Construction:** Base constructor runs FIRST, then derived. Always."
> "**Destruction:** REVERSE — derived destructor first, then base."

**[Show the Student constructor code]**

> "Look at this. `Student::Student(name, age, id, gpa) : Person(name, age), studentId(id), gpa(g) {}`"
>
> "The `Person(name, age)` part — that's calling the BASE constructor in the initializer list. You're explicitly saying 'here's how to construct the Person part of me.'"

**[Point to the IMPORTANT box]**

> "Read this in red. If the base class has no default constructor and you DON'T explicitly call it in the initializer list — COMPILE ERROR. The compiler tries to call `Person()` which doesn't exist."
>
> "This was literally a question on the 2017 final: 'If a base class has a constructor with arguments, the derived class must...' Answer: must have a constructor with arguments AND pass them to the base class constructor."

### Pointer Assignments [0:43 — 0:46] (3 min)

**[Scroll to "Base & Derived Pointer Assignments" table]**

> "This table is gold. Memorize these four cases:"
>
> "1. **Base pointer to base object** — fine, normal."
> "2. **Derived pointer to derived object** — fine, normal."
> "3. **Base pointer to derived object** — this is OK! It's called UPCASTING. This is how polymorphism works."
> "4. **Derived pointer to base object** — COMPILE ERROR. Not allowed without explicit cast."
>
> "Number 3 is the magic. You can have `Shape* p = new Circle()`. Through that Shape pointer, you can only call Shape's functions — UNLESS those functions are virtual. And that brings us to..."

### Practice [0:46 — 0:47] (1 min)

**[Quickly expand the Person → Student → GradStudent practice]**

> "Quick example — three-level hierarchy. GradStudent's constructor calls Student's, which calls Person's. GradStudent's `print()` calls Student's print first, then prints its own data."

**[Click "Polymorphism" in the nav]**

---

## SECTION 4: POLYMORPHISM [0:47 — 1:05] (18 min) ⭐

### Set the stage [0:47 — 0:48] (1 min)

**[On the Polymorphism section, the topic time says "MOST TESTED"]**

> "Stop me if you need a break — but this is the topic. The single most important section of the course. Every past final has a major question on this. If you walk out today understanding nothing else, understand this."

**[Point to the EXAM ALERT in red]**

> "Read this. EVERY past final has a polymorphism question worth 20-30 marks. Class hierarchy with virtual functions, driver program with array of base-class pointers, dynamic binding, virtual destructors. We're going to break each of those down."

### What is Polymorphism [0:48 — 0:50] (2 min)

**[Scroll to "What is Polymorphism?"]**

> "Poly = many. Morph = forms. Same function name, different behavior depending on the object type."
>
> "Imagine a video game. You have an array of SpaceObject pointers — but the actual objects are Martians, Spaceships, Lasers, whatever. You loop through and call `draw()` on each one. The Martian draws itself one way, the Spaceship draws itself another way. Same function call, different result."
>
> "That's polymorphism."

### Virtual Functions + Binding [0:50 — 0:55] (5 min — critical)

**[Scroll to "Virtual Functions & Dynamic Binding"]**

> "Virtual functions are how we ENABLE polymorphism. You declare a function `virtual` in the base class, and derived classes OVERRIDE it."

**[Point to the two concept cards: Static vs Dynamic Binding]**

> "Two types of binding. This is the #1 exam question."
>
> "**Static binding** — the compiler decides at COMPILE TIME which function to call. The compiler looks at the type of the variable/pointer."
>
> "**Dynamic binding** — the decision is made at RUNTIME based on the actual object type."

**[Show the Shape/Circle code example]**

> "Look at this. `Shape* sPtr = new Circle(...)`. Then `sPtr->print()`."
>
> "If `print` is NOT virtual: static binding, calls `Shape::print()`. The compiler sees a Shape pointer, calls Shape's version."
>
> "If `print` IS virtual: dynamic binding, calls `Circle::print()`. At runtime, the program checks 'what is this actually pointing to?' and finds Circle."

**[Scroll to the Binding Rules Summary table]**

> "Memorize this table. Five cases:"
> "- `obj.func()` — STATIC. Object call, always static."
> "- `ptr->nonVirtual()` — STATIC. Non-virtual, always static."
> "- `ptr->virtualFunc()` — **DYNAMIC.** Virtual + pointer = dynamic."
> "- `ref.virtualFunc()` — **DYNAMIC.** Virtual + reference = dynamic."
> "- `ptr->Base::func()` — STATIC. Explicit scope resolution forces static."
>
> "Rule of thumb: virtual + pointer or reference = dynamic. Everything else = static."
>
> "2017 final asked exactly this. `ePtr->print()` calls the derived (dynamic), `ePtr->Employee::print()` calls Employee's (static, explicit scope)."

### Abstract Classes + Pure Virtual [0:55 — 0:59] (4 min)

**[Scroll to "Abstract Classes & Pure Virtual Functions"]**

> "An abstract class is a class with at least one PURE VIRTUAL function. Pure virtual means `virtual void func() = 0;`. The `= 0` is the magic."
>
> "Pure virtual functions have NO implementation. You're saying 'I'm declaring this exists, but derived classes MUST provide an implementation.'"
>
> "Three rules about abstract classes:"
> "1. You CANNOT instantiate an abstract class. `new Shape()` — error."
> "2. You CAN have pointers and references of abstract type. That's how polymorphism works."
> "3. If a derived class doesn't override ALL pure virtual functions, the derived class is also abstract."

**[Point to the Employee code example]**

> "Look at the Employee example. `virtual double getPay() const = 0;` — pure virtual. Different employee types calculate pay differently — Manager gets a salary, HourlyEmployee gets hourly rate. So Employee can't have a default implementation. We force derived classes to provide it."

### Virtual Destructors [0:59 — 1:01] (2 min — important!)

**[Scroll to "Virtual Destructors & Dynamic Casting"]**

> "Read this red box. This is a CLASSIC exam trap."
>
> "If you delete a derived object through a base pointer, and the destructor is NOT virtual, only the BASE destructor runs. The derived part never gets cleaned up. MEMORY LEAK."
>
> "Solution: ALWAYS make the destructor virtual in any base class that has virtual functions. `virtual ~ClassName();`"
>
> "Cost is basically zero. Always do it."

### Practice [1:01 — 1:05] (4 min)

**[Expand the Shape Hierarchy practice question]**

> "Let's walk through this practice question. Shape is abstract — pure virtual `area()` and `perimeter()`. Rectangle and Circle override them."
>
> "Look at the driver program. We create an array of Shape pointers, fill it with Rectangle and Circle objects. Then we loop and call `print()`, `area()`, `perimeter()` — all DYNAMIC binding because they're virtual functions called through pointers."
>
> "Then we delete each one — and because the destructor is virtual, the correct derived destructor runs."

**[Show the binding analysis table at the bottom]**

> "Trace through this table:"
> "- `r.area()` — object call, static, calls Rectangle's version"
> "- `sPtr->area()` — virtual via pointer, dynamic, calls Rectangle's version"
> "- `sRef.area()` — virtual via reference, dynamic, calls Rectangle's version"
>
> "All three end up calling Rectangle's area, but for DIFFERENT reasons. The compiler resolves the first one. The runtime resolves the other two."
>
> "OK — does anyone need me to go over binding again? It's the most asked question on the final."

**[Click "Operator Overloading" in the nav]**

---

## SECTION 5: OPERATOR OVERLOADING [1:05 — 1:17] (12 min)

### Exam Alert + Rules [1:05 — 1:08] (3 min)

**[On the Operator Overloading section, point to the EXAM ALERT]**

> "Operator overloading is on EVERY final. Worth 15-20 marks. The patterns they ask:"
> "- `operator<<` as a friend"
> "- `operator=` with deep copy"
> "- Comparison operators: `<`, `==`, `>`"
> "- `operator[]`"
>
> "Two questions from the 2017 final you should know:"
> "Q15 — what's the correct return type for `operator[]`? Answer: `int&` — a REFERENCE. Because you need to be able to write `obj[2] = 3` — the left side must be an l-value."
> "Q8 — does overloading `+` automatically overload `+=`? **No!** Each operator is separate."

**[Scroll to "How It Works"]**

> "When you write `a + b` for objects, C++ needs to know what `+` means for your class. So you define a function called `operator+`. The function name is the keyword 'operator' followed by the symbol."
>
> "Three rules:"
> "1. You can't create new operators — only overload existing ones."
> "2. You can't change how operators work on built-in types."
> "3. Overloading one operator does NOT overload related ones."

### Member vs Friend [1:08 — 1:11] (3 min)

**[Scroll to "Member vs Friend Functions"]**

> "Big decision: member function or friend function?"
>
> "Rule: if the LEFT operand is your class object, it's a MEMBER. If the left operand is NOT your class, it's a FRIEND."
>
> "`a + b` means `a.operator+(b)`. The left operand `a` is your class — member function."
>
> "`cout << a` means `operator<<(cout, a)`. The left operand `cout` is an `ostream`, not your class — must be a friend."

**[Point to the table]**

> "Memorize this:"
> "**MUST be a member:** `=`, `()`, `[]`, `->`"
> "**MUST be a friend:** `<<`, `>>` (because the left operand is a stream)"

### operator= [1:11 — 1:14] (3 min)

**[Scroll to "Assignment Operator (operator=)"]**

> "operator= is critical when your class has dynamic memory. Look at this pattern."
>
> "Five steps:"
> "1. **Self-assignment check:** `if (&right != this)`. Without this, `a = a` will delete its own data and try to copy from garbage."
> "2. **Delete old memory:** `delete[] ptr;`"
> "3. **Allocate new memory** based on the source size"
> "4. **Copy the data** element by element"
> "5. **Return *this** to allow chaining"
>
> "This pattern is the same every time. Memorize it."

### Practice [1:14 — 1:17] (3 min)

**[Expand the Fraction practice question]**

> "Fraction class. operator+ is a member, operator<< and operator>> are friends, operator= has the self-assignment check."
>
> "Look at operator<<: `friend ostream& operator<<(ostream& out, const Fraction& f)`. Friend, takes a stream by reference, takes the object by const reference, returns the stream by reference for chaining."
>
> "If you can write `operator<<` and `operator=` from memory, you'll get full marks on this part of the exam."

**[Click "Exception Handling" in the nav]**

---

## SECTION 6: EXCEPTION HANDLING [1:17 — 1:23] (6 min)

### Concept [1:17 — 1:20] (3 min)

**[On the Exceptions section, point to the EXAM ALERT]**

> "Exception handling rarely gets a standalone question. It's usually combined with classes — like 'the constructor throws an exception if a parameter is invalid.'"

**[Scroll to "try / throw / catch"]**

> "Three keywords:"
> "**try** — wrap the code that might fail"
> "**throw** — throw an exception when something goes wrong"
> "**catch** — handle the exception"

**[Point to the code example]**

> "Important rule: as soon as `throw` happens, the rest of the try block is SKIPPED. The program jumps straight to the matching catch."
>
> "You can throw any type — int, string, custom class object. Each catch block specifies what type it handles."

### Custom Exceptions + Practice [1:20 — 1:23] (3 min)

**[Scroll to "Custom Exception Classes"]**

> "On the exam, they usually give you exception classes. They're simple — a constructor that stores info, and a `what()` method that returns an error message."
>
> "The 2011 final had three of these — `WeightLessThanZeroException`, `FlatFeeLessThanZeroException`, `AdditionalFeeLessThanZeroException`. Each thrown from a constructor when a value is negative."

**[Expand the SafeArray practice]**

> "Pattern: constructor throws if size negative. operator[] throws if index out of bounds. Driver wraps everything in try/catch with multiple catch blocks for different exception types."

**[Click "File I/O" in the nav]**

---

## SECTION 7: FILE I/O [1:23 — 1:27] (4 min)

**[On the File I/O section]**

> "File I/O is small but it shows up. 5-10 marks usually."

**[Scroll to "File Streams"]**

> "Three classes from `<fstream>`:"
> "- `ofstream` — output, writes to file"
> "- `ifstream` — input, reads from file"
> "- `fstream` — both"

**[Show the example code]**

> "Pattern for writing: open the file, check if it failed, use `<<` just like cout, close the file."
>
> "Pattern for reading: open, check, use `>>` in a while loop, close."

**[Scroll to "Important Operations"]**

> "One trap that's been on the 2017 final: if you read to the end of a file and want to read it again, you need TWO lines:"
> "1. `file.clear()` — resets the EOF flag"
> "2. `file.seekg(0)` — moves back to the beginning"
>
> "Forget either one and you can't re-read."
>
> "Also: ALWAYS pass file streams by reference, never by value."

**[Click "Templates" in the nav]**

---

## SECTION 8: TEMPLATES & STL [1:27 — 1:30] (3 min)

**[On the Templates section, brief overview]**

> "Templates let you write generic code. Write one function or class, the compiler generates versions for any type you use."

**[Scroll through Function Templates and Class Templates quickly]**

> "Function template: `template<class T>` before the function. Then you can use T as a type inside."
>
> "Class template: same thing, but for the whole class. When you define member functions OUTSIDE the class, you need both `template<class T>` AND `ClassName<T>::funcName`."

**[Scroll to "STL: vector Basics"]**

> "STL is the Standard Template Library. The big one is `vector` — a dynamic array. Functions to know:"
> "- `push_back` / `pop_back` — add/remove from end"
> "- `front` / `back` — first/last element"
> "- `size` — number of elements"
> "- `erase` — remove at position"
> "- `begin` / `end` — for iterators"
>
> "For loops use iterators: `vector<int>::iterator it; for (it = v.begin(); it != v.end(); it++)`."
>
> "Common exam question: 'determine the output' for a vector program. We'll see one in the mock final."

---

# TRANSITION + BREAK [1:30 — 1:35] (5 min)

**[Scroll up to the Quick Reference Cheat Sheet section]**

> "Before we move to the mock final, look at this cheat sheet. These are the patterns I want you to take a screenshot of and study."
>
> "Six code blocks:"
> "1. Constructor chain pattern"
> "2. Deep copy pattern"
> "3. operator= pattern"
> "4. operator<< pattern"
> "5. Polymorphic loop pattern"
> "6. File I/O pattern"
>
> "If you can write these six patterns from memory, you'll cover 80% of the exam."
>
> "OK — let's take a 5-minute break. Stretch, get some water. We're back at [SAY THE TIME] for the mock final."

---

# PART 2 — MOCK FINAL [1:35 — 3:00]

**[Click "Mock Final" in the nav]**

---

## OPENING PART 2 [1:35 — 1:37] (2 min)

> "Welcome back. We're going to solve a complete mock final together — 100 marks plus 10 bonus. The theme is University Course Management System."
>
> "Real exam tip: before you start coding ANY question, READ the entire question first. Note what they're asking. Plan your class structure on a scrap paper. Then code."
>
> "Let's start with Q1."

---

## Q1 — THEORY [1:37 — 1:50] (13 min)

**[On Q1 — Theory / Short Answer]**

> "Q1 is theory. They give you 6 questions, you answer 4. We're going to go through ALL 6 so you're prepared for anything."

### Q1.1 — Predefined Functions (2 min)

> "Question: 'List the predefined functions for every class.'"
>
> "**Answer:** Five of them — default constructor, copy constructor, destructor, assignment operator, address-of operator."
>
> "This was on the 2004 final. Just memorize the list."

### Q1.2 — Static vs Dynamic Binding (3 min)

> "We covered this in detail. Quick recap:"
> "**Static** = compile time. Object calls, non-virtual via pointer, explicit scope resolution."
> "**Dynamic** = runtime. Virtual function via pointer or reference."
>
> "Example for static: `Rectangle r; r.area();` — always Rectangle's version."
> "Example for dynamic: `Shape* p = new Rectangle(); p->area();` — calls Rectangle's at runtime."

### Q1.3 — Self-Assignment Check (2 min)

> "**Why do we need `if (this != &other)` in operator=?**"
>
> "Because if someone writes `a = a`, you'd delete[] your own data, then try to copy from the deleted memory. The check prevents this."

### Q1.4 — Abstract vs Concrete (2 min)

> "**Abstract:** has at least one pure virtual function. Cannot instantiate."
> "**Concrete:** all functions implemented. Can instantiate."
> "**You CAN have pointers** of abstract type — used for polymorphism."
> "**You CANNOT create objects** of abstract type."

### Q1.5 — Constructor/Destructor Order (2 min)

> "**Construction:** base → derived (always)."
> "**Destruction:** derived → base (reverse)."
> "**Virtual destructor:** without it, deleting a derived object through a base pointer only runs the base destructor — memory leak."

### Q1.6 — Templates Advantage (2 min)

> "**Templates avoid duplication.** Instead of writing `int max(int, int)`, `double max(double, double)`, `string max(string, string)`, you write ONE template and the compiler generates versions for each type used."

**[Move to Q2]**

---

## Q2 — THE BIG ONE: HIERARCHY + POLYMORPHISM [1:50 — 2:20] (30 min)

> "This is the big one. 30 marks. Class hierarchy with polymorphism. This question alone is 30% of the entire exam."

**[On Q2 — Classes + Inheritance + Polymorphism]**

> "Setup: We're building a Person hierarchy. Person is abstract. Instructor and Student inherit from it. GradStudent inherits from Student."
>
> "Person has: `char* name` (dynamic!), `int id`, `static int personCount`, pure virtual `calculatePay()`."

### Part a — Person Declaration (5 min)

> "Part a — write the class declaration. 5 marks."
>
> "We need:"
> "- Constructor"
> "- Copy constructor"
> "- Virtual destructor"
> "- Pure virtual `calculatePay() = 0`"
> "- Virtual `print()`"
> "- Friend `operator<<`"
> "- Static `getPersonCount()` and `personCount`"
> "- Members in `protected` (so derived classes can access)"

**[Click to expand the solution and walk through it]**

> "Notice: virtual destructor (always for base classes), pure virtual makes it abstract, friend operator<<, static count. This is the standard template."

### Part b — Implementation (5 min)

> "Part b — implement constructor, copy constructor, destructor."
>
> "Constructor: deep copy pattern — `new char[strlen(n)+1]`, `strcpy(name, n)`, increment count."
> "Copy constructor: SAME deep copy — new memory, strcpy, increment count."
> "Destructor: `delete[] name`, decrement count."
>
> "Static initialization OUTSIDE: `int Person::personCount = 0;`"
>
> "If you can write this from memory, you get 5 free marks."

### Part c — Instructor (4 min)

> "Inherits from Person. Has `double salary` and `string department`."
>
> "Constructor calls `Person(n, i)` in initializer list, then sets salary and department."
> "`calculatePay()` returns `salary / 12`."
> "Destructor is empty — no dynamic memory in Instructor itself."

### Part d — Student (5 min)

> "This one is trickier. Student has a `double* grades` — DYNAMIC ARRAY."
>
> "Constructor: call Person constructor, then `new double[numCourses]`, copy in a loop."
> "Destructor: `delete[] grades`."
> "`calculatePay()` returns 0."

### Part e — GradStudent (3 min)

> "GradStudent inherits from Student. Has `double stipend` and `string researchArea`."
>
> "Constructor calls Student constructor, sets stipend and area."
> "`calculatePay()` returns the stipend."
> "Destructor empty."

### Part f — Driver Program (8 min — most important)

> "This is the polymorphic driver. THIS is what they want to see on the exam."

**[Show the driver code, walk through it line by line]**

> "Step 1: array of Person pointers — `Person* people[4]`."
> "Step 2: dynamically allocate one of each type — Instructor, Student, Student, GradStudent."
> "Step 3: loop through and call `print()` and `calculatePay()` — DYNAMIC binding because virtual functions through base pointers."
> "Step 4: loop through again and `delete` each one — virtual destructor ensures correct cleanup."
>
> "This pattern is the answer to like 50% of all 244 exam questions. Burn it into your brain."

**[Move to Q3]**

---

## Q3 — COMPOSITION [2:20 — 2:35] (15 min)

**[On Q3 — Class Composition]**

> "Q3 is class composition. CourseManager contains Courses, each Course tracks student IDs in a dynamic array."

### Part a — enrollStudent (5 min)

> "Two checks before adding: course full? student already enrolled?"
>
> "Loop through existing IDs to check duplicates. If neither check fails, add at `enrolledIds[enrollmentCount]` and increment."

### Part b — dropStudent (5 min)

> "Find the student. Then SHIFT the array left to fill the gap."
>
> "Pattern: `for (j = i; j < count-1; j++) arr[j] = arr[j+1]`. Then decrement count."
>
> "This shift pattern shows up A LOT — 2011 final, solution exam, all use it."

### Part c — CourseManager (5 min)

> "Constructor: allocate array of Course POINTERS — `new Course*[maxCourses]`. Initialize all to nullptr."
> "Destructor: TWO levels of cleanup — delete each Course, then delete[] the array."
> "addCourse: check duplicates by code, then `new Course(c)` — uses the copy constructor."

**[Move to Q4]**

---

## Q4 — OPERATOR OVERLOADING [2:35 — 2:50] (15 min)

**[On Q4 — Operator Overloading]**

> "Transcript class with dynamic arrays for course codes and grades. Overload several operators."

### Part a — operator+= (4 min)

> "Adds a course and grade. If we're full, double the capacity, allocate new arrays, copy, delete old."

### Part b — operator= (4 min)

> "The classic pattern again:"
> "1. Self-assignment check"
> "2. Delete old arrays"
> "3. Copy data members"
> "4. Allocate new arrays"
> "5. Element-by-element copy"
> "6. Return *this"

### Part c — operator<< (4 min)

> "Friend function. Print student ID, loop and print each course/grade, print GPA. Return the ostream reference for chaining."

### Part d — operator> (3 min)

> "One line: `return getGPA() > other.getGPA();`. Done."

**[Move to Q5]**

---

## Q5 — EXCEPTIONS + FILE I/O [2:50 — 3:00] (10 min)

**[On Q5 — Exception Handling + File I/O]**

> "Q5 combines exceptions with file I/O. We're given three custom exception classes."

### Quick walkthrough of all parts (8 min)

> "**Part a — saveEnrollments:** open ofstream, throw FileException if it fails, write course code and student count, then student IDs. Close."
>
> "**Part b — loadEnrollments:** open ifstream, throw if fails, while loop reading code and count, then a for loop reading student IDs."
>
> "**Part c — safeEnroll:** validate grade range (throw if bad), check duplicate (throw if duplicate), then enroll."
>
> "**Part d — driver:** wrap calls in try/catch blocks. Multiple catches for different exception types."

### Q6 + Closing (2 min)

> "**Q6 — bonus question on templates and STL.** Function template `findMax` for any type. STL output tracing for vector with sort/insert/pop_back."
>
> "Quick output trace for the bonus: push_back COEN244, ENGR233, COEN352, ELEC275 → sort gives lexicographic order → insert COMP248 at index 2 → pop_back removes the last → final size is 4, front is COEN244, last is ELEC275."

---

# CLOSING [3:00 — 3:05] (5 min — wrap up)

> "And that's the crash course! 3 hours, every topic, full mock final."
>
> "Things to remember when you walk into the exam:"
> "1. **Read the question fully before coding** — twice if needed"
> "2. **Deep copy pattern: `new char[strlen+1]`, `strcpy`** — write it in your sleep"
> "3. **Polymorphism = array of base pointers + virtual functions + virtual destructor**"
> "4. **operator<< is friend, operator= needs self-check**"
> "5. **Construction order: base first. Destruction: derived first.**"
>
> "If you forget everything else, remember: **virtual functions through pointers = dynamic binding.** That single concept is on every final."
>
> "After this, go through the past finals. The questions on this website are based on real ones — if you can solve these, you can solve the real exam."
>
> "Good luck! You've got this. Any last questions?"

---

# END OF SCRIPT

## Time Summary
| Block | Section | Duration |
|-------|---------|----------|
| 0:00 | Opening | 3 min |
| 0:03 | COEN 243 / Pointers | 12 min |
| 0:15 | Classes | 12 min |
| 0:27 | Composition | 8 min |
| 0:35 | Inheritance | 12 min |
| 0:47 | Polymorphism ⭐ | 18 min |
| 1:05 | Operator Overloading | 12 min |
| 1:17 | Exception Handling | 6 min |
| 1:23 | File I/O | 4 min |
| 1:27 | Templates & STL | 3 min |
| 1:30 | Cheat Sheet + Break | 5 min |
| 1:35 | Mock Final Q1 (Theory) | 13 min |
| 1:50 | Mock Final Q2 ⭐ (Hierarchy) | 30 min |
| 2:20 | Mock Final Q3 (Composition) | 15 min |
| 2:35 | Mock Final Q4 (Operators) | 15 min |
| 2:50 | Mock Final Q5+Q6 | 10 min |
| 3:00 | Closing | 5 min |
| **TOTAL** | | **3h 05min** |
