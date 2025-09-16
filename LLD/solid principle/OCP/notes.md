# Open/Closed Principle

- The Open/Closed Principle states that a class should be open for extension but closed for modification. This means that you should be able to add new functionality to a class without altering its existing code. The goal is to prevent changes to existing code that could introduce bugs or affect the functionality of the system.

- To implement this principle, you can use techniques such as inheritance and interfaces. By creating abstract classes or interfaces, you can define a contract that new classes can implement or extend. This allows you to introduce new features or behaviors without modifying the existing classes.

- For example, if you have a class that handles printing invoices, instead of changing the existing class to add new printing features, you can create a new class that extends the original class or implements an interface. This way, the original class remains unchanged, adhering to the Open/Closed Principle, while still allowing for new functionality to be added