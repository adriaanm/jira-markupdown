Aladdin: **[http://scala-webapps.epfl.ch/bugtracking/bugs/displayItem.do?id=1293 bug 1293], [http://scala-webapps.epfl.ch/bugtracking/contribs/display.do?id=775 contrib 775]**

== Code ==

The language specification has
"Method `hashCode: ()int` computes a hash-code depending on the data
structure in a way which maps equal (with respect to equals) values to equal
hash-codes."

This is only true if the hashCodes of constructor arguments have this property (see example below).


Some possible alternative wordings:-

"Method hashCode: ()int computes a hash-code depending on the data
structure in a way which maps equal (with respect to equals) values to equal
hash-codes, provided the hashCode methods of the data structure members each map equal 
(with respect to equals) values to equal hash-codes."

"Method hashCode: ()int computes a hash-code. If the hashCode methods of the data structure members map
equal (with respect to equals) values to equal hash-codes, then the case class hashCode method does too."

```scala
object BadlyBehaved
{
    var counter = 0
}

class BadlyBehaved
{
    BadlyBehaved.counter = BadlyBehaved.counter + 1

    override val hashCode = BadlyBehaved.counter

    override def equals(rhs: Any) = rhs.isInstanceOf[BadlyBehaved]
}

case class CounterExample(b: BadlyBehaved) {}

val first = CounterExample(new BadlyBehaved())
val second = CounterExample(new BadlyBehaved())
assert(first.b == second.b)
assert(first.b.hashCode != second.b.hashCode)
assert(first == second)
assert(first.hashCode == second.hashCode, "Language Spec and implementation disagree") // FAILS
```

== What happened ==

```scala
java.lang.AssertionError: assertion failed: Language Spec and implementation disagree
```

== What expected ==
