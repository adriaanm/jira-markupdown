Aladdin: *[http://scala-webapps.epfl.ch/bugtracking/bugs/displayItem.do?id=1292 bug 1292], [http://scala-webapps.epfl.ch/bugtracking/contribs/display.do?id=764 contrib 764]*

== Code ==

{code}
trait Foo[T <: Foo[T, Enum], Enum <: Enumeration] {
  type StV = Enum#Value  
  type Meta = MegaFoo[T, Enum]

  type Slog <: Enumeration

  def getSingleton: Meta
}

trait MegaFoo[T <: Foo[T, Enum], Enum <: Enumeration] extends Foo[T, Enum] {
  def doSomething(what: T, misc: StV, dog: Meta#Event) = None
  abstract class Event
  object Event

  def stateEnumeration: Slog
  def se2: Enum
}

object E extends Enumeration {
  val A = Value
  val B = Value
}

class RFoo extends Foo[RFoo, E.type] {
  def getSingleton = MegaRFoo

  type Slog = E.type
}

object MegaRFoo extends RFoo with MegaFoo[RFoo, E.type] {
  def stateEnumeration = E
  def se2 = E
}
{code}

== What happened ==

{code}
bad.scala:31: error: error overriding method stateEnumeration in trait MegaFoo of type => MegaRFoo.this.Slog;
 method stateEnumeration has incompatible type => object E
  def stateEnumeration = E
  ^
bad.scala:32: error: error overriding method se2 in trait MegaFoo of type => E.type;
 method se2 has incompatible type => object E
  def se2 = E
  ^
two errors found
{code}

== What expected ==

Clean Compile
