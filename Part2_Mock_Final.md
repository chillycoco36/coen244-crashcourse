# COEN 244 Crash Course — Part 2: Mock Final Exam

## Theme: University Course Management System

**Duration:** 3 hours | **Total:** 100 marks

---

## QUESTION 1 — Theory / Short Answer (20 marks)

Answer any **four (4)** of the following **six (6)** questions. (5 marks each)

**1.1** List the member functions that are **predefined** (compiler-generated) for all classes in C++.

**1.2** Explain the difference between **static binding** and **dynamic binding**. Give one example of each using a base class pointer.

**1.3** Why does the `operator=` need to perform a **self-assignment check**? What can go wrong if it doesn't, especially when the class has dynamically allocated memory?

**1.4** What is the difference between an **abstract class** and a **concrete class**? Can you have pointers of an abstract class type? Can you have objects?

**1.5** In an inheritance hierarchy, what is the **order of constructor and destructor calls** when creating and destroying a derived class object? Why is a **virtual destructor** important?

**1.6** What is the advantage of using **templates** instead of function overloading? Give an example.

### Solutions:

**1.1:** Default constructor, copy constructor, destructor, assignment operator (`operator=`), address-of operator (`operator&`).

**1.2:**
- **Static binding**: function to call is determined at **compile time**. Happens with: object calls, non-virtual functions via pointers.
  - Example: `Rectangle r(5,3); r.area();` → always calls `Rectangle::area()`
- **Dynamic binding**: function to call is determined at **run time** based on the actual object type. Happens with: virtual functions called via pointer/reference.
  - Example: `Shape* p = new Rectangle(5,3); p->area();` → calls `Rectangle::area()` at runtime

**1.3:** If `operator=` doesn't check for self-assignment (`a = a`), and the class has a pointer:
```cpp
const A& A::operator=(const A& other) {
    delete[] data;                    // deletes our own data!
    data = new char[strlen(other.data) + 1]; // other.data is now garbage
    strcpy(data, other.data);         // copies garbage
    return *this;
}
```
Self-assignment check: `if (this != &other)` prevents this.

**1.4:**
- **Abstract class**: has at least one pure virtual function (`virtual void f() = 0;`). Cannot instantiate objects.
- **Concrete class**: all functions are implemented. Can instantiate objects.
- You **can** have pointers/references of abstract class type (for polymorphism). You **cannot** create objects of an abstract class.

**1.5:**
- Construction: base constructor → derived constructor
- Destruction: derived destructor → base destructor
- Virtual destructor is important because: when you `delete` a derived object through a base pointer, without virtual destructor only the base destructor runs → **memory leak** (derived members not cleaned up).

**1.6:** Templates avoid code duplication. Instead of writing:
```cpp
int max(int a, int b);
double max(double a, double b);
string max(string a, string b);
```
You write one template: `template<class T> T max(T a, T b);`
The compiler generates the correct version for each type used.

---

## QUESTION 2 — Classes + Inheritance + Polymorphism (30 marks)

We are building a university course management system. Consider the following class hierarchy:

```
         Person (abstract)
        /              \
   Instructor        Student
                        |
                  GradStudent
```

The base class `Person` stores:
- `char* name` (dynamically allocated)
- `int id`
- `static int personCount` (tracks total Person objects)

**a) (5 marks)** Provide the **complete class declaration** for `Person`:
- General constructor, copy constructor, destructor
- Pure virtual function: `virtual double calculatePay() const = 0;`
- Virtual function: `virtual void print() const;`
- Overloaded `operator<<` (friend)

**b) (5 marks)** Implement the **constructor, copy constructor, and destructor** for `Person`.

**c) (5 marks)** Declare the class `Instructor` that inherits publicly from `Person` with:
- `double salary` (annual salary)
- `string department`
- Implement the constructor and `calculatePay()` which returns `salary / 12` (monthly pay)

**d) (5 marks)** Declare the class `Student` that inherits publicly from `Person` with:
- `double* grades` (dynamically allocated array)
- `int numCourses`
- Implement the constructor, destructor, and `calculatePay()` which returns `0` (students don't get paid)

**e) (5 marks)** Declare `GradStudent` inheriting from `Student` with:
- `double stipend` (monthly stipend)
- `string researchArea`
- Override `calculatePay()` to return the `stipend`

**f) (5 marks)** Write a **driver program** that:
- Creates an array of 4 `Person*` pointers
- Dynamically allocates: 1 Instructor, 2 Students, 1 GradStudent
- Loops through to call `print()` and `calculatePay()` (demonstrating polymorphism)
- Deletes all objects properly

### Solution:

```cpp
// ===================== a) Person Declaration =====================
class Person {
public:
    Person(const char* name, int id);
    Person(const Person& other);
    virtual ~Person();

    virtual double calculatePay() const = 0;  // pure virtual
    virtual void print() const;

    static int getPersonCount();

    friend ostream& operator<<(ostream& out, const Person& p);

protected:
    char* name;
    int id;
    static int personCount;
};

// ===================== b) Person Implementation =====================
int Person::personCount = 0;

Person::Person(const char* n, int i) : id(i) {
    name = new char[strlen(n) + 1];
    strcpy(name, n);
    personCount++;
}

Person::Person(const Person& other) : id(other.id) {
    name = new char[strlen(other.name) + 1];
    strcpy(name, other.name);
    personCount++;
}

Person::~Person() {
    delete[] name;
    personCount--;
}

void Person::print() const {
    cout << "Name: " << name << ", ID: " << id << endl;
}

ostream& operator<<(ostream& out, const Person& p) {
    out << "Name: " << p.name << ", ID: " << p.id;
    return out;
}

int Person::getPersonCount() { return personCount; }

// ===================== c) Instructor =====================
class Instructor : public Person {
public:
    Instructor(const char* name, int id, double salary, const string& dept);
    ~Instructor();
    double calculatePay() const;
    void print() const;
private:
    double salary;
    string department;
};

Instructor::Instructor(const char* n, int i, double s, const string& d)
    : Person(n, i), salary(s), department(d) {}

Instructor::~Instructor() {}

double Instructor::calculatePay() const {
    return salary / 12.0;
}

void Instructor::print() const {
    Person::print();
    cout << "Department: " << department << ", Salary: $" << salary << endl;
}

// ===================== d) Student =====================
class Student : public Person {
public:
    Student(const char* name, int id, const double* grades, int numCourses);
    ~Student();
    double calculatePay() const;
    void print() const;
protected:
    double* grades;
    int numCourses;
};

Student::Student(const char* n, int i, const double* g, int num)
    : Person(n, i), numCourses(num) {
    grades = new double[numCourses];
    for (int j = 0; j < numCourses; j++) {
        grades[j] = g[j];
    }
}

Student::~Student() {
    delete[] grades;
}

double Student::calculatePay() const {
    return 0.0;
}

void Student::print() const {
    Person::print();
    cout << "Courses: " << numCourses << ", Grades: ";
    for (int i = 0; i < numCourses; i++)
        cout << grades[i] << " ";
    cout << endl;
}

// ===================== e) GradStudent =====================
class GradStudent : public Student {
public:
    GradStudent(const char* name, int id, const double* grades, int numCourses,
                double stipend, const string& area);
    ~GradStudent();
    double calculatePay() const;
    void print() const;
private:
    double stipend;
    string researchArea;
};

GradStudent::GradStudent(const char* n, int i, const double* g, int num,
                         double s, const string& a)
    : Student(n, i, g, num), stipend(s), researchArea(a) {}

GradStudent::~GradStudent() {}

double GradStudent::calculatePay() const {
    return stipend;
}

void GradStudent::print() const {
    Student::print();
    cout << "Research: " << researchArea << ", Stipend: $" << stipend << endl;
}

// ===================== f) Driver Program =====================
int main() {
    const int SIZE = 4;
    Person* people[SIZE];

    double grades1[] = {3.5, 3.7, 4.0};
    double grades2[] = {3.0, 2.8};
    double grades3[] = {3.9, 4.0, 3.8, 3.7};

    people[0] = new Instructor("Dr. Smith", 1001, 96000.0, "COEN");
    people[1] = new Student("Alice Johnson", 40123456, grades1, 3);
    people[2] = new Student("Bob Wilson", 40234567, grades2, 2);
    people[3] = new GradStudent("Carol Lee", 40345678, grades3, 4, 2000.0, "AI");

    cout << "Total persons: " << Person::getPersonCount() << endl;

    // Polymorphic processing
    for (int i = 0; i < SIZE; i++) {
        people[i]->print();  // dynamic binding
        cout << "Monthly pay: $" << people[i]->calculatePay() << endl;
        cout << "---" << endl;
    }

    // Clean up (virtual destructor ensures correct deletion)
    for (int i = 0; i < SIZE; i++) {
        delete people[i];
    }

    cout << "Remaining persons: " << Person::getPersonCount() << endl;
    return 0;
}
```

---

## QUESTION 3 — Class Composition (15 marks)

The class `CourseManager` manages a collection of `Course` objects. Each `Course` contains a roster of students.

```cpp
class Course {
public:
    Course(const string& code, const string& title, int capacity);
    Course(const Course& other);
    ~Course();
    const Course& operator=(const Course& other);

    bool enrollStudent(int studentId);
    bool dropStudent(int studentId);
    bool isEnrolled(int studentId) const;
    int getEnrollmentCount() const;
    string getCode() const;
    void print() const;

private:
    string courseCode;    // e.g., "COEN244"
    string courseTitle;   // e.g., "Programming Methodology II"
    int* enrolledIds;     // dynamic array of student IDs
    int enrollmentCount;
    int maxCapacity;
};
```

**a) (5 marks)** Implement `enrollStudent()` — adds a student ID to the array if:
- The student is not already enrolled
- The course is not full
Returns `true` if successful, `false` otherwise.

**b) (5 marks)** Implement `dropStudent()` — removes a student ID by **shifting** remaining elements left. Returns `false` if student not found.

**c) (5 marks)** Define and implement `CourseManager`:
```cpp
class CourseManager {
public:
    CourseManager(int maxCourses);
    ~CourseManager();
    bool addCourse(const Course& c);
    bool removeCourse(const string& courseCode);
    void listAllCourses() const;
private:
    Course** courses;    // dynamic array of Course pointers
    int numCourses;
    int maxCourses;
};
```
Implement the **constructor**, **destructor**, and `addCourse()`.

### Solution:

```cpp
// a) enrollStudent
bool Course::enrollStudent(int studentId) {
    if (enrollmentCount >= maxCapacity)
        return false;

    // Check if already enrolled
    for (int i = 0; i < enrollmentCount; i++) {
        if (enrolledIds[i] == studentId)
            return false;
    }

    enrolledIds[enrollmentCount] = studentId;
    enrollmentCount++;
    return true;
}

// b) dropStudent
bool Course::dropStudent(int studentId) {
    for (int i = 0; i < enrollmentCount; i++) {
        if (enrolledIds[i] == studentId) {
            // Shift elements left
            for (int j = i; j < enrollmentCount - 1; j++) {
                enrolledIds[j] = enrolledIds[j + 1];
            }
            enrollmentCount--;
            return true;
        }
    }
    return false;  // student not found
}

// c) CourseManager
CourseManager::CourseManager(int max) : numCourses(0), maxCourses(max) {
    courses = new Course*[maxCourses];
    for (int i = 0; i < maxCourses; i++) {
        courses[i] = nullptr;
    }
}

CourseManager::~CourseManager() {
    for (int i = 0; i < numCourses; i++) {
        delete courses[i];
    }
    delete[] courses;
}

bool CourseManager::addCourse(const Course& c) {
    if (numCourses >= maxCourses)
        return false;

    // Check for duplicate
    for (int i = 0; i < numCourses; i++) {
        if (courses[i]->getCode() == c.getCode())
            return false;
    }

    courses[numCourses] = new Course(c);  // uses copy constructor
    numCourses++;
    return true;
}
```

---

## QUESTION 4 — Operator Overloading (15 marks)

The class `Transcript` stores a student's course grades. It overloads several operators.

```cpp
class Transcript {
public:
    Transcript(int studentId);
    Transcript(const Transcript& other);
    ~Transcript();

    // Add a grade (course code + grade)
    Transcript& operator+=(const pair<string, double>& courseGrade);

    // Compare GPAs
    bool operator>(const Transcript& other) const;
    bool operator==(const Transcript& other) const;

    // Assignment
    const Transcript& operator=(const Transcript& other);

    // I/O
    friend ostream& operator<<(ostream& out, const Transcript& t);

    double getGPA() const;

private:
    int studentId;
    string* courseCodes;   // dynamic array
    double* grades;        // dynamic array
    int numCourses;
    int capacity;
};
```

**a) (4 marks)** Implement `operator+=` — adds a new course and grade to the transcript:

**b) (4 marks)** Implement `operator=` with proper deep copy:

**c) (4 marks)** Implement `operator<<` that outputs:
```
Student ID: 40123456
COEN244: 3.7
ENGR233: 3.3
GPA: 3.50
```

**d) (3 marks)** Implement `operator>` that compares two transcripts by their GPA.

### Solution:

```cpp
// a) operator+=
Transcript& Transcript::operator+=(const pair<string, double>& courseGrade) {
    if (numCourses >= capacity) {
        // Resize arrays (double capacity)
        int newCap = capacity * 2;
        string* newCodes = new string[newCap];
        double* newGrades = new double[newCap];
        for (int i = 0; i < numCourses; i++) {
            newCodes[i] = courseCodes[i];
            newGrades[i] = grades[i];
        }
        delete[] courseCodes;
        delete[] grades;
        courseCodes = newCodes;
        grades = newGrades;
        capacity = newCap;
    }
    courseCodes[numCourses] = courseGrade.first;
    grades[numCourses] = courseGrade.second;
    numCourses++;
    return *this;
}

// b) operator=
const Transcript& Transcript::operator=(const Transcript& other) {
    if (this != &other) {  // self-assignment check
        // Deallocate old memory
        delete[] courseCodes;
        delete[] grades;

        // Deep copy
        studentId = other.studentId;
        numCourses = other.numCourses;
        capacity = other.capacity;
        courseCodes = new string[capacity];
        grades = new double[capacity];
        for (int i = 0; i < numCourses; i++) {
            courseCodes[i] = other.courseCodes[i];
            grades[i] = other.grades[i];
        }
    }
    return *this;
}

// c) operator<<
ostream& operator<<(ostream& out, const Transcript& t) {
    out << "Student ID: " << t.studentId << endl;
    for (int i = 0; i < t.numCourses; i++) {
        out << t.courseCodes[i] << ": " << t.grades[i] << endl;
    }
    out << "GPA: " << t.getGPA() << endl;
    return out;
}

// d) operator>
bool Transcript::operator>(const Transcript& other) const {
    return getGPA() > other.getGPA();
}
```

---

## QUESTION 5 — Exception Handling + File I/O (20 marks)

We want to save and load course data with proper error handling.

**Given exception classes:**

```cpp
class FileException {
public:
    FileException(const string& filename) : filename(filename) {}
    string what() const { return "Cannot open file: " + filename; }
private:
    string filename;
};

class InvalidGradeException {
public:
    InvalidGradeException(double grade) : grade(grade) {}
    string what() const {
        return "Invalid grade: " + to_string(grade) + ". Must be between 0.0 and 4.3.";
    }
private:
    double grade;
};

class DuplicateEnrollmentException {
public:
    DuplicateEnrollmentException(int id, const string& code) : studentId(id), courseCode(code) {}
    string what() const {
        return "Student " + to_string(studentId) + " already enrolled in " + courseCode;
    }
private:
    int studentId;
    string courseCode;
};
```

**a) (6 marks)** Implement a function that **writes** all course enrollment data to a file. Throws `FileException` if file cannot be opened:

```cpp
void saveEnrollments(const CourseManager& manager, const string& filename);
```

The file format should be:
```
COEN244 3
40123456
40234567
40345678
ENGR233 2
40123456
40456789
```
(course code, number of students, then one student ID per line)

**b) (6 marks)** Implement a function that **reads** enrollment data from the file and rebuilds the `CourseManager`. Throws `FileException` if file cannot be opened:

```cpp
CourseManager* loadEnrollments(const string& filename, int maxCourses);
```

**c) (4 marks)** Implement a **safe enrollment function** that:
- Validates grade is between 0.0 and 4.3 (throws `InvalidGradeException`)
- Checks for duplicate enrollment (throws `DuplicateEnrollmentException`)

```cpp
void safeEnroll(Course& course, int studentId, double grade);
```

**d) (4 marks)** Write a **driver program** that calls the functions above with proper `try/catch` blocks handling all three exception types.

### Solution:

```cpp
// a) Save enrollments to file
void saveEnrollments(const CourseManager& manager, const string& filename) {
    ofstream outFile(filename, ios::out);
    if (!outFile) {
        throw FileException(filename);
    }

    for (int i = 0; i < manager.getNumCourses(); i++) {
        Course* c = manager.getCourse(i);
        outFile << c->getCode() << " " << c->getEnrollmentCount() << endl;
        for (int j = 0; j < c->getEnrollmentCount(); j++) {
            outFile << c->getStudentId(j) << endl;
        }
    }
    outFile.close();
}

// b) Load enrollments from file
CourseManager* loadEnrollments(const string& filename, int maxCourses) {
    ifstream inFile(filename, ios::in);
    if (!inFile) {
        throw FileException(filename);
    }

    CourseManager* manager = new CourseManager(maxCourses);
    string courseCode;
    int numStudents;

    while (inFile >> courseCode >> numStudents) {
        Course course(courseCode, "", numStudents);
        for (int i = 0; i < numStudents; i++) {
            int studentId;
            inFile >> studentId;
            course.enrollStudent(studentId);
        }
        manager->addCourse(course);
    }

    inFile.close();
    return manager;
}

// c) Safe enrollment with validation
void safeEnroll(Course& course, int studentId, double grade) {
    if (grade < 0.0 || grade > 4.3) {
        throw InvalidGradeException(grade);
    }
    if (course.isEnrolled(studentId)) {
        throw DuplicateEnrollmentException(studentId, course.getCode());
    }
    course.enrollStudent(studentId);
}

// d) Driver program
int main() {
    CourseManager manager(10);
    Course coen244("COEN244", "Programming Methodology II", 50);
    Course engr233("ENGR233", "Applied Advanced Calculus", 60);

    manager.addCourse(coen244);
    manager.addCourse(engr233);

    // Safe enrollment with exception handling
    try {
        safeEnroll(coen244, 40123456, 3.7);
        safeEnroll(coen244, 40234567, 3.5);
        safeEnroll(coen244, 40123456, 3.9);  // duplicate — throws!
    }
    catch (InvalidGradeException& e) {
        cout << "Grade Error: " << e.what() << endl;
    }
    catch (DuplicateEnrollmentException& e) {
        cout << "Enrollment Error: " << e.what() << endl;
    }

    // File I/O with exception handling
    try {
        saveEnrollments(manager, "enrollments.dat");
        cout << "Data saved successfully." << endl;

        CourseManager* loaded = loadEnrollments("enrollments.dat", 10);
        loaded->listAllCourses();
        delete loaded;
    }
    catch (FileException& e) {
        cout << "File Error: " << e.what() << endl;
    }

    return 0;
}
```

---

## QUESTION 6 — Templates + STL (bonus-style, if time allows)

**(a) (5 marks)** Write a **function template** `findMax` that takes an array of any type and its size, and returns the maximum element. The type must support `operator>`.

```cpp
template <class T>
T findMax(const T arr[], int size) {
    T max = arr[0];
    for (int i = 1; i < size; i++) {
        if (arr[i] > max)
            max = arr[i];
    }
    return max;
}
```

**(b) (5 marks)** Determine the output:

```cpp
int main() {
    vector<string> courses;
    courses.push_back("COEN244");
    courses.push_back("ENGR233");
    courses.push_back("COEN352");
    courses.push_back("ELEC275");

    sort(courses.begin(), courses.end());

    vector<string>::const_iterator it;
    for (it = courses.begin(); it != courses.end(); it++)
        cout << *it << " ";
    cout << endl;

    courses.insert(courses.begin() + 2, "COMP248");

    for (it = courses.begin(); it != courses.end(); it++)
        cout << *it << " ";
    cout << endl;

    courses.pop_back();
    cout << "Size: " << courses.size() << endl;
    cout << "First: " << courses.front() << endl;
    cout << "Last: " << courses.back() << endl;

    return 0;
}
```

**Output:**
```
COEN244 COEN352 ELEC275 ENGR233
COEN244 COEN352 COMP248 ELEC275 ENGR233
Size: 4
First: COEN244
Last: ELEC275
```

**Explanation:**
- `sort()` sorts strings lexicographically: COEN244 < COEN352 < ELEC275 < ENGR233
- `insert(begin()+2, "COMP248")` inserts at index 2 → pushes others right
- `pop_back()` removes last element (ENGR233)
- Size is now 4, front = COEN244, back = ELEC275

---

## Summary: Mark Distribution

| Question | Topic | Marks |
|----------|-------|-------|
| Q1 | Theory / Short Answer | 20 |
| Q2 | Classes + Inheritance + Polymorphism | 30 |
| Q3 | Class Composition | 15 |
| Q4 | Operator Overloading | 15 |
| Q5 | Exception Handling + File I/O | 20 |
| Q6 | Templates + STL (bonus) | 10 |
| **Total** | | **100 (+10 bonus)** |

This mirrors the real exam distribution seen across all past finals.

---
