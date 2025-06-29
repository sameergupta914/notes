## Object-Oriented Programming (OOP) in C++ – Interview-Focused Notes

### 1. Introduction to OOP

OOP is a paradigm centered around the concept of objects, which encapsulate data and methods. It enhances modularity, reusability, and maintainability.

---

### 2. Key Components of OOP

- **Class**: Blueprint for creating objects.
- **Object**: Instance of a class.
- **Attributes**: Variables holding state.
- **Methods**: Functions representing behavior.
- **Encapsulation**: Wrapping data and methods.
- **Abstraction**: Hiding complex implementation.
- **Inheritance**: Acquiring features from a base class.
- **Polymorphism**: Different behaviors via the same interface.

---

### 3. Class & Object

```cpp
class Car {
public:
    string brand;
    int speed;
    void drive() {
        cout << brand << " is driving at " << speed;
    }
};

Car c1;
c1.brand = "BMW";
c1.speed = 100;
c1.drive();
```

---

### 4. Encapsulation

```cpp
class Student {
private:
    int age;
public:
    void setAge(int a) { age = a; }
    int getAge() { return age; }
};
```

---

### 5. Abstraction

```cpp
class Shape {
public:
    virtual void draw() = 0; // Pure virtual
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing Circle";
    }
};
```

---

### 6. Inheritance

```cpp
class Animal {
public:
    void eat() { cout << "Eating"; }
};

class Dog : public Animal {
public:
    void bark() { cout << "Barking"; }
};
```

#### Types of Inheritance:

- Single
- Multiple
- Multilevel
- Hierarchical
- Hybrid

---

### 7. Polymorphism

#### Compile-Time:

```cpp
class Print {
public:
    void show(int x) { cout << x; }
    void show(string s) { cout << s; }
};
```

#### Run-Time:

```cpp
class Animal {
public:
    virtual void sound() {
        cout << "Some sound";
    }
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Bark";
    }
};
```

---

### 8. Constructors & Destructors

```cpp
class A {
public:
    A() { cout << "Constructor"; }
    ~A() { cout << "Destructor"; }
};
```

- Default
- Parameterized
- Copy Constructor

---

### 9. Virtual Functions & vTable

```cpp
class Base {
public:
    virtual void show() { cout << "Base"; }
    virtual ~Base() { cout << "Base destructor"; }
};
```

- Enables dynamic dispatch
- Virtual destructor ensures proper cleanup

---

### 10. Friend Function

```cpp
class A {
    int x = 10;
    friend void show(A);
};

void show(A a) {
    cout << a.x;
}
```

---

### 11. Operator Overloading

```cpp
class Complex {
public:
    int r, i;
    Complex operator+(Complex c) {
        Complex temp;
        temp.r = r + c.r;
        temp.i = i + c.i;
        return temp;
    }
};
```

---

### 12. Static Keyword

```cpp
class Test {
public:
    static int count;
    static void show() {
        cout << count;
    }
};
int Test::count = 0;
```

---

### 13. This Pointer

```cpp
class A {
    int x;
public:
    void setX(int x) { this->x = x; }
};
```

---

### 14. Deep vs Shallow Copy

```cpp
class Sample {
    int* data;
public:
    Sample(const Sample& s) {
        data = new int;
        *data = *(s.data); // Deep Copy
    }
};
```

---

### 15. Diamond Problem

```cpp
class A {};
class B : virtual public A {};
class C : virtual public A {};
class D : public B, public C {};
```

- Use virtual inheritance to avoid ambiguity

---

### 16. Common Interview Questions

- Class vs Struct
- Constructor vs Destructor
- Copy Constructor
- Virtual vs Pure Virtual
- Object Slicing
- Why Virtual Destructor?
- Overloading vs Overriding

---

### 17. Cheatsheet

| Concept              | Feature                      |
| -------------------- | ---------------------------- |
| Encapsulation        | Data hiding using class      |
| Abstraction          | Interface/pure virtual funcs |
| Inheritance          | Reusability of code          |
| Polymorphism         | Compile/run time behavior    |
| Static               | Shared across all instances  |
| Friend               | Access to private members    |
| Operator Overloading | Customize operators          |

