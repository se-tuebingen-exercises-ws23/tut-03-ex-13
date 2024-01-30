# Exercise #13 - SOLID, Software Design

## 0. Organization

1. Is there anything your tutor wants to tell you about previous homework?
2. Do you have any homework-related questions?
3. Have you looked at the peer review of Homework #5? If not, take a look into your `hw5-tut...` repository! `:)`
4. Have you accepted Homework #11 already? Do you know that it's the last homework and that the deadline is this Sunday?
5. What do you know about the Teamprojekt? What knowledge do you think you'll need from other courses?
6. Do you know that the next tutorial will include exercising project management with LEGO bricks? `:)`

## 1. Requirements Analysis and User Stories

Your Teamprojekt is to implement a new forum-like system for Software Engineering.
One of your team members defines the following User Story:

> 1. **As a** professor,
> 2. **I want** to post announcements
> 3. **so that** they appear on my students' feed where they can read them.

### Part 1

You are implementing this User Story.
What classes will be in your "model" (for example a UML class diagram?) 
_Only name the classes._

<details>
Professor, Announcement, and Student, Feed can be possible model classes.
</details>

### Part 2

Come up with two more requirements in the form of a User Story:

> 1. **As a** `user role concerned by the story`,
> 2. **I want** `goal of the story`,
> 3. **so that** `reason for the story`.

## 2. Revise SOLID

**SOLID**
- **S**ingle Responsibility Principle: _A component (class, function, module, ...) should only have one reason to change._
- **O**pen–Closed Principle: _A component should be open for extension, but closed for modifications._
- **L**iskov Substitution Principle: _Subtypes must be behaviorally substitutable for their base types._
- **I**nterface Segregation Principle: _Clients should not be forced to depend upon interfaces that they do not use._
- **D**ependency Inversion Principle: _High-level modules should not depend on low-level modules. Both should depend upon abstractions [not concretions]._

## 3. SOLID

### Part 1: Bookstore Management System

A student is building a management software for a bookstore.
Their first UML design includes the following:

```mermaid
classDiagram

class Shelf
class Book
class DVD

Book <-- Shelf
DVD <-- Shelf
```

The tutor suggested this alternative design:

```mermaid
classDiagram

class Shelf
class Item {
  <<interface>>
}
class Book
class DVD

Item <-- Shelf
DVD ..|> Item
Book ..|> Item
```

**Question**: Explain what SOLID design principle(s) the tutor's revised design adheres to.

<details>

**Dependency Inversion Principle**: An abstraction (Item interface) is introduced between the high-level classes (Shelf) and low-level classes (Book & DVD) that changes the direction of the dependency and splits the dependency between the high-level and low-level modules.

**Open/Closed Principle**: To extend the application, e.g. to use other "items", all that needs to be done is to add another concrete implementation of the Item interface. The extention (adding a new Item) will not require any changes to the classes already exsiting in the model.

</details>

### Part 2: Board games made 3D

Assume that you're implementing a board game (for example chess) which uses a 2D square-shaped board. You defined a class `Board` which stores all the tiles in it in a map.

You also implemented many methods (including getters and setters), among which there is `getTile(x, y)` returning a tile in the `(x, y)` location.
Later the requirements change and you need to support 3D boards as well.
A 3D board _is_ a board, so you decide to extend the existing `Board` class and come up with the following design:

```mermaid
classDiagram

class Tile {
  <<enum>>
  ...
}

class Board {
  tiles: Map[Int, Int][Tile]
  
  getTile(x: Int, y: Int): Option[Tile]
}

Board <|-- 3DBoard

class 3DBoard {
  tiles: Map[Int, Int, Int][Tile]
  
  getTile(x: Int, y: Int, z: Int): Option[Tile]
}
```

**Questions**: Is this a reasonable design? Take the SOLID principles into account!
How would you improve the design?

<details>
This design goes against **Liskov Substitution Principle (LSP)**.

At first glance, it may seem a good decision to make 3DBoard a subclass of Board class. Board provides a 2D board and functionalities over two dimensions, and 3DBoard can inherit all that and add a new dimension. The problem though is that many of Board's methods, such as the given getTile(), are designed to work with two dimension units, not three. So, they lose their context/meaning when it comes to three dimensions. For instance, getTile(int, int) loses its meaning in 3DBoard as getTile(int, int, int) makes more sense.

Thus, a client code attempting to use the 3DBoard class as its base class Board would be very out of luck, which is what LSP is about i.e. being able to substitute base class objects with subclass objects.
</details>

## 4. Software Design: Markdown

_This task has three parts, part `N` is in `./MarkdownN`._

To start, see `./Markdown1/README.md`. Then continue to part 2 and part 3.
