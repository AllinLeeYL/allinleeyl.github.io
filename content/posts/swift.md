+++
title = 'Swift Language Notes'
date = 2024-10-27T23:13:18+08:00
draft = false
tags = ["programming language"]
+++

This is a note from the Video [Learn the Essentials of Swift in one hour](https://www.youtube.com/watch?v=n5X_V81OYnQ&t=1239s). It aims to conclude the most frequently used grammar and features and give some examples, rather than cover all the grammar listed by [this site](https://docs.swift.org/swift-book/documentation/the-swift-programming-language). 

# [Functions](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/functions)

Swift function parameters can have two names, one used externally and one used internally.

```Swift
func printTimesTable(for number: Int) {
    for i in 1...12 {
        print("\(i) x \(number) = \(i * number)")
    }
}
printTimesTable(for: 5) // "for" is used externally
```

Swift can also have positional parameters so the parameter name will not show in the function call. To declare a positional parameter, just type "_" before the parameter name.

```Swift
func greet (_ name: String, formal: Bool = false) {
    if formal == true {
        print("Hello, \(name)")
    } else {
        print("Hi! \(name)")
    }
}
greet("Trump") // Hello, Trump
greet("Taylor") // Hi! Taylor
```

The **return** keyword can be removed if a closure or function has only one line of code to make them easier to read.

# [Closures](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/closures)

You can assign functionality directly to a constant or variable. The assigned constant or variable is a closure which can be called like a function. You can also pass parameters to closures by placing them into braces. The **in** keyword marks the segment with parameters and body. Everything after **in** is the main body of closure itself.

```Swift
let sayHello = { (name: String) -> String in
    "Hi, \(name)"
}
sayHello("Taylor") // Hi, Taylor
```

You can also directly use the closure like this:

```Swift
let team = ["Gloria", "Suzanne", "Tiffany", "Tasha"]
let onlyT = team.filter({ (name: String) -> Bool in
    return name.hasPrefix("T")
})
// or you can even write your code in a more easire way
let onlyT = team.filter { name in
    name.hasPrefix("T") // the return keyword can be removed
}
// or go further by writing like this
let onlyT = team.filter { $0.hasPrefix("T") }
print(onlyT) // ["Tiffany", "Tasha"]
```

# [Structs](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/classesandstructures)

Structs let us make our own data types. Structs can have constants, variables, and functions. If you want a struct method to change one of its properties, you must mark it as **mutating**. For example,

```Swift
struct Album {
    let title: String
    let artist: String
    let isReleased: Bool = true
    func printSummary() {
        print("\(title) by \(artist)")
    }
    mutating func removeFromSale() {
        isReleased = false
    }
}
let red = Album(title: "Red", artist: "Taylor Swift")
```

Computed properties are variables computed at each call dynamically. You can also explicitly define the get and set method for the variable. For example: 

```Swift
struct Employee {
    var vacationAllowed = 14
    var vacationTaken = 0
    var vacationRemaining = Int {
        vacationAllowed - vacationTaken
    }
    var vacationRemaining = Int { // another version
        get {
            vacationAllowed - vacationTaken
        }
        set {
            vacationAllowed = vacationAllowed + newValue
        }
    }
}
```

In the set method, the **newValue** provided by Swift stores whatever value the user was trying to assign to this property. 

Property observers are pieces of code that run when a property changes. **didSet** is called after the change has taken place. **willSet** is called before the change has taken place. For example,

```Swift
struct Game {
    var score = 0 {
        didSet {
            print("Score is now \(score)")
        }
    }
}
```

Initializers

```Swift
struct Player {
    let name: String
    let number: Int
    init(name: String) {
        self.name = name
        self.number = Int.random(in: 1...99)
    }
}
```

# [Classes](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/classesandstructures/)

> Swift doesn’t require you to create separate interface and implementation files for custom structures and classes. In Swift, you define a structure or class in a single file, and the external interface to that class or structure is automatically made available for other code to use.

Classes let you create customized data types like structs, but are different from structs primarily in 6 key ways:

* Classes can be built upon or inherit characteristics from another class.
* Type casting enables you to check and interpret the type of a class instance at runtime.
* All structures have an automatically generated memberwise initializer, but classes do not. Unlike structures, class instances don’t receive a default memberwise initializer.
* All copies of a class share one particular set of data. That is, the copies are references to a single one rather than new instances.
* Classes can have a de-initializer using the keword **deinit** if needed.
* Classes let you change variable properties even in the class instance itself is constant (defined by the keyword **let**).

If a child class wants to change the method it inherit from the parent class, the keyword **override** is used. 

# [Access Control](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/accesscontrol/)

There are 4 most common access control options:

* **private**: Let nothing outside this struct read or write it.
* **private(set)**: Something outside can read it, but only something inside can write it.
* **fileprivate**: Anything inside the file can read and write it.
* **public**: Anything can read and write it.

# [Protocols](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/protocols)

Protocols define functionality that we expect other types to support. I think it is similar to *interface* in Java and *virtual function* in C++, even though there are differences. For example: 

```Swift
protocol Vehicle {
    var name: String { get }
    var currentPassengers: Int { get set }
    func travel(distance: Int)
}

struct Car: Vehicle {
    let name = "Car"
    var currentPassengers = 2
    func travel(distance: Int) {
        print("The car drives \(distance)km.")
    }
}
```

# [Extensions](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/extensions)

Extensions let you add new functionality to any type. For example:

```Swift
extension String {
    func trimmed() -> String {
        self.trimmingCharacters(in: .whitespacesAndNewlines)
    }
    mutating func trimmed() {
        self = self.trimmed()
    }
    var lines: [String] {
        self.components(seperatedBy: .newlines)
    }
}
let lyrics = """
I keep cruising
Can't stop, won't stop moving
"""
print(lyrics.lines.count) // 2
```

Extension protocols add new functionality to a whole protocol. So any type conforming to the protocol gets access to them. For example, arraies, dictionaries, and sets all conform to a protocol called *Collection*.

```Swift
extension Collection {
    var isNotEmpty: Bool {
        isEmpty == false
    }
}
let guests = ["Mario", "Luigi"]
print(guests.isNotEmpty) // true
```

# Optional

Swift introduces optional types, which handle the absence of a value. Optionals say either “there is a value, and it equals x” or “there isn’t a value at all”.

To write an optional type, you write a question mark (?) after the name of the type that the optional contains — for example, the type of an optional Int is Int?

```Swift
// serverResponseCode contains an actual Int value of 404
var serverResponseCode: Int? = 404
// serverResponseCode now contains no value
serverResponseCode = nil
```

Optional chaining is a process for querying and calling properties, methods, and subscripts on an optional that might currently be nil. If the optional contains a value, the property, method, or subscript call succeeds; if the optional is nil, the property, method, or subscript call returns nil. Multiple queries can be chained together, and the entire chain fails gracefully if any link in the chain is nil.

If you create a new Person instance, its residence property is default initialized to nil, by virtue of being optional. In the code below, john has a residence property value of nil:

```Swift
class Person {
    var residence: Residence?
}
class Residence {
    var numberOfRooms = 1
}
let john = Person()
```

If you try to access the numberOfRooms property of this person’s residence, by placing an exclamation point after residence to force the unwrapping of its value, you trigger a runtime error, because there’s no residence value to unwrap:

```Swift
// this triggers a runtime error
let roomCount = john.residence!.numberOfRooms 
```

Optional chaining provides an alternative way to access the value of numberOfRooms. To use optional chaining, use a question mark. This tells Swift to “chain” on the optional residence property and to retrieve the value of numberOfRooms if residence exists.

```Swift
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
} // Prints "Unable to retrieve the number of rooms."
```

You can also use *guard let*. For example:

```Swift
func printSquire(of number: Int?) {
    guard let number = number else {
        print("number is nil")
        return
    }
    print("\(number) x \(number) is \(number * number)")
}
```

You can use the *nil coalescing* operator (??) to unwrap the optional. If the optional is empty, *nil coalescing* lets you to provide a default value. For example

```Swift
let tvShows = []
let curShow = tvShows.randomElement() ?? "None" // None
let number = Int("") ?? 0 // 0
```

You can use *optional try* by adding (?) after the keyword **try**. For example: 

```Swift
enum UserError: Error {
    case badID, networkFailed
}
func getUser(id: Int) throws -> String {
    throw UserError.networkFailed
}
if let user = try? getUser(id: 23) {
    print(user)
} else {
    print("Wrong")
}
```