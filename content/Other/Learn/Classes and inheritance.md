> [!info] Heads up
> This page talks about classes in a general way, but uses Unreal Engine classes in examples.

# Classes
What is a class?
Like in other programming languages, a `Class` is an *object* to which you can assign values and methods.

For example, if we want to create a player class, we will create a new class called `Player` derived from the `Character` class, then declare variables (Health, Stamina, etc.), functions (Die, AddHealth, etc.), events, and more inside it.
Depending on how everything is configured, we can control these variables, functions, etc., both inside and/or outside the `Player` class.

Once a class is created, we can create an infinite number of **instances**. An instance is essentially a *live version* of your class.

# Inheritance
An important concept of classes is *inheritance*.

A `child` is essentially a `subclass` of another class (called the `parent`).
This `child` class will therefore `inherit` all the declarations contained in the `parent` class (variables, functions, etc.). But it doesn't necessarily have access to everything (variables or functions defined as private in parent won't be accessible in childs).

For example, we can say that the `Animal` class has the variable `Name`.
Now, by creating a `child` class of `Animal` called `Cat`, we can access this variable.

![[Pasted image 20241109195221.png|400]]

Like said before, when we want to create a player, we will create a new class `Player` derived from the `Character` class.
This means the `Player` class is a `child` of the `Character` class, and thus it will inherit its content.

Additionally, the `Character` class is a child of the `Pawn` class, which is itself a child of the `Actor` class.
![[Pasted image 20241109205459.png]]

# Casting
By default, if you use/retrieve a variable, function, etc., from a class within itself, you don't need to specify an instance of the class because it defaults to `self` (known as `this`).

However, to use/retrieve variables, functions, etc., from a class externally *(for example, if we want a trap to reduce the player's health (from the `Player` class) by 1 when the player enters the trapped zone)*, we need a **reference** to an instance of the class we want to access (In our example, the `Trap`class wants access to an instance of the `Player` class).

> [!error]- UE Blueprint Casting warning
> When you cast to a Blueprint class in UE this will **load** the blueprint class and **all its dependencies**
> This is why important blueprints such as the player one can load a big part of your blueprints in your project (because of all the dependencies).
> This is why big assets should be referenced as a soft reference.

**Typical Scenario:**
Let's imagine that the trap has a reference of type `Actor` that contains an instance of the `Player` class (using `On Component Begin Overlap`).

![[Pasted image 20241109201651.png]]
 
We cannot access the player's `Health` variable because the code *doesn't know* that this reference is of the type `Player`.

**What can we do?**
A simple way to solve this problem is to use the `Cast To <...>` node, it requires a `target` of type `Object`.

![[Pasted image 20241109201854.png|750]]
- If the reference is indeed from the `Player` class, the top Exec Pin will execute, and we will obtain a new reference, directly from the `Player` class (`As Player`).
- If the reference is incorrect, the `Cast Failed` Exec Pin will execute.

We can now retrieve the health variable and manipulate it as we wish.

# Interfaces
==TODO==

