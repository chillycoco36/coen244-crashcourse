# COEN 244 Crash Course — Part 1: Practice Questions Per Topic

---

## Topic 1: Classes (12 min)

### Practice Question: BankAccount Class

Design and implement a class `BankAccount` with the following specifications:

```cpp
class BankAccount {
public:
    BankAccount();                              // Default constructor
    BankAccount(int id, const char* owner, double balance);  // General constructor
    BankAccount(const BankAccount& other);      // Copy constructor
    ~BankAccount();                             // Destructor

    double getBalance() const;
    int getId() const;
    void deposit(double amount);
    bool withdraw(double amount);
    void print() const;

    static int getCount();

private:
    int accountId;
    char* ownerName;      // dynamically allocated
    double balance;
    static int count;     // tracks number of BankAccount objects
};
```

**Tasks:**

a) **(4 marks)** Implement the **general constructor**. It must:
- Dynamically allocate memory for `ownerName` using `strcpy`
- Initialize all data members
- Increment the static `count`

b) **(3 marks)** Implement the **copy constructor**. Ensure it performs a **deep copy**.

c) **(2 marks)** Implement the **destructor**. Ensure proper memory deallocation and decrement `count`.

d) **(1 mark)** Initialize the static member `count` outside the class.

### Solution:

```cpp
// Static member initialization
int BankAccount::count = 0;

// a) General constructor
BankAccount::BankAccount(int id, const char* owner, double bal)
    : accountId(id), balance(bal) {
    ownerName = new char[strlen(owner) + 1];
    strcpy(ownerName, owner);
    count++;
}

// b) Copy constructor — deep copy
BankAccount::BankAccount(const BankAccount& other)
    : accountId(other.accountId), balance(other.balance) {
    ownerName = new char[strlen(other.ownerName) + 1];
    strcpy(ownerName, other.ownerName);
    count++;
}

// c) Destructor
BankAccount::~BankAccount() {
    delete[] ownerName;
    count--;
}

// d) Already shown above: int BankAccount::count = 0;
```

**Key takeaways to emphasize:**
- `char*` requires dynamic allocation → need copy constructor, destructor, assignment operator (Rule of 3)
- `static` members are shared across all objects, initialized outside the class
- Copy constructor must do **deep copy** (not just copy the pointer)
- `const` correctness on parameters and member functions

---

## Topic 2: Class Composition (8 min)

### Practice Question: Engine and Car

Given the following `Engine` class:

```cpp
class Engine {
public:
    Engine(int hp, const string& type);
    ~Engine();
    int getHorsepower() const;
    string getType() const;
    void print() const;
private:
    int horsepower;
    string type;  // "V6", "V8", "Electric"
};
```

**Tasks:**

a) **(4 marks)** Define and implement a class `Car` that **has-a** `Engine` (composition). The `Car` class must have:
- Data members: `string model`, `int year`, `Engine engine`
- A constructor that initializes all members using the **member initializer list**

b) **(2 marks)** What is the **order of construction** when a `Car` object is created? What is the **order of destruction**?

c) **(2 marks)** Why must we use the **member initializer list** to initialize the `Engine` member?

### Solution:

```cpp
class Car {
public:
    Car(const string& model, int year, int hp, const string& engineType);
    ~Car();
    void print() const;
private:
    string model;
    int year;
    Engine engine;  // composition: Car "has-a" Engine
};

// a) Constructor with member initializer list
Car::Car(const string& m, int y, int hp, const string& engineType)
    : model(m), year(y), engine(hp, engineType) {
    // body can be empty — all initialization done in initializer list
}

Car::~Car() {
    // Engine destructor called automatically after Car destructor body
}
```

**b) Construction order:**
1. `Engine` constructor is called first (member object)
2. `Car` constructor body executes

**Destruction order (reverse):**
1. `Car` destructor body executes
2. `Engine` destructor is called automatically

**c)** Because `Engine` has no default constructor (only a parameterized one). If we don't use the initializer list, the compiler tries to call `Engine()` which doesn't exist → **compilation error**.

**Key takeaways:**
- Composition = "has-a" relationship
- Member objects are constructed **before** the containing object's constructor body
- Destroyed in **reverse** order
- Must use initializer list when member class has no default constructor

---

## Topic 3: Inheritance (12 min)

### Practice Question: Person → Student → GradStudent

```cpp
class Person {
public:
    Person(const string& name, int age);
    virtual ~Person();
    virtual void print() const;
protected:
    string name;
    int age;
};

class Student : public Person {
public:
    Student(const string& name, int age, int studentId, double gpa);
    ~Student();
    void print() const;
protected:
    int studentId;
    double gpa;
};
```

**Tasks:**

a) **(4 marks)** Implement the constructors for `Person` and `Student`. The `Student` constructor must call the `Person` constructor via the initializer list.

b) **(4 marks)** Define and implement a class `GradStudent` that inherits from `Student` with:
- Additional data members: `string thesisTopic`, `string supervisor`
- Constructor that initializes all members (including inherited ones)

c) **(2 marks)** Implement `print()` for `GradStudent`. It should call the base class `print()` and then print the additional members.

d) **(2 marks)** If `Person` did **not** have a default constructor, and a programmer writes:
```cpp
Student::Student(const string& n, int a, int id, double g) {
    studentId = id;
    gpa = g;
}
```
What happens? Why?

### Solution:

```cpp
// a) Constructors
Person::Person(const string& n, int a) : name(n), age(a) {}

Student::Student(const string& n, int a, int id, double g)
    : Person(n, a), studentId(id), gpa(g) {}

// b) GradStudent class
class GradStudent : public Student {
public:
    GradStudent(const string& name, int age, int id, double gpa,
                const string& thesis, const string& supervisor);
    ~GradStudent();
    void print() const;
private:
    string thesisTopic;
    string supervisor;
};

GradStudent::GradStudent(const string& n, int a, int id, double g,
                         const string& t, const string& s)
    : Student(n, a, id, g), thesisTopic(t), supervisor(s) {}

// c) print() calling base class
void GradStudent::print() const {
    Student::print();  // calls Student's print (which could call Person's print)
    cout << "Thesis: " << thesisTopic << endl;
    cout << "Supervisor: " << supervisor << endl;
}
```

**d)** Compilation error. Without explicitly calling `Person(n, a)` in the initializer list, the compiler tries to call `Person()` (default constructor). Since `Person` has no default constructor, it fails.

**Key takeaways:**
- Derived constructors **must** call base constructor if no default exists
- Construction order: base → derived. Destruction order: derived → base
- Use `Base::function()` to call overridden functions
- `protected` members are accessible in derived classes but not outside

---

## Topic 4: Polymorphism (18 min) ⭐ MOST IMPORTANT

### Practice Question: Shape Hierarchy

```cpp
class Shape {
public:
    Shape(const string& name);
    virtual ~Shape();
    virtual double area() const = 0;       // pure virtual
    virtual double perimeter() const = 0;  // pure virtual
    virtual void print() const;
private:
    string shapeName;
};

class Rectangle : public Shape {
public:
    Rectangle(double w, double h);
    ~Rectangle();
    double area() const;
    double perimeter() const;
    void print() const;
private:
    double width, height;
};

class Circle : public Shape {
public:
    Circle(double r);
    ~Circle();
    double area() const;
    double perimeter() const;
    void print() const;
private:
    double radius;
};
```

**Tasks:**

a) **(4 marks)** Implement `area()` and `perimeter()` for both `Rectangle` and `Circle`.

b) **(4 marks)** Can you create an object of type `Shape`? Why or why not? What kind of class is `Shape`?

c) **(6 marks)** Write a driver program that:
- Creates an array of 3 `Shape*` pointers
- Dynamically allocates `Rectangle` and `Circle` objects
- Uses a loop to call `area()` and `print()` on each — demonstrating **polymorphism**
- Properly deletes all objects

d) **(4 marks)** For each line below, state whether the binding is **static** or **dynamic**, and what function executes:

```cpp
Rectangle r(5, 3);
Shape* sPtr = &r;
Shape& sRef = r;

r.area();           // Line 1
sPtr->area();       // Line 2
sRef.area();        // Line 3
sPtr->Shape::area();// Line 4
```

### Solution:

```cpp
// a) Implementations
double Rectangle::area() const { return width * height; }
double Rectangle::perimeter() const { return 2 * (width + height); }

double Circle::area() const { return 3.14159 * radius * radius; }
double Circle::perimeter() const { return 2 * 3.14159 * radius; }

// b) No. Shape is an ABSTRACT class because it has pure virtual functions
//    (area and perimeter). You cannot instantiate abstract classes.

// c) Driver program
int main() {
    const int SIZE = 3;
    Shape* shapes[SIZE];

    shapes[0] = new Rectangle(5.0, 3.0);
    shapes[1] = new Circle(4.0);
    shapes[2] = new Rectangle(10.0, 2.0);

    for (int i = 0; i < SIZE; i++) {
        shapes[i]->print();           // dynamic binding — calls correct print()
        cout << "Area: " << shapes[i]->area() << endl;  // dynamic binding
        cout << "Perimeter: " << shapes[i]->perimeter() << endl;
    }

    // Clean up — virtual destructor ensures proper deletion
    for (int i = 0; i < SIZE; i++) {
        delete shapes[i];
    }

    return 0;
}
```

**d) Binding analysis:**

| Line | Binding | Function Called | Why |
|------|---------|----------------|-----|
| `r.area()` | **Static** | `Rectangle::area()` | Called by object (not pointer/reference) |
| `sPtr->area()` | **Dynamic** | `Rectangle::area()` | Virtual function called via pointer |
| `sRef.area()` | **Dynamic** | `Rectangle::area()` | Virtual function called via reference |
| `sPtr->Shape::area()` | **Static** | `Shape::area()` | Explicit scope resolution → static binding (but this would fail since Shape::area() is pure virtual) |

**Key takeaways:**
- **Abstract class** = has at least one pure virtual function (`= 0`)
- **Static binding**: object calls, non-virtual function calls, explicit `Base::func()` calls
- **Dynamic binding**: virtual function called through pointer or reference
- **Virtual destructor** is essential when deleting derived objects through base pointers
- The **type of the object** (not the pointer) determines which function runs in dynamic binding

---

## Topic 5: Operator Overloading (12 min)

### Practice Question: Fraction Class

```cpp
class Fraction {
public:
    Fraction(int num = 0, int den = 1);

    // Arithmetic
    Fraction operator+(const Fraction& other) const;
    Fraction operator-(const Fraction& other) const;

    // Comparison
    bool operator==(const Fraction& other) const;
    bool operator<(const Fraction& other) const;

    // Assignment
    const Fraction& operator=(const Fraction& other);

    // I/O (friends)
    friend ostream& operator<<(ostream& out, const Fraction& f);
    friend istream& operator>>(istream& in, Fraction& f);

private:
    int numerator;
    int denominator;
};
```

**Tasks:**

a) **(3 marks)** Implement `operator+`:

b) **(3 marks)** Implement `operator<<` and `operator>>`:

c) **(3 marks)** Implement `operator=`:

d) **(3 marks)** Why must `operator<<` and `operator>>` be **friend functions** and not member functions?

### Solution:

```cpp
// a) operator+
Fraction Fraction::operator+(const Fraction& other) const {
    int num = numerator * other.denominator + other.numerator * denominator;
    int den = denominator * other.denominator;
    return Fraction(num, den);
}

// b) operator<< and operator>>
ostream& operator<<(ostream& out, const Fraction& f) {
    out << f.numerator << "/" << f.denominator;
    return out;
}

istream& operator>>(istream& in, Fraction& f) {
    cout << "Enter numerator: ";
    in >> f.numerator;
    cout << "Enter denominator: ";
    in >> f.denominator;
    return in;
}

// c) operator=
const Fraction& Fraction::operator=(const Fraction& other) {
    if (this != &other) {  // self-assignment check
        numerator = other.numerator;
        denominator = other.denominator;
    }
    return *this;
}
```

**d)** Because the **left operand** of `<<` is `ostream` (not `Fraction`) and the left operand of `>>` is `istream` (not `Fraction`). Member functions require the left operand to be an object of the class. Since `cout << f` means `operator<<(cout, f)`, it cannot be a member of `Fraction`. We make them `friend` so they can access private members.

**Key takeaways:**
- `<<` and `>>` must be **friend** (left operand is not our class)
- `=`, `()`, `[]`, `->` **must** be member functions
- Always check for **self-assignment** in `operator=`
- Return `ostream&` / `istream&` to allow chaining: `cout << a << b`
- When class has pointers: `operator=` needs **deep copy** (like copy constructor)

---

## Topic 6: Exception Handling (8 min)

### Practice Question: SafeArray with Exceptions

```cpp
class IndexOutOfBoundsException {
public:
    IndexOutOfBoundsException(int idx, int maxSize);
    string what() const;
private:
    int index;
    int size;
};

class NegativeSizeException {
public:
    NegativeSizeException(int s);
    string what() const;
private:
    int size;
};

class SafeArray {
public:
    SafeArray(int size);
    ~SafeArray();
    int& operator[](int index);
    int getSize() const;
private:
    int* arr;
    int size;
};
```

**Tasks:**

a) **(3 marks)** Implement the `SafeArray` constructor that **throws** `NegativeSizeException` if size is negative.

b) **(3 marks)** Implement `operator[]` that **throws** `IndexOutOfBoundsException` if index is out of range.

c) **(2 marks)** Write a driver that creates a `SafeArray`, accesses an invalid index, and **catches** the exception.

### Solution:

```cpp
// Exception implementations
IndexOutOfBoundsException::IndexOutOfBoundsException(int idx, int maxSize)
    : index(idx), size(maxSize) {}

string IndexOutOfBoundsException::what() const {
    return "Index " + to_string(index) + " out of bounds. Size: " + to_string(size);
}

NegativeSizeException::NegativeSizeException(int s) : size(s) {}

string NegativeSizeException::what() const {
    return "Negative size: " + to_string(size);
}

// a) Constructor
SafeArray::SafeArray(int s) {
    if (s < 0) throw NegativeSizeException(s);
    size = s;
    arr = new int[size];
}

// b) operator[]
int& SafeArray::operator[](int index) {
    if (index < 0 || index >= size)
        throw IndexOutOfBoundsException(index, size);
    return arr[index];
}

// c) Driver
int main() {
    try {
        SafeArray sa(5);
        sa[0] = 10;
        sa[1] = 20;
        cout << sa[0] << endl;  // prints 10
        cout << sa[10] << endl; // throws exception
    }
    catch (IndexOutOfBoundsException& e) {
        cout << "Error: " << e.what() << endl;
    }
    catch (NegativeSizeException& e) {
        cout << "Error: " << e.what() << endl;
    }
    return 0;
}
```

**Key takeaways:**
- `throw` sends the exception, `catch` handles it
- Custom exception classes store useful info (what went wrong)
- Multiple `catch` blocks handle different exception types
- Code after `throw` in the `try` block does **not** execute
- Exception propagation: if not caught, it goes up the call stack

---

## Topic 7: File I/O (8 min)

### Practice Question: Student Records File

**Tasks:**

a) **(3 marks)** Write a function that **writes** an array of student records to a sequential file:

```cpp
struct StudentRecord {
    int id;
    string name;
    double gpa;
};

void writeRecords(const StudentRecord records[], int count, const string& filename);
```

b) **(3 marks)** Write a function that **reads** all records from the file and prints them:

```cpp
void readAndPrint(const string& filename);
```

c) **(2 marks)** Write a function that **searches** for a student by ID in the file and returns their GPA. Return `-1` if not found.

### Solution:

```cpp
// a) Write to file
void writeRecords(const StudentRecord records[], int count, const string& filename) {
    ofstream outFile(filename, ios::out);
    if (!outFile) {
        cerr << "Error opening file for writing." << endl;
        exit(1);
    }
    for (int i = 0; i < count; i++) {
        outFile << records[i].id << " "
                << records[i].name << " "
                << records[i].gpa << endl;
    }
    outFile.close();
}

// b) Read and print
void readAndPrint(const string& filename) {
    ifstream inFile(filename, ios::in);
    if (!inFile) {
        cerr << "Error opening file for reading." << endl;
        exit(1);
    }
    int id;
    string name;
    double gpa;
    while (inFile >> id >> name >> gpa) {
        cout << "ID: " << id << " Name: " << name << " GPA: " << gpa << endl;
    }
    inFile.close();
}

// c) Search by ID
double searchByID(const string& filename, int searchId) {
    ifstream inFile(filename, ios::in);
    if (!inFile) {
        cerr << "Error opening file." << endl;
        exit(1);
    }
    int id;
    string name;
    double gpa;
    while (inFile >> id >> name >> gpa) {
        if (id == searchId) {
            inFile.close();
            return gpa;
        }
    }
    inFile.close();
    return -1;  // not found
}
```

**Key takeaways:**
- `ofstream` → write, `ifstream` → read, `fstream` → both
- Always check if file opened successfully
- `inFile >> var` reads whitespace-separated values
- `getline(inFile, line)` reads a full line
- Always `close()` the file when done
- To re-read a file: `file.clear()` then `file.seekg(0)`

---

## Topic 8: Templates & STL (8 min)

### Practice Question: Generic Pair Template + STL

**Part A — Class Template (4 marks):**

Define and implement a class template `Pair<T1, T2>` that:
- Stores two values of potentially different types
- Has a constructor, getters, and overloaded `operator<<`

**Part B — STL (4 marks):**

Determine the output of the following program:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> v;
    v.push_back(30);
    v.push_back(10);
    v.push_back(50);
    v.push_back(20);
    v.push_back(40);

    cout << "Size: " << v.size() << endl;           // Line 1

    sort(v.begin(), v.end());

    vector<int>::iterator it;
    for (it = v.begin(); it != v.end(); it++)
        cout << *it << " ";                          // Line 2
    cout << endl;

    v.erase(v.begin() + 1);

    for (it = v.begin(); it != v.end(); it++)
        cout << *it << " ";                          // Line 3
    cout << endl;

    cout << "Front: " << v.front() << endl;          // Line 4
    cout << "Back: " << v.back() << endl;             // Line 5

    return 0;
}
```

### Solution:

**Part A:**
```cpp
template <class T1, class T2>
class Pair {
public:
    Pair(T1 first, T2 second) : first(first), second(second) {}

    T1 getFirst() const { return first; }
    T2 getSecond() const { return second; }

    friend ostream& operator<<(ostream& out, const Pair<T1, T2>& p) {
        out << "(" << p.first << ", " << p.second << ")";
        return out;
    }

private:
    T1 first;
    T2 second;
};

// Usage:
int main() {
    Pair<string, int> student("Alice", 40123456);
    Pair<double, double> point(3.5, 7.2);
    cout << student << endl;  // (Alice, 40123456)
    cout << point << endl;    // (3.5, 7.2)
    return 0;
}
```

**Part B — Output:**
```
Size: 5
10 20 30 40 50
10 30 40 50
Front: 10
Back: 50
```

**Explanation:**
- Line 1: vector has 5 elements → `Size: 5`
- After `sort()`: `{10, 20, 30, 40, 50}`
- Line 2: prints sorted: `10 20 30 40 50`
- `erase(v.begin() + 1)` removes element at index 1 (which is 20)
- Line 3: `10 30 40 50`
- Line 4: `front()` returns first element: `10`
- Line 5: `back()` returns last element: `50`

**Key takeaways:**
- Templates use `template<class T>` before class/function
- Use template notation: `ClassName<T>::functionName()`
- STL `vector`: `push_back()`, `size()`, `front()`, `back()`, `erase()`, `begin()`, `end()`
- Iterators: declare with `vector<type>::iterator` or `vector<type>::const_iterator`
- `sort()` from `<algorithm>` sorts in ascending order by default

---
