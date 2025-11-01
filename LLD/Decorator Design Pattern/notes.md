# Decorator design pattern

The primary goal of the Decorator Pattern is to dynamically extend or change the functionality of an object without altering its code.

* **Core Goal:** To provide **additional responsibilities/functionalities** to an existing object at **run time** .
* This allows a method call (e.g., `doSomething`) to return an enhanced output (e.g., changing from "I did something" to "I did something great") dynamically.
* The pattern offers a way to enhance an object's characteristics dynamically, which is typically done through **inheritance** but without its drawbacks.

***

## 2. The Problem with Inheritance (Class Explosion)

While inheritance can change an object's behavior at runtime through polymorphism (e.g., overriding a method), it creates significant design flaws in the long run.

* **Inheritance is Bad:** Generally, **inheritance is discouraged** in favor of composition because it creates a complex, tight coupling and rigid hierarchy.
* **Class Explosion:** The main problem is the creation of a massive number of subclasses when trying to add multiple, optional features.
    * **Example (Mario Game):** Mario's character can gain power-ups: Height Up, Gun Ability, Star Ability .
    * If inheritance is used, for every combination of power-ups, a new class must be created (e.g., `MarioWithGunAndHeight`, `MarioWithFlyAndGun`, etc.).
    * This rapid proliferation of subclasses for every feature combination is called **Class Explosion**.
* **Design Principle:** The goal is to **"Use Composition Over Inheritance"** to build more flexible and scalable applications.

***

## 3. The Decorator Solution: Wrapping and Delegation

The Decorator Pattern solves the Class Explosion problem by **wrapping** the original object with additional functionality.

* **Wrapping:** The original object (Obj 1) is wrapped by a new object, the **Decorator 1**.
* **Delegation:** When a method is called on the Decorator (e.g., `doSomething`), the Decorator first calls the same method on the **wrapped object** (the original component). This is called **delegation**.
* **Enhancement:** The Decorator takes the result from the wrapped object, **adds its own logic/enhancement** to it, and then returns the final, modified result to the client.
* **Stackability (Recursion):** Decorators can be **stacked indefinitely** (e.g., Decorator 2 wraps Decorator 1, which wraps Obj 1). This creates a recursive chain: the request travels down, and the enhanced response travels up.

***

## 4. The Relationship: Is-A and Has-A

The Decorator Pattern is unique because it combines two fundamental object-oriented relationships: **Inheritance (Is-A)** and **Composition (Has-A)**.


* **Is-A Relationship (Inheritance):**
    * The Decorator **is a** Component (i.e., it inherits from the Component Interface/Abstract Class).
    * **Purpose:** This allows the Decorator to **behave like** the base object. It ensures that any code expecting the base object can accept the Decorator, enabling the wrapping/stacking functionalit.
* **Has-A Relationship (Composition):**
    * The Decorator **has a** reference to the Component (i.e., it holds an instance of the Component Interface/Abstract Class),.
    * **Purpose:** This reference is used to **dynamically extend** the behavior. The Decorator delegates the method call to this internal reference and then modifies the result,.

***

## 5. Standard UML and Official Definition

### Standard UML Diagram Components

1.  **IComponent (Abstract Component):** An interface or abstract class that defines the methods all concrete components and decorators must implement.
2.  **ConcreteComponent:** The original object that provides the base functionality (e.g., `MarioCharacter`).
3.  **Decorator (Abstract Decorator):** An abstract class that implements the `IComponent` (Is-A) and holds a reference to `IComponent` (Has-A).
4.  **Concrete Decorators:** Implement specific enhancements, delegating the call to the wrapped component and adding their own logic (e.g., `HeightUpDecorator`).

### Official Definition

> "Decorator pattern **attaches additional responsibilities** to an object **dynamically**. Decorator provides a **flexible alternative to sub-classing** for **extending functionality**."

The key benefit is providing a flexible alternative to inheritance, which typically causes problems when extending functionality.

***

## 6. Code Walkthrough Summary

The code structure demonstrates the two key relationships and the recursive nature of the pattern:

* **ICharacter (Component Interface):** Defines `getAbilities()`.
* **Mario (Concrete Component):** Implements `getAbilities()` and returns the base string "Mario" ([00:24:36]).
* **CharacterDecorator (Abstract Decorator):** Extends `ICharacter` (Is-A) and has a private `ICharacter character` reference (Has-A) set in the constructor.
* **Concrete Decorators (e.g., `HeightUpDecorator`):**
    1.  Call the wrapped component's method: `this.character.getAbilities()`.
    2.  Append their specific enhancement: `... + " with Height Up"`.
* **Client Implementation:** Objects are nested to create the chain of enhancements at runtime:
    ```
    // The inner-most object is the base component (Mario)
    ICharacter mario = new StarPowerUpDecorator(
        new GunPowerUpDecorator(
            new HeightUpDecorator(
                new Mario() 
            )
        )
    );
    ```

***

## 7. Real-world Use Cases

The Decorator Pattern is ideal for scenarios where features must be added or removed flexibly without using a fixed inheritance structure.

* **Text Editor Functionality:**
    * **Base Component:** A simple `Text` object.
    * **Decorators:** **Bold**, **Italics**, **Underline**, or **Font Change** decorators.
    * The user can combine these features in any order (e.g., Bold and Italic, or just Underline) without needing a pre-defined class for every combination.
* **Web Form/API Validation:**
    * **Base Component:** A basic form/data submission object.
    * **Decorators:** **Email Checker** (validates format), **SQL Injection Checker** (sanitizes input), **CSS Injection Checker**.
    * A system can apply different validation decorators based on the context or data type.