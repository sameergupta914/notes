# Observer Design Pattern

## 1. Core Problem and Analogy

The Observer Pattern solves a specific problem: how to notify multiple objects about a state change in another object.

* **Real-world Analogy (YouTube):**
    * You **subscribe** to a YouTube channel.
    * When the channel (the **Subject**) uploads a new video (a **state change**), you (the **Observer**) get a notification.
    * The channel doesn't know *who* you are specifically, it just knows it has a list of subscribers to notify.
* **The Goal:** To define a **one-to-many relationship** between objects. When one object (the Subject) changes, all its dependents (Observers) are **automatically notified** and updated.

---

## 2. Polling vs. Pushing (The Solution)

There are two main ways to get updates:

### A. Polling (The Inefficient Way)

* This is the technique used *without* the Observer pattern.
* The **Observer** is responsible for checking the state.
* The Observer must repeatedly ask the Subject: "Has your value changed?", "Has your value changed?", "Has your value changed?".
* **Problems with Polling:**
    * **Inefficient:** Most requests will return "No," wasting resources.
    * **Frequency:** It's hard to know how often to poll (every millisecond? every 10 seconds?). Polling too fast wastes resources; polling too slow means you get stale data.

### B. Pushing (The Observer Pattern)

* This is the solution provided by the pattern.
* The **Subject (Observable)** is responsible for notifying the Observers.
* When the Subject's state changes, it **pushes** a notification to all registered Observers.
* This is far more efficient as notifications are sent *only* when a change actually happens.

---

## 3. Terminology and Relationships

* **Observable (or Subject):** The object that is being watched. It maintains a list of its dependents. (e.g., The YouTube Channel)
* **Observer:** The object that is watching. It registers with the Subject to receive updates. (e.g., The Subscriber)
* **Relationship:** One-to-Many (One Observable can have many Observers).

---

## 4. How It Works (The Mechanics)

1.  **Subscription:** The Observable (Subject) must be aware of its Observers. It maintains an internal **list** (or map, set, etc.) of all Observers that have subscribed to it.
2.  **State Change:** When the Observable's internal state changes (e.g., a new video is uploaded), it triggers its notification logic.
3.  **Notification:** The Observable iterates through its list of Observers and calls a specific method (e.g., `update()`) on each one.

---

## 5. UML Diagram and Components

[Image of Observer Design Pattern UML Diagram]

### A. `IObservable` (The Subject Interface)

This interface defines the contract for the object *being watched*.

* `add(IObserver)`: Allows an Observer to subscribe. (In the video, this is `subscribe()`).
* `remove(IObserver)`: Allows an Observer to unsubscribe. (In the video, this is `unsubscribe()`).
* `notify()`: Notifies all Observers in the list.

### B. `IObserver` (The Observer Interface)

This interface defines the contract for the object *doing the watching*.

* `update()`: This is the method that the `IObservable` will call when `notify()` is triggered.

### C. Concrete Classes

* **`ConcreteObservable` (e.g., `YouTubeChannel`)**
    * Implements `IObservable`.
    * Maintains the `List<IObserver>`.
    * Implements the `add`, `remove`, and `notify` methods.
    * Contains the core business logic (e.g., `uploadVideo()`) that, when executed, will change the state and call `notify()`.
    * Contains getter methods (e.g., `getVideo()`) to allow Observers to "pull" the new data.
* **`ConcreteObserver` (e.g., `Subscriber`)**
    * Implements `IObserver`.
    * Maintains a **Has-A** relationship (a reference) to the `ConcreteObservable`.
    * **Why?** So that when its `update()` method is called, it knows *which* Observable to ask for the new data.
    * The `update()` method's logic is typically: `value = observable.getValue()`.

---

## 6. Code Example (YouTube Analogy)

### `IChannel.java` (Interface for Observable)
```java
public interface IChannel {
    void subscribe(ISubscriber subscriber);
    void unsubscribe(ISubscriber subscriber);
    void notifySubscribers();
}