# Init to Win It

## Class Initialization in Kotlin

---

# David Greenhalgh

## @drgreenhalgh

## Big Nerd Ranch

---

# Defining a Class

```kotlin
class Player
```

^ This is a type declaration

---

# Constructing a Class Instance

```kotlin
Player()
```

---

# Class Properties

```kotlin
class Player {
    val name = "David"
}
```

---
[.code-highlight: all]
[.code-highlight: 3]

# Class Properties

```kotlin
Player().name

>>> "David"
```

---

# Property Initialization

```kotlin
class Player {
    val name: String
}
```

---

# Property Initialization

```kotlin
Player().name
```

^ What would happen? Properties must be initialized.

---

# Primary Constructor

```kotlin
Player()
```

---

# Primary Constructor

```kotlin
class Player {
    val name = "David"
}
```

---
[.code-highlight: all]
[.code-highlight: 1]

# Primary Constructor

```kotlin
class Player() {
    val name = "David"
}
```

---

# Primary Constructor

```kotlin
class Player(_name: String) {
    val name = _name
}
```

---

# Passing an Argument to a Primary Constructor

```kotlin
Player("Eric")
```

---

# Passing an Argument to a Primary Constructor

```kotlin
Player("Eric").name

>>> "Eric"
```

---

# Property Definition Shorthand

```kotlin
class Player(val name: String)
```

---

# Passing an Argument to a Primary Constructor

```kotlin
Player("Eric").name

>>> "Eric"
```

---

# Default Arguments

```kotlin
class Player(val name: String = "David")
```

---

# Default Arguments

```kotlin
Player("Eric").name

>>> "Eric"
```

---

# Default Arguments

```kotlin
Player().name

>>> "David"
```

---

# Secondary Constructors

```kotlin
class Player(val name: String = "David") {

}
```

---

# Secondary Constructors

```kotlin
class Player(
        val name: String = "David",
        val healthPoints: Int
) {

}
```

---

# Secondary Constructors

```kotlin
class Player(
        val name: String = "David",
        val healthPoints: Int
) {

    constructor(name: String) : this(name, 42)
}
```

---
[.code-highlight: 6-10]

# Secondary Constructors

```kotlin
class Player(
        val name: String = "David",
        val healthPoints: Int
) {

    constructor(name: String) : this(name, 42) {
        if (name == "Eric") {
            println("Eric alert")
        }
    }
}
```

---
[.code-highlight: 6-10]

# Secondary Constructors

```kotlin
class Player(
        val name: String = "David",
        val healthPoints: Int
) {

    constructor(name: String) : this(name, 42) {
        if (name == "Eric") {
            println(healthPoints)
        }
    }
}
```

---

# Secondary Constructors

```kotlin
class Player(
        val name: String = "David",
        val healthPoints: Int
) {

    constructor(name: String) : this(name, 42) {
        if (name == "Eric") {
            println(healthPoints)
        }
    }
}

>>> 42
```

^ `healthPoints` is accessible and initialized within the scope of the secondary constructor

---

# The Initializer Block

```kotlin
class Player(
        val name: String = "David",
        val healthPoints: Int
) {

    init {
        require(healthPoints > 0)
    }
}
```

^ `healthPoints` is accessible and initialized within the scope of the secondary constructor

---

# Property Initialization

```kotlin
class Player {
    val name: String

    init {
        name = "David"
    }
}
```

---

# Property Initialization

```kotlin
class Player {
    init {
        name = "David"
    }

    val name: String
}
```

^ Doesn't compile

---

# Property Initialization

```kotlin
class Player {
    val name: String
    
    init {
        println(getNameFirstCharacter())
        name = "David"
    }

    fun getNameFirstCharacter() = name[0]
}
```

^ Runtime exception

---

# Property Initialization

```kotlin
class Player(
        val name: String = "David",
        val healthPoints: Int
) {
    init {
        println("Init")
    }

    constructor(name: String) : this(name, 42) {
        println("Secondary")
    }
}
```

---

# Property Initialization

```kotlin
class Player(
        val name: String = "David",
        val healthPoints: Int
) {
    init {
        println("Init")
    }

    constructor(name: String) : this(name, 42) {
        println("Secondary")
    }
}

>>> Init
>>> Secondary
```

^ Timing of when properties are initialized is an important theme

---

# Late Initialization

```kotlin
class Player {
    lateinit var name: String
}
```

---

# Late Initialization

```kotlin
class PlayerActivity {
    
    lateinit var nameTextView: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_start)

        nameTextView = findViewById(R.id.name_text_view)
    }
}
```

---

# Lazy Initialization

```kotlin
class Player {
    val name by lazy { generateRandomName() }

    fun generateRandomName() = ...
}
```

^ Delay computationally expensive tasks until necessary

---

# Object Declarations

```kotlin
object Game
```

^ Singletons are initialized when accessed and live as long as your application lives

---

# Object Initialization

```kotlin
object Game {
    fun play() = ...
}
```

---

# Object Initialization

```kotlin
Game.play()
```

---

# Object Initialization

```kotlin
object Game {
    init {
        ...
    }
}
```

^ Object declarations don't have constructors because you don't call the constructor directly

---

# Object Expressions

```kotlin
nameTextView.setOnClickListener(object : View.OnClickListener {
    override fun onClick(p0: View?) {
        ...
    }
})
```

^ Initialized when its enclosing class is initialized - not when the view is clicked


---

# Object Expressions

```kotlin
val clickListener = object : View.OnClickListener {
    override fun onClick(p0: View?) {
        ...
    }
}
```

^ Initialized immediately when defined at file-level

---

# Companion Objects

```kotlin
class PlayerActivity {
    
    companion object {
        ...
    }
}
```

^ For conceptually associating statics with a type
^ Initialized before class

---

# The `object` Keyword

* Object declarations
* Object expressions
* Companion objects

---

# The `class` keyword

* Normal classes
* Nested classes
* Inner classes
* Enum classes

---

# Class Initialization

* Primary constructor
* Secondary constructor
* Initializer block

---

# Property Initialization

* On property declaration
* In secondary constructor
* In initializer block
* `lateinit`
* Property delegation

---

# More Kotlin

* Big Nerd Ranch
* Atlanta Android Club
* Let's talk

---