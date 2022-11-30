# Dependency Injection

**Dependency** When something depends on something else 

**What is DI?**

Classes sometimes need references to other classes. for example,
The car class might need a reference to an Engine class.
That means the Car is dependent on Engine class, without an engine
a Car is nothing [real-world scenario]

Three ways a class can get an object **if needed**

1. Class constructs the dependency(Engine) it needs.

```kotlin
fun main(args: Array) {
  val car = Car()
  car.start()
}

class Car {
  private val engine = Engine()
  
  fun start() {
    engine.start()
  }
}

class Engine {
  fun start() {
    print("engine is started ...")
  }
} 
```

2. Grab it from else like some APIs Context, getSystemService()

3. Supplied as a parameter, when a class is inited, the constructor would receive it as a parameter, 
which is also called **dependency injection** 


## issue

* The Car and Engine are tightly coupled - Car uses one type of Engine. Have to create two types of Cars for engines type Gas and Electric.
* Engine makes testing more difficult, Car uses a real instance of Engine.

```kotlin

fun main() {
  val car1 = CarGass()
  val car2 = CarElectric()
  car1.start()
  car2.start()
}

class CarGass {
  val engine = Engine("Gas")
  
  fun start() {
    println(engine.start())
  }
}

class CarElectric {
  val engine = Engine("Electric")
  
  fun start() {
    println(engine.start())
  }
}

class Engine(val name: String) {

  fun start() {
    println("$name engine is starting ...")
  }
}
```

## DI implementation

* Instead of a Car constructing its own Engine, it receives an Engine object as a parameter in its constructor

```kotlin
fun main() {
  val engine1 = Engine("Gas")
  val engine2 = Engine("Electric")
    
  val car1 = Car(engine1)
  val car2 = Car(engine2)
   
  car1.start()
  car2.start()
}

class Car(val engine: Engine) {
  fun start() {
    println(engine.start())
  }
}

class Engine(val name: String) {
  fun start() {
    println("$name engine is starting ...")
  }
}
```

## Benefits 

* Reusability: can pass in different implementations of Engine to Car
* Easy testing: can pass in test doubles to test different scenarios. For example, create a test double of 
Engine called FakeEngine and configure it for different tests.

Two major ways to do dependency injection in Android
----------------------------------------------------

**1. Constructor Injection** Described above <br />

**2. Field Injection (or Setter Injection)** Dependencies are instantiated after the class is created.
The code would look like this - 

```kotlin
class Car {
  lateinit var engine: Engine

  fun start() {
    engine.start()
  }
}

fun main(args: Array) {
  val car = Car()
  car.engine = Engine()
  car.start()
}
```
