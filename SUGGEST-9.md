One of the uses for package private is to make sub-classing outside the package impossible (package private is not handled correctly right now, but the fix seems to be on the way, see Ticket #2568).

Example of using package private to restrict sub-classing:
```scala
package pkg1 {
    abstract class A {
        def bar: Int
        private[pkg1] def foo: Int
    }
}
package pkg2 {
    import pkg1.A
    class B extends A {
        val bar = 10
        // cannot define foo, so cannot subclass outside of pkg1
    }
}
```
There should be a cleaner way to restrict sub-classing to the members of a package. The sealed[Package] modifier on the class would make more sense:
```scala
package pkg1 {
    sealed[pkg1] abstract class A {
        def bar: Int
        def foo: Int
    }
}
package pkg2 {
    import pkg1.A
    abstract class B extends A { //not possible to subclass
        
    }
}
```

Proposed specification:
Qualified sealed locks sub-classing to the specified package (or outer class). However it does not guarantee match-case safety like the unqualified sealed modifier does.
Your input is appreciated, but I'm afraid we are no longer accepting "Suggestion" tickets on JIRA.

Available mechanisms for making suggestions include:
- the Scala mailing lists
- SIP (Scala Improvement Process) and SLIP (Scala Library Improvement Process)
- a pull request on GitHub implementing the suggestion
