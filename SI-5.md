Aladdin: **[http://scala-webapps.epfl.ch/bugtracking/bugs/displayItem.do?id=1295 bug 1295], [http://scala-webapps.epfl.ch/bugtracking/contribs/display.do?id=770 contrib 770]**

== Code ==

```scala
class X { def unapply(v : Int) = Some(v + 1) }
val q = new X
5 match { case q(x) => x }
```

== What happened ==

```scala
java.lang.ClassCastException:  cannot be cast to 
```

== What expected ==

Prints 6. This works in the compiler, and works in the interpreter if I use an object rather than class + val.
It has something to do with unapply's when mixed with nested objects.  The following code generates the same error with the stand-alone compiler:
```scala
object A1 {
 object A2 {
   class X { def unapply(v : Int) = Some(v + 1) }
 }
}

object B1 {
  object B2 {
    val q = new A1.A2.X
  }
}

object Test {
  def main(args: Array[String]) {
    import B1.B2.q
    val res = 5 match { case q(x) => x }
    println(res)
  }
}
```

I have committed a test case at `test/pending/run/bugtr0005.scala`.

I don't know who should be assigned pattern-matching bugs right now. Burak, is it you still, or should it be passed to someone else or to devteam?
It was not related to the other bugs,  here is the thing after typer...
```scala
[[syntax trees at end of typer]]// Scala source: t5.scala
package <empty> {
  final object A1 extends java.lang.Object with ScalaObject {
    def this(): object A1 = {
      A1.super.this();
      ()
    };
    final object A2 extends java.lang.Object with ScalaObject {
      def this(): object A1.this.A2 = {
        A2.super.this();
        ()
      };
      class X extends java.lang.Object with ScalaObject {
        def this(): A2.this.X = {
          X.super.this();
          ()
        };
        def unapply(v: Int): Some[Int] = scala.Some[Int](v.+(1))
      }
    }
  };
  final object B1 extends java.lang.Object with ScalaObject {
    def this(): object B1 = {
      B1.super.this();
      ()
    };
    final object B2 extends java.lang.Object with ScalaObject {
      def this(): object B1.this.B2 = {
        B2.super.this();
        ()
      };
      private[this] val q: A1.A2.X = new A1.A2.X();
      <stable> <accessor> def q: A1.A2.X = B2.this.q
    }
  };
  final object Test extends java.lang.Object with ScalaObject {
    def this(): object Test = {
      Test.super.this();
      ()
    };
    def main(args: Array[String]): Unit = {
      <empty>;
      val res: Int = 5 match {
        case A1.A2.q.unapply(<unapply-selector>) <unapply> ((x @ _)) => x
      };
      scala.this.Predef.println(res)
    }
  }
}
```

and after explicitouter

```scala
  final class Test extends java.lang.Object with ScalaObject {
    def this(): object Test = {
      Test.super.this();
      ()
    };
    def main(args: Array[java.lang.String]): Unit = {
      val res: Int = {
        <synthetic> val temp2: Some[Int] = A1.A2.q().unapply(5);
        if (true)
          temp2.get()
        else
          throw new MatchError(5)
      };
      scala.this.Predef.println(res)
    }
```

and erasure

```scala
  final class Test extends java.lang.Object with ScalaObject {
    def this(): object Test = {
      Test.super.this();
      ()
    };
    def main(args: Array[java.lang.String]): Unit = {
      val res: Int = {
        <synthetic> val temp2: Some = A1.A2.$$asInstanceOf[object B1#B2]().q().unapply(5);
        if (true)
          scala.Int.unbox(temp2.get())
        else
          throw new MatchError(scala.Int.box(5))
      };
      scala.this.Predef.println(scala.Int.box(res))
    }
  }
```
