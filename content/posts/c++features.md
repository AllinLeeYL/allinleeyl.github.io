---
date: '2025-08-17T10:30:52+08:00'
draft: false
title: 'C++ Features Compilation'
tags: ["programming", "C/C++"]
---

# [Virtual Method](https://www.geeksforgeeks.org/cpp/virtual-function-cpp/)

> A virtual function is a member function that is declared within a base class and is re-defined (overridden) by a derived class. When you refer to a derived class object using a pointer or a reference to the base class, you can call a virtual function for that object and execute the derived class's version of the method.

It is always helpful when you want to execute the right (derived) function when invoking a method overridden by a derived class, using a pointer that you don't know if it's of the base class or the derived class.

* Virtual functions ensure that the correct function is called for an object, regardless of the type of reference (or pointer) used for the function call.
* They are mainly used to achieve Runtime polymorphism.
* Functions are declared with a virtual keyword in a base class.
* The resolving of a function call is done at runtime, using [vTable and vPtr](https://www.geeksforgeeks.org/cpp/vtable-and-vptr-in-cpp/).

Let's just figure it out by reading the example below.

```c++
#include<iostream>
#include<string>

class Entity {
public:
    std::string getName() {return "entity";}
};
class Player : public Entity {
public:
    std::string getName() {return "player";}
};

int main() {
    Entity* e = new Entity();
    std::cout<<e->getName()<<std::endl; // entity
    Entity* player = new Player();
    std::cout<<player->getName()<<std::endl; // entity
}
```

```c++
#include<iostream>
#include<string>

class Entity {
public:
    virtual std::string getName() {return "entity";}
};
class Player : public Entity {
public:
    std::string getName() override {return "player";}
};

int main() {
    Entity* e = new Entity();
    std::cout<<e->getName()<<std::endl; // entity
    Entity* player = new Player(); // A vPtr pointing to the vTable for Player is created along with the this object.
    std::cout<<player->getName()<<std::endl; // player
}
```

Though there is some (negligible) performance penalty due to the vTable and vPtr mechanism, virtual function makes sure that the derived class's method is actually executed.
