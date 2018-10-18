# Init to Win It

## Class Initialization in Kotlin

---

# David Greenhalgh

## @drgreenhalgh

## Big Nerd Ranch

---

# Init to Win It

## Class Initialization in Kotlin

---

^ When we were writing the Kotlin guide, we had a chapter called _Classes_ that was 30 pages and growing. 
(That content ended up being 64 pages.)
We knew that Kotlin had a lot of nuance in how you declare classes.

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

^ Explain difference between instance and type

---

# Holding a Reference to a Class Instance

```kotlin
val player = Player()
```

^ Explain usefulness of passing references around

---

# Class Properties

```kotlin
class Player {
    val name = "David"
}
```

^ Classes hold state in properties

---
[.code-highlight: all]
[.code-highlight: 3]

# Class Properties

```kotlin
Player().name

>>> "David"
```

^ That state can later be referenced at runtime

---

# Property Initialization

```kotlin
class Player {
    val name: String
}
```

^ Properties must be initialized

---

# Property Initialization

```kotlin
Player().name
```

^ What would happen? Properties must be initialized.

---

---

# Primary Constructor

```kotlin
Player()
```

^ Here, a primary constructor is invoked.

---

# Primary Constructor

```kotlin
class Player {
    val name = "David"
}
```

^ The primary constructor on `Player` here has no parameters.

---
[.code-highlight: all]
[.code-highlight: 1]

# Primary Constructor

```kotlin
class Player() {
    val name = "David"
}
```

^ This slide is effectively the same as the last.

---

# Primary Constructor

```kotlin
class Player(_name: String) {
    val name = _name
}
```

^ Arguments may be passed to the constructor to customize how a class is initialized.
Setting property values, for example.

---

# Passing an Argument to a Primary Constructor

```kotlin
Player("Eric")
```
^ Here, we initialize `Player` with the `name` "Eric"

---

# Passing an Argument to a Primary Constructor

```kotlin
Player("Eric").name

>>> "Eric"
```

^ We can later reference the `name` property to see that the state is held onto.

---

# Property Definition Shorthand

```kotlin
class Player(val name: String)
```

^ You can take a shortcut and define properties in the primary constructor rather than creating a temp variable.

---

# Passing an Argument to a Primary Constructor

```kotlin
Player("Eric").name

>>> "Eric"
```

^ This works the same way as before

---

# Default Arguments

```kotlin
class Player(val name: String = "David")
```

^ Default arguments give you the ability to define default values if a value isn't passed to your constructor.

---

# Default Arguments

```kotlin
Player("Eric").name

>>> "Eric"
```

^ If you pass "Eric" to `name`, then "Eric" is the `name` value.

---

# Default Arguments

```kotlin
Player().name

>>> "David"
```

^ If you pass nothing to `name`, then "David" is the `name` value.

---

# Secondary Constructors

```kotlin
class Player(val name: String = "David")
```

^ This, again, is our primary constructor.

---

# Secondary Constructors

```kotlin
class Player(
        val name: String = "David",
        val healthPoints: Int
)
```

^ Let's add another property to `Player`.

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

^ Another way to give the user the ability to instantiate our class in a different way is to define a different constructor.
You can have multiple secondary constructors. They must all call back to the primary constructor somehow.
`this` is referring to the class scope.

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

^ Secondary constructors can hold logic

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

^ Secondary constructors can reference properties. 
This implies that properties are initialized before the secondary constructor body is executed.

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

^ Declaration and assignment may be separated if the property is assigned in the initializer block.

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

^ Which is printed first?

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

---

# Late Initialization

```kotlin
class Player {
    lateinit var name: String
}
```

^ Sometimes, you don't have access to a constructor or otherwise can't assign a property on class initialization. 
What can you do?

---

# Late Initialization

```kotlin
class PlayerActivity : AppCompatActivity {
    
    lateinit var nameTextView: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_start)

        nameTextView = findViewById(R.id.name_text_view)
    }
}
```

^ lateinit is essential on Android.

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

^ Objects can have properties, functions, and their own state.

---

# Object Initialization

```kotlin
Game.play()
```

^ They are initialized when first referenced.

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
    override fun onClick(v: View?) {
        ...
    }
})
```

^ Initialized when its enclosing class is initialized - not when the view is clicked


---

# Object Expressions

```kotlin
val clickListener = object : View.OnClickListener {
    override fun onClick(v: View?) {
        ...
    }
}
```

^ Initialized immediately when defined at file-level

---

# Companion Objects

```kotlin
class PlayerActivity : AppCompatActivity {
    
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