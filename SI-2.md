Aladdin: *[http://scala-webapps.epfl.ch/bugtracking/bugs/displayItem.do?id=1159  bug 1159]*

== Code ==

{code}
class Outer {
  case class Foo(x: int, y: int) {
    override def equals(other: Any) = other match {
      case Outer.this.Foo(`x`, `y`) => true
      case _ => false
    }
  }
}

object Test extends Application {
  val o1 = new Outer
  val o2 = new Outer
  val x: Any = o1.Foo(1, 2)
  val y: Any = o2.Foo(1, 2)
  println(x equals y)
  x match {
    case o2.Foo(x, y) => println("error 1")
    case o1.Foo(x, y) => println("OK")
    case _ => println("error 2")
  }
}

{code}

== What happened ==

{code}
true
OK
{code}

== What expected ==

{code}
false
OK
{code}
(meta) haha, if you don't assign to anybody, trac picks "dubochet" (/meta)

This ticket is from open aladdin bug SI-1159.
