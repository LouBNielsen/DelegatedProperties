# Delegated Properties

Properties can have some kind of same behavior. Behaviours we would like to reuse, instead of having to declare the same code over and over again.
The properties we would like to delegate is lazy properties, observable properties and storing properties in a map. 

** Lazy properties 

The first time a lazy property is initialized is when it's getValue() is called. This is good practice because it saves memory not to initialize the property before there is a need to.
It is also good practice in case of another part of the program is requiered to be ready before the property can be used.

    companion object {
        val DB_NAME = "VETDB"
        val DB_VERSION = 4
        val instance by lazy { DBController() }
    }
 

Here the DBController gets computed only upon the first time it is being called/accessed

** Observable properties

Listeners get notiﬁed about changes to this property. 

Delegates.observable() takes two arguments: the initial value and a handler for modiﬁcations. The handler gets called every time we assign to the property (after the assignment has been performed). It has three parameters: a property being assigned to, the old value and the new one:

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
    
 This example prints 
 <handler for modiﬁcations> -> first first -> second


** Storing properties in a map
