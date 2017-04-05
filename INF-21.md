Hello,
the second example on http://www.scala-lang.org/node/111/ reads as follows:
{code}
object ComprehensionTest2 extends Application {
  def foo(n: Int, v: Int): Iterator[(Int, Int)] =
    for (i <- 0 until n;
         j <- i + 1 until n if i + j == v) yield
      Pair(i, j);
  foo(20, 32) foreach {
    case (i, j) =>
      println("(" + i + ", " + j + ")")
  }
}
{code}
This code does not compile on 2.8.0.final but results in the following error[[BR]]
<console>:7: error: type mismatch;
 found   : scala.collection.immutable.IndexedSeq[(Int, Int)]
 required: Iterator[(Int, Int)]
           for (i <- 0 until n;
                  ^ 
I suggest changing the code to
{code}
object ComprehensionTest2 extends Application {
  def foo(n: Int, v: Int): IndexedSeq[(Int, Int)] =
    for (i <- 0 until n;
         j <- i + 1 until n if i + j == v) yield
      Pair(i, j)
}

ComprehensionTest2.foo(20, 32) foreach {
  case (i, j) =>
     println("(" + i + ", " + j + ")")
}
{code}
which works and is more readable (at least to me).
In addition, the following text "This example shows that comprehensions are not restricted to lists. The previous program uses iterators instead." should be changed to "This example shows that comprehensions are not restricted to lists. The previous program uses a sequence instead."

Kind Regards,[[BR]]
sk300666
