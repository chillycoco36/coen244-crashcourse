# COEN 244 CRASH COURSE — FULL PRESENTER SCRIPT v3
## 3 Hours — Every word you need to say

> **How to use:** Read the `>` quoted lines out loud. `[Brackets]` tell you what to click/scroll on the website. Time markers keep you on track. If running behind, look for `[SKIP IF SHORT ON TIME]` notes.

---

# OPENING [0:00 — 0:03]

**[Open http://localhost:3000 — hero section visible]**

> "Hey everyone, welcome to the COEN 244 crash course. I'm Chris, and for the next 3 hours we're going to go through everything you need for the final."
>
> "Here's the plan. First hour and a half — we go through every single topic. Theory, code examples, patterns. Second hour and a half — we solve a full mock final together, question by question."
>
> "I built this from real past finals — 2004, 2011, 2017, plus solution exams. When I say 'this gets tested every year,' I'm not guessing."

**[Scroll down past hero to the Exam Strategy table]**

> "Look at this table. It shows how marks are usually distributed. Polymorphism and Inheritance together — about 50% of the final. Operator overloading — another 15-20%. Those three are your top priorities."
>
> "Now — important heads up. The professor has indicated that POST-MIDTERM topics will be more heavily tested on the final. That means operator overloading, exception handling, file I/O, and templates/STL are going to carry extra weight. So don't just study inheritance and polymorphism — make sure you really know the post-midterm material too."
>
> "The number one pattern across all finals: they give you a class hierarchy, ask you to implement it with virtual functions, write a driver program with an array of base-class pointers, overload some operators, and throw in exceptions. That single pattern covers roughly 70% of the exam."
>
> "One more thing — I know you get two A4 cheat sheets for the exam. At the end of today, we'll go through exactly what to put on those sheets. I've prepared a full guide."
>
> "Any questions about the format? Good. Let's start."

**[Click "COEN 243 Review" in the nav bar]**

---

# PART 1 — THEORY & PRACTICE [0:03 — 1:30]

---

## SECTION 0: COEN 243 REVIEW [0:03 — 0:15]

**[On the COEN 243 Review section]**

> "Before we get into 244, we need to make sure the 243 foundation is solid. Especially pointers. If pointers don't click for you, nothing in this course will make sense. So we're spending about 10 minutes here."

### Variables, I/O, Control Structures [0:03 — 0:04] — skim

**[Scroll past Variables & Data Types, Input/Output, Control Structures cards quickly]**

> "Variables — int, double, char, bool, string. You know these. cin reads from keyboard with >>, cout prints to screen with <<. if/else makes decisions, for loop repeats a fixed number of times, while loop repeats as long as a condition is true. All 243 stuff, moving on."

### Functions [0:04 — 0:06]

**[Stop on the Functions section — show the pass by value vs reference cards]**

> "Functions — I want to stop here for a minute because this matters for 244."
>
> "Look at these two cards side by side. Pass by value versus pass by reference."
>
> "Pass by VALUE: the function gets a COPY of the argument. Whatever you change inside the function, the original variable is untouched. Look at the example — doubleIt takes int x. Inside, x becomes 10. But back in main, a is still 5. Because the function only changed its own copy."
>
> "Pass by REFERENCE: you add an ampersand to the parameter — int& x. Now the function has direct access to the original variable. Inside, x becomes 10 — and back in main, a is also 10. Because x IS a, not a copy of a."
>
> "Why does this matter for 244? Because objects are big — they can have tons of data members, pointers, arrays. Copying them is slow and wasteful. So in 244, we almost always pass objects by reference. And if the function isn't supposed to modify the object, we use const reference: const Time& t. This gives us speed without the risk of accidental modification."

### Pointers [0:06 — 0:13]

**[Scroll to the big red "POINTERS" heading]**

> "OK. Pointers. This is the section that separates people who survive 244 from people who struggle."
>
> "A pointer is a variable that stores a memory address. That's the entire definition. It's not magic. It's just a variable whose value happens to be an address instead of a regular number."

**[Scroll to "Declaring & Using Pointers" — show the code block]**

> "Two operators you need to memorize right now."
>
> "The ampersand — & — means 'give me the ADDRESS of this variable.' If x is an integer, then &x is the memory address where x is stored."
>
> "The star — * — means 'go to this address and give me the VALUE that's stored there.' We call this DEREFERENCING. If ptr holds an address, then *ptr means 'go to that address, give me what's there.'"
>
> "Look at this code. int x = 42. Now int* ptr = &x. What did we just do? We created a pointer called ptr, and we stored the address of x inside it."
>
> "Now look at the outputs:"
> "- cout << x prints 42 — just the value"
> "- cout << &x prints some hex address like 0x7fff — that's where x lives in memory"
> "- cout << ptr prints the same hex address — because ptr IS storing that address"
> "- cout << *ptr prints 42 — because *ptr says 'go to the address ptr is holding, and get the value there' — which is x's value"
>
> "And here's the powerful part: *ptr = 100. This goes to x's address and changes what's there to 100. We just modified x THROUGH the pointer, without ever touching x directly. Now cout << x prints 100."

**[Point to the memory visualization box]**

> "Look at this diagram if you're visual. x is at address 0x1000, holding value 42. ptr is at address 0x2000, and the value that ptr holds is 0x1000 — that's x's address. So when we dereference ptr, we follow the trail: ptr says '0x1000,' we go to 0x1000, and we find 42."

**[Scroll to "Dynamic Memory Allocation (new / delete)"]**

> "Now the critical part for this course. Dynamic memory."
>
> "So far, every variable we've created lives on the STACK — it's automatic. Created when you enter a function, destroyed when you leave. But sometimes you need memory that persists, or you don't know the size at compile time."
>
> "That's where the HEAP comes in. You allocate memory on the heap with the keyword new. new returns the address of the memory it allocated. You store that address in a pointer."
>
> "int* ptr = new int; — this creates ONE integer on the heap. ptr stores its address."
> "*ptr = 42; — now that heap integer holds 42."
> "delete ptr; — this FREES the memory. You're done with it."
>
> "For arrays: int* arr = new int[size]; — allocates an array on the heap. Use it just like a normal array: arr[0], arr[1], etc. When done: delete[] arr; — with the square brackets."

**[Point to the red important box]**

> "Critical rules. Memorize these:"
> "- Every new must have a matching delete"
> "- Every new[] must have a matching delete[] — with the brackets"
> "- Forget to delete? Memory leak — the memory is never freed, your program slowly eats RAM"
> "- Use a pointer after delete? Dangling pointer — the memory is gone but the pointer still has the old address. Undefined behavior."
>
> "Why does this matter for 244? Because almost every class you write will have a char* or a dynamic array as a data member. And that means you NEED three things: a destructor that calls delete, a copy constructor that does a deep copy, and an overloaded operator= that also does a deep copy. This is called the Rule of Three, and it shows up on every single final exam."

**[Scroll to "Common Pointer Mistakes" — show the 4 cards]**

> "Four mistakes. Burn these into your brain."
>
> "One — uninitialized pointer. You declare int* ptr but never assign it. ptr is pointing to some random garbage address. Then you do *ptr = 42 — you're writing 42 to a random memory location. Crash. Always initialize to nullptr or to a real address."
>
> "Two — memory leak. You new something, then you reassign the pointer to something else without deleting the first allocation. That first chunk of memory is now lost forever. No pointer points to it. It can never be freed."
>
> "Three — dangling pointer. You delete the memory, great. But the pointer still holds the old address. If you try to use it — *ptr — you're reading freed memory. Could be garbage, could crash. Set the pointer to nullptr after deleting."
>
> "Four — wrong delete. You allocated with new[] but you call delete without brackets. Or vice versa. Always match: new with delete, new[] with delete[]."

**[SKIP IF SHORT ON TIME: Pointer to Pointer, const with Pointers sections]**

### Wrap Up 243 [0:13 — 0:15]

> "Quick check — anyone need me to go over anything in pointers again? This stuff is the foundation for everything that follows."
>
> "There are practice exercises at the bottom of this section if you want to try them later — pointer tracing, dynamic array functions, and a bug-finding exercise. All have solutions."
>
> "OK. Now we're in 244 territory."

**[Click "Classes" in the nav bar]**

---

## SECTION 1: CLASSES [0:15 — 0:27]

**[On the Classes section]**

### Exam Alert + What is a Class [0:15 — 0:18]

**[Point to the orange EXAM ALERT box]**

> "Quick exam context. You won't get a standalone 'write a simple class' question on the final. But classes are the foundation of EVERY question. You cannot do the inheritance question without knowing constructors. You cannot do operator overloading without knowing deep copy. You cannot do the driver program without knowing how to create objects with new."
>
> "The five predefined functions the compiler generates — default constructor, copy constructor, destructor, operator=, operator& — this gets asked as an MCQ almost every year. Just memorize the list."

**[Scroll to "What is a Class?" — show the concept cards]**

> "A class is a blueprint for creating objects. It bundles DATA and FUNCTIONS together into one unit. An object is a specific INSTANCE of a class."
>
> "Four parts to every class:"
>
> "CONSTRUCTOR — this is a function that runs AUTOMATICALLY when you create an object. It has the same name as the class, and it has no return type — not even void. Its job is to initialize the data members. You can have multiple constructors — this is called overloading. A default constructor has no parameters. A parameterized constructor takes arguments."

**[Show the Time class code example on the website]**

> "DATA MEMBERS — these are the variables inside the class. They represent the attributes of the object. In this Time class, we have hour, minute, second. They're marked private — which means ONLY member functions of this class can access them. Code outside the class cannot touch them directly. This is encapsulation."
>
> "MEMBER FUNCTIONS — these are the functions inside the class. They represent what the object can DO. setTime, printTime, getHour — these are public, meaning anyone can call them. These are the interface to the outside world."
>
> "DESTRUCTOR — runs automatically when the object is destroyed. Tilde plus class name: ~Time(). No parameters, no return type, and you can only have ONE destructor — no overloading. Its job is cleanup — especially freeing dynamic memory."

### Encapsulation + Object Access [0:18 — 0:20]

**[Scroll to "Encapsulation & Access Specifiers"]**

> "Three access levels in C++:"
>
> "private — only the class's own member functions can see these. Nothing outside can touch them. This is where you put data members — to protect them."
>
> "public — anyone can access these. This is where you put member functions — the interface."
>
> "protected — like private, EXCEPT that derived classes can also access them. We'll use this in inheritance."

**[Point to the red important box about t.hour = 14]**

> "Key rule: you CANNOT access private data members directly from outside. If you write t.hour = 14 in main, that's a COMPILE ERROR. hour is private. You have to go through a public function like setTime(14, 0, 0). That's the whole point of encapsulation — controlled access."

**[Scroll to "Pointers to Objects & Dynamic Memory"]**

> "Two ways to create objects. On the stack: Time t; — automatic, destroyed when scope ends. On the heap: Time* pT = new Time(); — manual, YOU are responsible for calling delete pT."
>
> "For stack objects, use the DOT operator: t.printTime()"
> "For heap objects via pointer, use the ARROW operator: pT->printTime()"
>
> "Arrow is just shorthand. pT->printTime() means (*pT).printTime() — dereference the pointer, then use dot. But arrow is cleaner."

### Copy Constructor & Deep Copy [0:20 — 0:24]

**[Scroll to "Copy Constructor & Deep Copy"]**

> "Pay close attention here. This is the number one thing students mess up on exams."
>
> "A copy constructor creates a NEW object by copying an EXISTING object. Its parameter is a const reference to the same class type: Time(const Time& other)."
>
> "The compiler gives you a DEFAULT copy constructor for free. It copies every data member value-for-value. For simple types like int and double, this works perfectly fine."
>
> "BUT — and this is the critical part — if your class has a POINTER data member, the default copy constructor just copies the POINTER. Not what it points to. Now both objects have the same pointer, pointing to the same memory."
>
> "This is called a SHALLOW COPY. It's dangerous. If one object deletes the memory in its destructor, the other object's pointer is now dangling — pointing to freed memory."

**[Point to the Student copy constructor code on the website]**

> "So when your class has pointers, you write your OWN copy constructor that does a DEEP COPY. Look at this pattern:"
>
> "Step 1: allocate NEW memory — fname = new char[strlen(stud.fname) + 1]. The +1 is for the null terminator that C-strings need."
> "Step 2: COPY the actual data — strcpy(fname, stud.fname). Now we have our own independent copy."
>
> "This two-line pattern — new char[strlen + 1], then strcpy — you will write this SO many times on the exam. It's the deep copy pattern. Memorize it like your phone number."
>
> "When does the copy constructor get called? Three situations:"
> "1. Student s2(s1); or Student s2 = s1; — you're explicitly creating a copy"
> "2. When you pass an object BY VALUE to a function — the function needs a copy"
> "3. When you return an object BY VALUE from a function — the caller needs a copy"
>
> "That's why we pass objects by reference whenever possible — to avoid triggering unnecessary copies."

### Static Members [0:24 — 0:26]

**[Scroll to "Static Members"]**

> "Static data members belong to the CLASS, not to any individual object. Every object of the class shares the SAME static variable."
>
> "Classic exam example: a counter that tracks how many objects of the class currently exist. You declare it static int count. Increment in every constructor. Decrement in the destructor."
>
> "Two important rules about static members:"
>
> "Rule 1: you MUST initialize static data members OUTSIDE the class definition. In the .cpp file, you write: int Account::interestRate = 7.5; This is because static members need one single storage location — if you initialized inside the class, every #include would try to create a new copy."
>
> "Rule 2: you can call static member functions WITHOUT creating an object. Use the class name and scope resolution operator: Account::getInterestRate(). You don't need an object because static functions don't belong to any object — they belong to the class."

**[Quickly mention the this pointer]**

> "Quick note — every non-static member function has a hidden pointer called 'this.' It points to the current object. Useful when parameter names conflict with data member names: this->hour = hour. We'll see it again in operator=."

### Practice [0:26 — 0:27]

**[Click to expand the "BankAccount Class" practice question]**

> "Quick look at this practice problem. BankAccount has char* ownerName — that means we need the deep copy pattern. Constructor does new char and strcpy. Copy constructor does the same. Destructor does delete[]. Static count gets incremented and decremented."
>
> "Expand the solution later to study it. This exact pattern is on every exam."

**[Click "Composition" in the nav]**

---

## SECTION 2: COMPOSITION [0:27 — 0:35]

### What is Composition [0:27 — 0:30]

**[On the Composition section]**

> "Composition means a class CONTAINS an object of another class as a data member. We call this a 'has-a' relationship."
>
> "A Car HAS-A Engine. A Date HAS-A Time. A Rectangle HAS-A two Points. The containing class is called the COMPOSITE class. The contained object is the COMPONENT."
>
> "Don't confuse this with inheritance. Inheritance is 'is-a' — a Student IS-A Person. Composition is 'has-a' — a Car HAS-A Engine. They're fundamentally different relationships."

**[Show the Date/Time example on the website]**

> "Look at this Date class. It has int day, month, year — those are simple data members. But it also has Time time — a whole object of another class. That's composition."
>
> "Important: Date CANNOT access Time's private members directly. Even though Date contains a Time object, it still has to use Time's PUBLIC functions. Encapsulation still applies."

### Construction Order [0:30 — 0:31]

**[Scroll to "Construction & Destruction Order"]**

> "There's a specific order that things happen when you create a composite object:"
>
> "CONSTRUCTION: the component objects (the inner objects) are constructed FIRST, BEFORE the composite class's constructor body even starts executing. So when you create a Date, the Time object inside it is constructed first."
>
> "DESTRUCTION: the REVERSE. The composite class's destructor body runs first, then the component objects' destructors run."
>
> "This order is automatic. You don't control it. Just know it for the exam."

### Member Initializer List [0:31 — 0:34]

**[Scroll to "Member Initializer List" — show the comparison code]**

> "This is one of the most important syntax patterns in the course. The member initializer list."
>
> "When do you MUST use it? Two situations:"
> "1. The component class has NO default constructor — only a parameterized one"
> "2. You have a const data member or a reference data member"
>
> "Look at the comparison on the website. WITHOUT the initializer list, the compiler first tries to construct Engine using its default constructor Engine(). But Engine doesn't HAVE a default constructor — it only has Engine(int hp, string type). So you get a compile error."
>
> "WITH the initializer list, you write it like this after the constructor signature:"
> "Car::Car(string m, int y, int hp, string type) : model(m), year(y), engine(hp, type) { }"
>
> "See the colon after the parameter list? Then comma-separated member(value) pairs? That's the initializer list. It calls Engine(hp, type) directly — no default constructor needed."
>
> "Even when the component DOES have a default constructor, the initializer list is more efficient. Without it, the default constructor runs first, then you assign new values in the body. With the initializer list, you construct with the right values from the start — one step instead of two."

### Dynamic Arrays of Objects [0:34 — 0:35]

**[Scroll to "Dynamic Arrays of Objects"]**

> "Four ways to have collections of objects. Know all four:"
>
> "1. Time arr[40] — static array of objects. Fixed size, objects need a default constructor."
> "2. Time* ptr = new Time[size] — dynamic array of objects. Flexible size, still needs default constructor."
> "3. Time* arr[40] — static array of POINTERS. Fixed number of pointers, objects can be different sizes."
> "4. Time** arr = new Time*[size] — dynamic array of POINTERS. This is the one we use for polymorphism."
>
> "Number 4 is the most important for this course. It's how you create an array of base-class pointers that point to different derived-class objects. We'll see this over and over."

**[Click "Inheritance" in the nav]**

---

## SECTION 3: INHERITANCE [0:35 — 0:47]

### Exam Context [0:35 — 0:37]

**[Point to the orange EXAM ALERT]**

> "Inheritance is never tested by itself. It's always combined with polymorphism in the big 25-30 mark question on the final. So think of this section as building the foundation for the next one."

**[Scroll to "What is Inheritance?"]**

> "Inheritance lets you create a new class based on an existing one. The new class — the DERIVED class — inherits all the data members and member functions of the old class — the BASE class."
>
> "This is an 'is-a' relationship. A Student IS-A Person. A Circle IS-A Shape. Everything a Person can do, a Student can also do — plus more."
>
> "Three types of inheritance — public, protected, private. In this course, we basically ONLY use public. With public inheritance:"
> "- Base public members remain public in derived"
> "- Base protected members remain protected in derived"
> "- Base private members are NOT directly accessible from derived — even though they're inherited, you can't touch them. You have to go through the base class's public or protected functions."

**[Point to the concept cards]**

> "That's why we use PROTECTED instead of private for data members that derived classes need to access. Protected means: the class itself and its derived classes can see it, but nobody outside can."

### Constructor Chaining [0:37 — 0:42]

**[Scroll to "Constructors & Destructors in Inheritance"]**

> "This is the most important part of inheritance for the exam. Construction order."
>
> "When you create a derived class object, the BASE constructor runs FIRST, then the DERIVED constructor. Always. No exceptions."
>
> "When the object is destroyed, it's the REVERSE. DERIVED destructor first, then BASE destructor."
>
> "Think of it like building a house. You build the foundation (base) first, then the walls (derived). You tear down the walls first, then the foundation."

**[Show the Student constructor code]**

> "Now, how does the derived constructor tell the base constructor what to do? Through the INITIALIZER LIST."
>
> "Look at this: Student::Student(string name, int age, int id, double gpa) : Person(name, age), studentId(id), gpa(gpa) {}"
>
> "See the Person(name, age) in the initializer list? That's explicitly calling the base class constructor and passing name and age to it. The base constructor sets up the Person part. Then studentId and gpa are initialized for the Student part."

**[Point to the red important box]**

> "CRITICAL RULE: if the base class has NO default constructor — only a parameterized one — and you DON'T explicitly call it in the initializer list, you get a COMPILE ERROR. The compiler tries to call Person() which doesn't exist."
>
> "This was literally asked on the 2017 final: 'If a base class has a constructor with arguments, the derived class must...' Answer: have a constructor with arguments AND pass them to the base class constructor through the initializer list."
>
> "Also remember: base class constructors, destructors, operator=, and friend functions are NOT inherited. The derived class gets its own."

### Pointer Assignments [0:42 — 0:45]

**[Scroll to "Base & Derived Pointer Assignments" table]**

> "This table is essential. Four possible pointer assignments — know which ones are legal."
>
> "1. Base pointer pointing to a base object — Base* p = new Base(). Totally fine. Normal."
>
> "2. Derived pointer pointing to a derived object — Derived* p = new Derived(). Totally fine. Normal."
>
> "3. Base pointer pointing to a DERIVED object — Base* p = new Derived(). This IS ALLOWED. It's called UPCASTING. This is the entire basis for polymorphism."
>
> "4. Derived pointer pointing to a BASE object — Derived* p = new Base(). This is a COMPILE ERROR. You cannot point a more specific pointer at a more general object."
>
> "Number 3 is the magic one. You can have Shape* p = new Circle(). Through that Shape pointer, you can only call Shape's functions. BUT — if those functions are virtual, the Circle versions get called instead. That's polymorphism, which is our next topic."

### Practice [0:45 — 0:47]

**[Expand the Person → Student → GradStudent practice]**

> "Quick example — three-level hierarchy. GradStudent calls Student's constructor, which calls Person's. GradStudent's print() calls Student::print() first, then adds its own info."
>
> "Notice how print() uses the Base::function() syntax: Student::print(). That's how a derived class calls the base version of an overridden function."

**[Click "Polymorphism" in the nav]**

---

## SECTION 4: POLYMORPHISM [0:47 — 1:05] ⭐

### Set the Stage [0:47 — 0:49]

**[On the Polymorphism section]**

> "This is it. The most important topic of the entire course. Every single past final has a major question on polymorphism worth 20-30 marks. If you understand one thing from today, make it this."

**[Point to the red EXAM ALERT box — read the bullet points]**

> "Look at what every final asks for:"
> "- A class hierarchy with virtual and pure virtual functions"
> "- A driver program with an array of base-class pointers"
> "- Demonstrating dynamic binding through a loop"
> "- Proper use of virtual destructors"
>
> "And there's a real 2017 exam question right here. 'Employee is a base class, HourlyWorker is derived with a redefined virtual print. Will ePtr->print() and ePtr->Employee::print() give the same output?' Answer: No. First one is dynamic binding — calls HourlyWorker's version. Second one has the explicit scope Employee:: — that forces static binding, calls Employee's version."

### What is Polymorphism [0:49 — 0:51]

**[Scroll to "What is Polymorphism?"]**

> "Poly means many, morph means forms. Polymorphism means the same function call can behave differently depending on what object is actually there."
>
> "Picture a video game. You have SpaceObject as a base class. Martian, Venusian, Spaceship, Laserbeam all inherit from it. Each has its own draw() function."
>
> "A screen manager has an array of SpaceObject pointers. To refresh the screen, it loops through and calls draw() on each one. The Martian draws itself in red with antennas. The Spaceship draws a silver saucer. Same function call — draw() — completely different results."
>
> "THAT is polymorphism. One interface, many implementations."

### Virtual Functions & Binding [0:51 — 0:57]

**[Scroll to "Virtual Functions & Dynamic Binding" — show concept cards]**

> "To make polymorphism work, you need the keyword virtual. You declare the function as virtual in the base class. Derived classes override it with their own version."
>
> "Now here's the critical concept — binding. When you call a function, the program needs to decide WHICH version to call. There are two ways this decision gets made."

**[Point to the Static Binding card]**

> "STATIC BINDING — the compiler decides at COMPILE TIME which function to call. The compiler looks at the TYPE OF THE POINTER, not what it points to. This is the default behavior for non-virtual functions."
>
> "Example: Shape* ptr points to a Circle. But print() is NOT virtual. ptr->print() calls Shape::print(). The compiler sees a Shape pointer, calls Shape's version. Done. Doesn't care that it's actually pointing to a Circle."

**[Point to the Dynamic Binding card]**

> "DYNAMIC BINDING — the decision is delayed until RUNTIME. The program checks what the pointer is ACTUALLY pointing to, and calls THAT object's version. This only happens with VIRTUAL functions called through pointers or references."
>
> "Same example: Shape* ptr points to a Circle. But now print() IS virtual. ptr->print() calls Circle::print(). At runtime, the program checks 'what type of object is this really?' — it's a Circle — so it calls Circle's version."

**[Show the Shape/Circle code example]**

> "Look at this code. Shape has virtual print(). Circle overrides it. We create a Circle with a Shape pointer. sPtr->print() calls Circle's print — because virtual + pointer = dynamic binding."

**[Scroll to the Binding Rules Summary table]**

> "Here's the complete table. Memorize this. There are five cases."
>
> "1. obj.func() — object call, always STATIC. No pointer involved, compiler resolves it immediately."
>
> "2. ptr->nonVirtualFunc() — non-virtual via pointer, STATIC. The function isn't virtual, so the compiler uses the pointer type."
>
> "3. ptr->virtualFunc() — virtual via POINTER — DYNAMIC. This is the polymorphism case. Runtime decides based on the actual object."
>
> "4. ref.virtualFunc() — virtual via REFERENCE — DYNAMIC. Same as pointer. References also trigger dynamic binding for virtual functions."
>
> "5. ptr->Base::func() — explicit scope resolution — STATIC. Even if the function is virtual, putting Base:: in front forces the compiler to call the base version. Overrides dynamic binding."
>
> "The rule of thumb: ask yourself two questions. Is the function virtual? Am I calling through a pointer or reference? If BOTH are yes — dynamic. Otherwise — static."

### Abstract Classes [0:57 — 1:00]

**[Scroll to "Abstract Classes & Pure Virtual Functions"]**

> "Sometimes you have a base class where a function CAN'T have a meaningful implementation. What does it mean to calculate the area of a generic 'Shape'? You can't — you need to know if it's a circle, rectangle, triangle."
>
> "So you make the function PURE VIRTUAL by adding = 0 at the end of the declaration."
>
> "virtual double area() const = 0;"
>
> "The = 0 does two things:"
> "1. It says to derived classes: YOU MUST provide an implementation of this function."
> "2. It makes the class ABSTRACT — you cannot create objects of it."

**[Point to the Employee code example]**

> "Look at this Employee class. It has virtual double getPay() const = 0. That's pure virtual. Why? Because different employees get paid differently — Manager gets a salary, HourlyWorker gets an hourly rate. There's no sensible default implementation for a generic Employee."
>
> "Three rules about abstract classes:"
> "1. You CANNOT instantiate them. new Employee() — compile error."
> "2. You CAN have pointers and references of abstract type. Employee* ptr is perfectly legal. That's how polymorphism works."
> "3. If a derived class doesn't override ALL pure virtual functions, the derived class is ALSO abstract."

### Virtual Destructors [1:00 — 1:02]

**[Scroll to "Virtual Destructors & Dynamic Casting" — point to the red box]**

> "This is a classic exam trap. Pay attention."
>
> "Scenario: you have Shape* ptr = new Circle(). The Circle constructor allocated some memory. Now you call delete ptr."
>
> "If Shape's destructor is NOT virtual: C++ sees a Shape pointer, calls Shape's destructor. Circle's destructor NEVER RUNS. Circle's allocated memory is LEAKED."
>
> "If Shape's destructor IS virtual: C++ checks the actual object type at runtime — it's a Circle — calls Circle's destructor first, then Shape's. Everything gets cleaned up properly."
>
> "The rule: if your class has ANY virtual functions, make the destructor virtual too. virtual ~Shape(); Cost is basically zero. Benefit is enormous."

**[Show the dynamic_cast code briefly]**

> "Quick bonus — dynamic_cast. Sometimes you have a base pointer and you want to check if it actually points to a specific derived type. dynamic_cast<Circle*>(shapePtr) tries the cast. If it succeeds, you get a Circle pointer. If it fails, you get nullptr. Check for nullptr before using it."

### Practice [1:02 — 1:05]

**[Expand the "Shape Hierarchy" practice question]**

> "Let's walk through this together. Shape is abstract — pure virtual area() and perimeter(). Rectangle and Circle override both."

**[Show the driver code]**

> "The driver program. This is THE pattern:"
> "1. Create an array of Shape pointers — Shape* shapes[3]"
> "2. Fill it with different derived objects — new Rectangle, new Circle, new Rectangle"
> "3. Loop through and call virtual functions — shapes[i]->print(), shapes[i]->area(). DYNAMIC binding happens because these are virtual functions called through pointers."
> "4. Delete each one — virtual destructor ensures the right cleanup."

**[Show the binding analysis table]**

> "And look at the binding analysis:"
> "- r.area() — object call, static. Always Rectangle's version."
> "- sPtr->area() — virtual via pointer, dynamic. Calls Rectangle's version because the object is a Rectangle."
> "- sRef.area() — virtual via reference, dynamic. Same reason."
>
> "All three call Rectangle::area(), but for DIFFERENT reasons. This distinction matters on the exam."
>
> "Does anyone want me to go over binding one more time? This is the most heavily tested concept."

**[Click "Operators" in the nav]**

---

## SECTION 5: OPERATOR OVERLOADING [1:05 — 1:17]

### Exam Alert + Basics [1:05 — 1:08]

**[On the Operator Overloading section — point to the red EXAM ALERT]**

> "Operator overloading is on every final. 15-20 marks. Here's what they ask:"
> "- Implement operator<< — appears literally every time"
> "- Implement operator= with deep copy — any time the class has char*"
> "- Implement comparison operators like <, ==, > — 2011, 2017"
> "- Implement operator[] — 2011, 2017"
>
> "Two real 2017 questions: 'What's the correct return type for operator[]?' Answer: int& — a reference, not int, because you need to be able to write obj[2] = 3. 'Does overloading + automatically overload +=?' Answer: No. Each operator is separate."

**[Scroll to "How It Works"]**

> "In C++, operators like +, -, ==, << are actually function calls behind the scenes. When you write 5 + 3, the compiler knows how to add integers. But when you write fraction1 + fraction2 — C++ has no idea what + means for YOUR class."
>
> "So you define it. The function name is the keyword operator followed by the symbol: operator+, operator<<, operator=."
>
> "Three rules:"
> "1. You can't create NEW operators — only overload existing ones"
> "2. You can't change how operators work on built-in types like int"
> "3. Overloading + does NOT automatically overload +=. They're separate."

### Member vs Friend [1:08 — 1:11]

**[Scroll to "Member vs Friend Functions" — show the table]**

> "Here's the big question for every operator: should it be a MEMBER function or a FRIEND function?"
>
> "The rule is about the LEFT operand."
>
> "If the LEFT operand is an object of YOUR class — make it a MEMBER function. When you write a + b, that becomes a.operator+(b). The object on the left calls the function. The 'this' pointer is the left operand."
>
> "If the LEFT operand is NOT your class — make it a FRIEND function. When you write cout << myObj, the left operand is cout — that's an ostream, not your class. You can't add a member function to ostream. So you make it a standalone function and declare it friend so it can access your private members."
>
> "Memorize these two lists:"
> "MUST be member: =, (), [], ->"
> "MUST be friend: <<, >> — because the left operand is always a stream"

**[Show the Rational class example code]**

> "Look at operator+ as a member: Rational Rational::operator+(const Rational& x) const. Left operand is 'this' (the Rational on the left). Returns a new Rational."
>
> "Look at operator<< as a friend: friend ostream& operator<<(ostream& out, const Rational& r). NOT a member of Rational — it's a friend. Takes ostream by reference on the left, Rational by const reference on the right. Returns ostream& so you can chain: cout << a << b."

### operator= [1:11 — 1:14]

**[Scroll to "Assignment Operator (operator=)"]**

> "operator= matters when your class has dynamic memory. The default does a shallow copy — just like the default copy constructor. Both objects end up pointing to the same memory. Disaster."
>
> "So you write your own. There's a SPECIFIC five-step pattern:"

**[Point to the code example]**

> "Step 1: SELF-ASSIGNMENT CHECK. if (&right != this). Why? Because if someone writes a = a, you'd delete your own data in step 2, then try to copy from it in step 3. You'd be copying from freed memory — garbage."
>
> "Step 2: DELETE the old memory. delete[] ptr. You're about to get new data, so free the old stuff first."
>
> "Step 3: ALLOCATE new memory. size = right.getSize(); ptr = new int[size]."
>
> "Step 4: COPY the data. Loop through and copy element by element."
>
> "Step 5: RETURN *this. This allows chaining: a = b = c."
>
> "This pattern is identical every time. Memorize it."

### Practice [1:14 — 1:17]

**[Expand the Fraction practice]**

> "Fraction class. Let's look at the solution."
>
> "operator+ is a member function — returns a new Fraction with the computed numerator and denominator."
>
> "operator<< is a friend — takes ostream& out and const Fraction& f. Prints f.numerator / f.denominator. Returns out."
>
> "operator= — self-assignment check, copy the two ints, return *this."
>
> "Why is << a friend? Because the LEFT operand is cout, which is an ostream. Not our class. Must be friend."
>
> "If you can write operator<< and operator= from memory, you'll get full marks on the operator question. These two patterns alone cover most of what they ask."

**[Click "Exceptions" in the nav]**

---

## SECTION 6: EXCEPTION HANDLING [1:17 — 1:23]

### Exam Context [1:17 — 1:18]

**[Point to the EXAM ALERT]**

> "Exception handling rarely gets its own standalone question. Instead, they weave it INTO other questions. The 2011 final had constructors that throw exceptions when weight is negative. The solution exam had a delete_policy function that throws if the policy isn't found. The 2017 final had an output tracing question — 'what does this try/catch print?'"

### How It Works [1:18 — 1:21]

**[Scroll to "try / throw / catch"]**

> "Three keywords. Simple concept."
>
> "TRY — you put code that might fail inside a try block. You're saying 'watch this code for problems.'"
>
> "THROW — when something goes wrong, you throw an exception. throw 'Division by zero'; or throw NegativeSizeException(s);. You can throw anything — an int, a string, a custom class object."
>
> "CATCH — this is where you handle the problem. Each catch block specifies what TYPE it handles. catch (int e) handles thrown integers. catch (string msg) handles thrown strings."

**[Point to the code example]**

> "Now the CRITICAL rule about flow. The MOMENT a throw happens, EVERYTHING after it in the try block is SKIPPED. The program jumps directly to the first matching catch block."
>
> "In this example: line A runs, then throw happens, lines B and C NEVER run. The program jumps straight to catch."
>
> "If you have multiple catch blocks, only the FIRST one that matches the thrown type runs. The rest are skipped."
>
> "If NO catch block matches, the exception PROPAGATES — it goes up to the calling function. If nobody catches it, the program terminates."

### Custom Exceptions + Practice [1:21 — 1:23]

**[Scroll to "Custom Exception Classes"]**

> "On the exam, they define custom exception classes. They're simple — just a class with a constructor that stores some error info, and a what() method that returns a string describing the error."
>
> "The typical exam pattern: a constructor validates its parameters and throws if something's wrong."

**[Expand SafeArray practice briefly]**

> "SafeArray: constructor throws NegativeSizeException if size < 0. operator[] throws IndexOutOfBoundsException if index is out of range. The driver wraps everything in try with multiple catch blocks. Clean and simple."

**[Click "File I/O" in the nav]**

---

## SECTION 7: FILE I/O [1:23 — 1:27]

**[On the File I/O section]**

**[Point to EXAM ALERT]**

> "File I/O is usually 5-10 marks. They ask you to write data to a file, read it back, or search a file for a record."

**[Scroll to "File Streams"]**

> "You need #include <fstream>. Three classes:"
> "- ofstream — output file stream. You WRITE to it. Think 'o' for output."
> "- ifstream — input file stream. You READ from it. Think 'i' for input."
> "- fstream — does both."
>
> "The beautiful thing: they work EXACTLY like cout and cin. Same << and >> operators. Just replace cout with your ofstream object, and cin with your ifstream object."

**[Show the code examples]**

> "Writing pattern: create ofstream, check if it opened — if (!outFile) { exit(1); } — use << just like cout, close when done."
>
> "Reading pattern: create ifstream, check if opened, use >> in a while loop — while (inFile >> id >> name >> grade) — this loop automatically stops at end of file. Close when done."

**[Scroll to "Important Operations"]**

> "One exam trap — this was on the 2017 final. You read a file to the end, now you want to read it again. Maybe for a different query. You CAN'T just start another while loop."
>
> "When you reach the end of a file, an internal EOF flag gets set. That flag blocks further reading. You need to CLEAR it, then REWIND the position."
>
> "Two lines: file.clear(); then file.seekg(0);"
>
> "clear() resets the EOF flag. seekg(0) moves the read position back to byte 0 — the beginning."
>
> "You need BOTH lines. clear() alone just clears the flag but you're still at the end. seekg(0) alone tries to move but the EOF flag blocks it."
>
> "One more rule: ALWAYS pass file streams by REFERENCE to functions. Never by value. Streams can't be copied properly."

**[Click "Templates" in the nav]**

---

## SECTION 8: TEMPLATES & STL [1:27 — 1:30]

**[On the Templates section]**

**[Point to EXAM ALERT]**

> "Templates and STL are usually 5-10 marks. Mostly MCQ or 'determine the output.' Not a huge chunk but it's free marks if you know the patterns."

### Function Templates [1:27 — 1:28]

**[Show the printArray template]**

> "Templates let you write one function that works with any type. Instead of writing separate functions for int, double, string, you write ONE template."
>
> "Syntax: template<class T> before the function. Then use T wherever you'd normally put a specific type."
>
> "When you call printArray(intArr, 5), the compiler sees T = int and generates an int version. When you call printArray(doubleArr, 3), it generates a double version. You wrote it once."

### Class Templates [1:28 — 1:29]

**[Show the Stack<T> example]**

> "Same idea for classes. template<class T> before the class. Use T as the type for data members and functions."
>
> "Key syntax: when you define member functions OUTSIDE the class, you need BOTH template<class T> AND Stack<T>::functionName. Not just Stack:: — you need the <T>."
>
> "Usage: Stack<double> doubleStack(5); or Stack<int> intStack(10);. You specify the type in angle brackets."
>
> "2017 final Q10 asked which destructor syntax is correct for Stack<T>. Answer: template<class T> Stack<T>::~Stack() { delete[] stackPtr; }. Need both template line and <T>."

### STL Vectors [1:29 — 1:30]

**[Scroll to "STL: vector Basics"]**

> "STL is the Standard Template Library. The one to know is vector — a dynamic array that handles its own memory."
>
> "Functions to know: push_back adds to the end, pop_back removes from the end, size returns count, front and back return first and last elements, erase removes at a position."
>
> "Looping with iterators: vector<int>::iterator it; for (it = v.begin(); it != v.end(); it++) cout << *it;"
>
> "Sorting: sort(v.begin(), v.end()) from <algorithm>."
>
> "Common exam question: they give you a block of code with push_back, sort, erase, insert, and ask 'what's the output?' Trace step by step — write down the vector after each operation."

---

## TRANSITION + BREAK [1:30 — 1:35]

**[Scroll to the Quick Reference Cheat Sheet]**

> "Before we break, look at this cheat sheet. Six code patterns that cover 80% of the exam:"
> "1. Constructor chain — Derived::Derived(args) : Base(args), member(args) {}"
> "2. Deep copy — new char[strlen+1], strcpy"
> "3. operator= — self-check, delete old, deep copy, return *this"
> "4. operator<< — friend, ostream& return, const& object"
> "5. Polymorphic loop — Base* arr[], virtual calls, delete with virtual destructor"
> "6. File I/O — open, check, read/write, close. Re-read: clear() + seekg(0)"
>
> "Take a screenshot of this. Study it tonight."
>
> "Let's take a 5-minute break. Be back at [SAY THE TIME] for the mock final."

---

# PART 2 — MOCK FINAL [1:35 — 3:00]

**[Click "Mock Final" in the nav]**

## Opening [1:35 — 1:37]

> "Welcome back. Now we solve a full 100-mark mock final together. Theme: University Course Management System."
>
> "Exam tip: before you code ANYTHING, read the ENTIRE question. Understand what they want. Then plan your approach — sketch the class hierarchy on paper. THEN code."

---

## Q1 — THEORY / SHORT ANSWER [1:37 — 1:50] (13 min)

**[On Q1]**

> "Q1 is theory. Six questions, pick four. Let's go through all six."

### 1.1 — Predefined Functions (2 min)

> "'List the predefined member functions for all classes.'"
>
> "Five of them. Memorize this list:"
> "1. Default constructor"
> "2. Copy constructor"
> "3. Destructor"
> "4. Assignment operator (operator=)"
> "5. Address-of operator (operator&)"
>
> "These are compiler-generated defaults. They work fine for simple classes. But the moment you have dynamic memory, the defaults for copy constructor, destructor, and operator= are wrong — you need to write your own."

### 1.2 — Static vs Dynamic Binding (3 min)

> "'Explain the difference with examples.'"
>
> "Static binding: compiler decides at compile time. Looks at the pointer/variable type."
> "Example: Rectangle r(5,3); r.area(); — the compiler sees r is a Rectangle, calls Rectangle::area(). Done at compile time."
>
> "Dynamic binding: decided at runtime. Looks at the actual object type."
> "Example: Shape* p = new Rectangle(5,3); p->area(); — at runtime, checks what p points to, finds Rectangle, calls Rectangle::area()."
>
> "Key: dynamic only happens when the function is virtual AND called through a pointer or reference."

### 1.3 — Self-Assignment Check (2 min)

> "'Why does operator= need a self-assignment check?'"
>
> "Consider a = a. Without the check:"
> "Step 1: delete[] data — you just deleted your own data"
> "Step 2: data = new char[strlen(other.data)+1] — but other IS you. other.data was just deleted. strlen of freed memory = garbage."
> "Step 3: strcpy from garbage."
>
> "The fix: if (this != &other) at the top. If they're the same object, skip everything and just return *this."

### 1.4 — Abstract vs Concrete (2 min)

> "'What's the difference? Can you have pointers of abstract type?'"
>
> "Abstract class: has at least one pure virtual function (= 0). CANNOT create objects of it."
> "Concrete class: all functions implemented. CAN create objects."
>
> "You CAN have pointers and references of abstract type — Shape* ptr is legal. You just can't do new Shape(). The pointer is how you achieve polymorphism."

### 1.5 — Constructor/Destructor Order (2 min)

> "'What's the order? Why virtual destructor?'"
>
> "Construction: base first, then derived."
> "Destruction: derived first, then base."
>
> "Virtual destructor: without it, delete basePtr only runs the base destructor. Derived class memory leaks. With virtual destructor, the correct derived destructor runs first, then base. Proper cleanup."

### 1.6 — Templates vs Overloading (2 min)

> "'Advantage of templates?'"
>
> "Templates avoid code duplication. Instead of writing max() for int, double, and string separately — three functions with identical logic — you write ONE template. The compiler generates the versions automatically."

---

## Q2 — THE BIG ONE [1:50 — 2:20] (30 min)

**[On Q2 — Classes + Inheritance + Polymorphism, 30 marks]**

> "This is 30 marks — 30% of the exam in one question. Person hierarchy: Person (abstract) → Instructor, Student → GradStudent."
>
> "Person has char* name (dynamic!), int id, static int personCount, pure virtual calculatePay()."

### Part a — Person Declaration (5 min)

**[Expand solution, walk through declaration]**

> "We need: constructor, copy constructor, virtual destructor, pure virtual calculatePay() = 0, virtual print(), friend operator<<, static getPersonCount()."
>
> "Data members in PROTECTED — not private — so derived classes can access name and id."
>
> "Notice: virtual destructor (mandatory for any base class), = 0 makes it abstract, friend on operator<< because left operand will be ostream."

### Part b — Person Implementation (5 min)

> "Constructor: the deep copy pattern. id gets assigned directly. name needs new char[strlen(n)+1] then strcpy(name, n). Increment personCount."
>
> "Copy constructor: exact same deep copy. New memory, strcpy, increment count."
>
> "Destructor: delete[] name. Decrement personCount."
>
> "Static initialization OUTSIDE the class: int Person::personCount = 0;"
>
> "This block of code — constructor, copy constructor, destructor — is 5 free marks if you've memorized the deep copy pattern."

### Part c — Instructor (4 min)

> "Inherits from Person publicly. New data: double salary, string department."
>
> "Constructor: Instructor(const char* n, int i, double s, const string& d) : Person(n, i), salary(s), department(d) {}."
>
> "Calls Person(n, i) in the initializer list — passing name and id to the base constructor."
>
> "calculatePay() returns salary / 12.0 — monthly pay."
>
> "Destructor is empty — Instructor has no dynamic memory of its own. The base destructor handles name."

### Part d — Student (5 min)

> "This one is more complex. Student has double* grades — a DYNAMIC ARRAY."
>
> "Constructor: call Person(n, i) in initializer list. Then allocate grades = new double[numCourses]. Loop and copy each grade from the parameter array."
>
> "Destructor: delete[] grades. This is why we need a destructor — dynamic memory."
>
> "calculatePay() returns 0.0 — students don't get paid."
>
> "grades is PROTECTED, not private — because GradStudent inherits from Student and might need it."

### Part e — GradStudent (3 min)

> "Inherits from Student. New data: double stipend, string researchArea."
>
> "Constructor calls Student(n, i, g, num) in initializer list. Sets stipend and researchArea."
>
> "calculatePay() returns stipend."
>
> "Destructor empty — no new dynamic memory in GradStudent itself. Student's destructor handles grades, Person's handles name."

### Part f — Driver Program (8 min)

> "This is where it all comes together. The polymorphic driver."

**[Walk through the code line by line]**

> "Line 1: Person* people[4]; — array of four Person POINTERS. Not Person objects — pointers."
>
> "Lines 2-5: fill the array with different types."
> "people[0] = new Instructor('Dr. Smith', 1001, 96000, 'COEN');"
> "people[1] = new Student('Alice', 40123456, grades1, 3);"
> "people[2] = new Student('Bob', 40234567, grades2, 2);"
> "people[3] = new GradStudent('Carol', 40345678, grades3, 4, 2000, 'AI');"
>
> "This is UPCASTING — storing derived class objects in base class pointers."
>
> "The loop: for (int i = 0; i < 4; i++) { people[i]->print(); people[i]->calculatePay(); }"
>
> "Both print() and calculatePay() are virtual. Called through Person pointers. So DYNAMIC BINDING happens. people[0]->calculatePay() calls Instructor::calculatePay() which returns 96000/12 = 8000. people[3]->calculatePay() calls GradStudent::calculatePay() which returns 2000."
>
> "Cleanup: for (int i = 0; i < 4; i++) delete people[i];"
>
> "Virtual destructor ensures: for people[1], Student's destructor runs (delete[] grades), then Person's destructor runs (delete[] name). Proper cleanup."
>
> "THIS PATTERN — array of base pointers, polymorphic loop, proper deletion — is the answer to the biggest question on every exam."

---

## Q3 — COMPOSITION [2:20 — 2:35] (15 min)

**[On Q3]**

> "CourseManager manages Courses. Each Course has a dynamic array of enrolled student IDs."

### Part a — enrollStudent (5 min)

> "Two checks before adding:"
> "1. Is the course full? if (enrollmentCount >= maxCapacity) return false;"
> "2. Is this student already enrolled? Loop through and check. If found, return false."
> "If both pass: enrolledIds[enrollmentCount] = studentId; enrollmentCount++; return true;"

### Part b — dropStudent (5 min)

> "Find the student by looping. When found, SHIFT the array left to fill the gap."
> "The shift: for (j = i; j < enrollmentCount - 1; j++) enrolledIds[j] = enrolledIds[j+1];"
> "Then enrollmentCount--. Return true."
> "If not found after the loop, return false."
>
> "This array-shifting pattern appears on the 2011 final and the solution exam."

### Part c — CourseManager (5 min)

> "Constructor: courses = new Course*[maxCourses]; — array of Course POINTERS. Initialize all to nullptr in a loop."
>
> "Destructor: TWO levels of cleanup. First delete each Course: for (i = 0; i < numCourses; i++) delete courses[i]. Then delete[] courses — the array itself."
>
> "addCourse: check not full, check no duplicate by comparing course codes, then courses[numCourses] = new Course(c) — uses the copy constructor to create a new Course on the heap."

---

## Q4 — OPERATOR OVERLOADING [2:35 — 2:50] (15 min)

**[On Q4]**

> "Transcript class. Dynamic arrays for course codes and grades."

### Part a — operator+= (4 min)

> "Adds a course/grade pair. If arrays are full, resize — double the capacity, allocate new arrays, copy old data, delete old arrays, update capacity. Then add the new course and grade at position numCourses, increment."

### Part b — operator= (4 min)

> "The classic pattern:"
> "1. Self-assignment: if (this != &other)"
> "2. Delete old: delete[] courseCodes; delete[] grades;"
> "3. Copy scalars: studentId, numCourses, capacity"
> "4. Allocate new: courseCodes = new string[capacity]; grades = new double[capacity];"
> "5. Copy arrays in a loop"
> "6. Return *this"

### Part c — operator<< (4 min)

> "Friend function. Print student ID, loop through courses/grades printing each, print GPA. Return out."
>
> "Key: it's a friend because left operand is ostream. Takes ostream& and const Transcript&. Returns ostream& for chaining."

### Part d — operator> (3 min)

> "One line: return getGPA() > other.getGPA(). That's it."

---

## Q5 — EXCEPTIONS + FILE I/O [2:50 — 3:00] (10 min)

**[On Q5]**

> "Three custom exception classes are given. We implement functions that use them."

### Parts a-d walkthrough (8 min)

> "Part a — saveEnrollments: open ofstream. If it fails, throw FileException(filename). Write each course's code and enrollment count, then student IDs one per line. Close."
>
> "Part b — loadEnrollments: open ifstream. Throw if fails. Read course code and count in a while loop, create a Course, for loop to read and enroll each student ID, add course to manager. Close. Return the manager."
>
> "Part c — safeEnroll: if grade < 0 or > 4.3, throw InvalidGradeException. If student already enrolled, throw DuplicateEnrollmentException. Otherwise call enrollStudent."
>
> "Part d — driver: try block calls safeEnroll multiple times. Catch InvalidGradeException and DuplicateEnrollmentException separately. Second try block for file operations, catch FileException."

### Q6 + Wrap up (2 min)

> "Q6 is bonus — template function findMax, and an STL output tracing question."
>
> "findMax: template<class T>, loop through array, track max, return it."
>
> "Output trace: push_back four strings, sort alphabetically, insert at position 2, pop_back, print size/front/back."

---

# CHEAT SHEET GUIDE [3:00 — 3:10]

**[Click "Cheat Sheet" in the nav]**

> "OK before we wrap up — let's talk about your cheat sheet. You get two A4 sheets, front and back. That's four pages. Here's exactly how to use them."

**[Scroll through the three cheat sheet pages on the website]**

> "PAGE 1 — Code Patterns. This is your MOST IMPORTANT page. Write the actual code templates you'll need on the exam:"
>
> "1. Deep copy pattern — the new char[strlen+1], strcpy block. Write it for constructor AND copy constructor."
> "2. operator= — with the self-assignment check, delete old, deep copy, return *this."
> "3. operator<< — the friend function with ostream& return."
> "4. operator>> — same pattern but istream."
> "5. Polymorphic driver — Base* arr[N], fill with derived, loop with virtual calls, delete."
> "6. Constructor chain — Derived::Derived(args) : Base(args), member(val) {}"
> "7. File I/O — write and read patterns, plus clear() + seekg(0)."
> "8. Exception handling — try/throw/catch template."
>
> "These are the EXACT code blocks you'll be writing on the exam. Having them on your sheet means you just adapt them to whatever scenario they give you."

> "PAGE 2 — Rules and quick reference. Write the binding rules table, operator rules (must be member vs must be friend), construction/destruction order, the five predefined functions, Rule of Three, const with pointers, template syntax, STL vector functions."

> "PAGES 3 and 4 — A full worked example. Pick one complete class hierarchy — either from your assignments or from the mock final Q2 on this website. Write out the entire thing: base class, derived class, constructors, operator<<, operator=, driver program. If you have room, add a file I/O example and an exception example."

> "Pro tip: on the exam, every question is basically a VARIATION of the same patterns. Different class names, different data members, but the STRUCTURE is identical. If you have one full worked example, you can adapt it to anything."

---

# EXTRA PRACTICE + CLOSING [3:10 — 3:15]

**[Click "Extra Practice" in the nav]**

> "Last thing — on the website there are extra practice problems that we didn't have time to cover today. These are available for you to work through on your own:"
>
> "Extra 1 — IntArray class with operator[], operator=, operator==, operator<<. Full operator overloading practice."
>
> "Extra 2 — File I/O with exception handling combined. Read a file, validate data, write filtered output."
>
> "Extra 3 — A full class template DynamicArray<T> with add, operator[], resize, operator<<. Great template practice."
>
> "Extra 4 — STL output tracing. A vector program with sort, erase, insert, push_back, pop_back. Trace the output step by step."
>
> "All four have complete solutions. These are specifically focused on POST-MIDTERM topics since the prof said those carry more weight."

> "Five things to remember walking into the exam:"
>
> "1. Read the ENTIRE question before coding. Plan first."
>
> "2. Deep copy pattern — new char[strlen+1], strcpy — you'll write it multiple times."
>
> "3. Polymorphism pattern — array of base pointers, virtual functions, virtual destructor."
>
> "4. operator<< is always a friend. operator= always has a self-assignment check."
>
> "5. POST-MIDTERM topics carry more weight — operator overloading, exceptions, file I/O, templates."
>
> "One sentence to rule them all: VIRTUAL FUNCTIONS THROUGH POINTERS EQUALS DYNAMIC BINDING."
>
> "The website stays online — use it to review, practice the extra problems, and build your cheat sheet. Good luck on the exam! You've got this."

---

# TIME SUMMARY

| Block | Section | Duration |
|-------|---------|----------|
| 0:00 | Opening + Exam Strategy | 3 min |
| 0:03 | COEN 243 Review / Pointers | 12 min |
| 0:15 | Classes | 12 min |
| 0:27 | Composition | 8 min |
| 0:35 | Inheritance | 12 min |
| 0:47 | **Polymorphism** ⭐ | 18 min |
| 1:05 | Operator Overloading | 12 min |
| 1:17 | Exception Handling | 6 min |
| 1:23 | File I/O | 4 min |
| 1:27 | Templates & STL | 3 min |
| 1:30 | Cheat Sheet + Break | 5 min |
| 1:35 | Mock Final Opening | 2 min |
| 1:37 | Q1 — Theory | 13 min |
| 1:50 | **Q2 — Hierarchy + Polymorphism** ⭐ | 30 min |
| 2:20 | Q3 — Composition | 15 min |
| 2:35 | Q4 — Operators | 15 min |
| 2:50 | Q5+Q6 — Exceptions/IO/Templates | 10 min |
| 3:00 | Cheat Sheet Guide | 10 min |
| 3:10 | Extra Practice + Closing | 5 min |
| **TOTAL** | | **~3h 15min** |
