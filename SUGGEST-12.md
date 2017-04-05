In a future release of Scala, I'd like to be able have anonymous functions use traits without having to implement the apply method. 

Example:
Given trait:
```scala
trait DoesFoo {
  def foo() = 42
}
```
Currently one can do:
```scala
val func = new (Int => Int) with DoesFoo {
     def apply(x:Int) = foo() * x
    }
```
The enhancment would support syntax:
```scala
val func = { x:Int => foo() * x } with DoesFoo
```
Duplicate of #2448
I reopened this because IMHO this change is a lot simpler that what is being presented in #2448. This is just syntactic sugar to avoid having to implement the apply method and doesn't involve coercing function objects to satisfy the interfaces for a single method abstractions. 
Your input is appreciated, but I'm afraid we are no longer accepting "Suggestion" tickets on JIRA.

Available mechanisms for making suggestions include:
- the Scala mailing lists
- SIP (Scala Improvement Process) and SLIP (Scala Library Improvement Process)
- a pull request on GitHub implementing the suggestion
