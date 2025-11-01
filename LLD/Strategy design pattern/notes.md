# Strategy Design Pattern

- Defines a family of Algorithm, put them into separate classes so that they can be changed at run time.
- Put algorithms into separate classes. The client can choose or switch algorithms at runtime.

    - Example (Real-world)
    Payment Systems:
    Algorithms = CreditCardPayment, UPIPayment, CryptoPayment.
    A PaymentContext class delegates payment to the chosen strategy.
    Adding new payment = create new strategy class (no changes in existing ones → OCP preserved).

# Problem with Inheritance

- Code Re-use: Inheritance reuses code, but forces tight coupling. If a parent class changes, all child classes are affected → can cause regressions.

    - Example: If Vehicle class has startEngine() and you create Car and Bike as children, adding a wingspan attribute for Plane in Vehicle would unnecessarily affect all subclasses.

- To add new feature a lot of changes were required: Inheritance creates rigid hierarchies → hard to extend.

    - Example: If you add a new payment type in an e-commerce system (CryptoPayment), modifying a parent Payment class may force changes across all child classes.

- Breaking OCP: OCP says: “Classes should be open for extension but closed for modification.” Inheritance often requires modifying existing classes to support new behavior.

    - Example: In a Shape hierarchy (Circle, Square), if you add Triangle, you may need to modify draw() logic in base class.

# Conclusion

- Encapsulate what varies, Put changing logic into its own class or module.

    - Example: Sorting algorithms (QuickSort, MergeSort) vary → encapsulate them separately from the DataSet class.

- Solution to inheritance is not more inheritance, Adding more subclasses increases rigidity.

    - Example: If you need FlyingCar, don’t keep extending Car → FlyingCar → LuxuryFlyingCar. Instead, use composition.

- Favor Composition over Inheritance, Composition = “has-a” relationship, not “is-a”.

    - Example: Instead of FlyingCar inheriting from both Car and Airplane, create a Car class that has a FlightMode strategy.

- Code to Interface, not Implementation. Depend on abstractions, not concrete classes.

    - Example: Write code that uses PaymentStrategy, not CreditCardPayment directly.

- DRY (Don’t Repeat Yourself)
Reuse behavior through composition and delegation.
    - Example: Logging/validation logic should be in reusable utility/service classes, not duplicated across all business classes.