# Delegated Properties

Properties can have some kind of same behavior. Behaviours we would like to reuse, instead of having to declare the same code over and over again.
The properties we would like to delegate is lazy properties, observable properties and storing properties in a map. 

## Lazy properties 

The first time a lazy property is initialized is when it's getValue() is called. This is good practice because it saves memory not to initialize the property before there is a need to.
It is also good practice in case of another part of the program is requiered to be ready before the property can be used.

    companion object {
        val DB_NAME = "VETDB"
        val DB_VERSION = 4
        val instance by lazy { DBController() }
    }
 

Here the DBController gets computed only upon the first time it is being called/accessed

## Observable properties

Listeners get notiﬁed about changes to this property. 

Delegates.observable() takes two arguments: the initial value and a handler for modiﬁcations. The handler gets called every time we assign to the property (after the assignment has been performed). It has three parameters: a property being assigned to, the old value and the new one:

```kotlin

    import kotlin.properties.Delegates

    class User {    
      var initialValue: String by Delegates.observable("<handler for modiﬁcations>"){        
        prop, old, new ->        
        println("$old -> $new")    
        } 
    }
             
    fun main(args: Array<String>) {    
      val user = User()    
      user.initialValue = "first"    
      user.initialValue = "second" 
    }
```

 This example prints 
 `<handler for modiﬁcations> -> first first -> second`


## Storing the values of a properties in a Map

Another way to delegate the values of a property is to get them from a map, using the name of the property as the key of the map.

Another way to say it is that if you want to store the values of a property in a Map, you can use the map instance itself as the delegate for a delegated property.

        class User(val map: Map<String, Any?>) {    
            val name: String by map    
            val age: Int     by map 
        }
        
In this example, the constructor takes a map:
        
        val user = User(mapOf(    
            "name" to "John Doe",    
            "age"  to 25 
        ))

Delegated properties take values from this map (by the string keys –– names of properties):

        println(user.name) // Prints "John Doe" 
        println(user.age)  // Prints 25

This works also for var’s properties if you use a MutableMap instead of read-only Map:

        class MutableUser(val map: MutableMap<String, Any?>) {    
            var name: String by map    
            var age: Int     by map 
        }

## Get and set

All delegated properties has a getValue() and setValue()

The getter will return a value if it’s assigned, otherwise it will throw an exception. 
The setter will assign the value if it is still null, otherwise it will throw an exception.
