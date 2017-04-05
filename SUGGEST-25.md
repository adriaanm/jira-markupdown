= Code =
```scala
abstract class Arith
case class Plus(l: Arith,r: Arith) extends Arith
case class Prod(l: Arith,r: Arith) extends Arith
case class Number(n: Int) extends Arith
case class Var(x: String) extends Arith


def isEquation(expr: Arith): Boolean = expr match {
  case Plus(l,r) | Prod(l,r) => isEquation(l) || isEquation(r)
  case Number(_) => false
  case Var(_) => true
}

val expr = Plus(Number(1),Prod(Number(2),Number(3)))

isEquation(expr)
```

= What happened =

At compilation the following error is prompt
```scala
9: error: illegal variable in pattern alternative
  case Plus(l,r) | Prod(l,r) => isEquation(l) || isEquation(r) 
            ^ 
```

Though, this is normal as or-patterns are not allowed to bind variables.

= Enhancement = 

As the case classes **Plus** and **Prod** both accept a left and right argument of type **Arith**, it would be nice to allow binding of variables in the or-pattern definition.
Your input is appreciated, but I'm afraid we are no longer accepting "Suggestion" tickets on JIRA.
Available mechanisms for making suggestions include:
- the Scala mailing lists
- SIP (Scala Improvement Process) and SLIP (Scala Library Improvement Process)
- a pull request on GitHub implementing the suggestion
