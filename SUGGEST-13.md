I would like this feature, and when I thought about it in the past I liked the following syntax:
```scala
   "2 + 2 = \{2+2}.  I want some pie."
```
Essentially, {} is a new kind of backslash escape in strings.  This would help with many issues where you build strings, but would not help with the issue you mention regarding regexes and backslashes.  (For those, the """ quotes seem good.)

Somehow I find the \{} much easier to look at than the version you would write right now:
```scala
   "2 + 2 = "+(2+2)+"I want some pie."
```

You will notice, though, that I do not like it enough to have put together a patch even after two years of very serious Scala hacking.
Yeah, I actually knew about - I recently threw together the following hack (you can hopefully infer what the missing general-purpose functions are):

```scala
packag templates
import commons.Control._
import commons.Io._
import scala.collection.mutable._
object Templates {
  object StackItem extends Enumeration {
    val O,E,Q = Value
  }
  import StackItem._
  def main(args: Array[String]) {
    var e,q = false
    val stack = new Stack[Value]
    stack push O
    val in = using (TextReader(System.in)) (_ read)
    using (TextWriter(System.out)) { out =>
      val xs = for (c <- in) yield {
        c match {
          case '`' =>
            if (stack.top != E) { stack push E; "\"\"\"}{" }
            else { stack pop; "}{\"\"\"" }
          case '~' =>
            if (stack.top != Q) { stack push Q; "<p>{\"\"\"" }
            else { stack pop; "\"\"\"}</p>.text" }
          case _   => "" + c
        }
      }
      assert (stack.top == O)
      out println (xs mkString "")
    }
  }
}
```

The only key here was that I chose to use characters ` and ~ that I would probably not use in either the Scala language or the output language(s). It's total rubbish for that reason, but a smarter parser could be made to accept configurable delimiters.

I initially needed this for a project where I was using Scala to generate Java code. Since then, I found it useful in a number of other contexts. For instance, just now I used it to script the psql to produce some datasets, followed by the scripting of gnuplot to plotting these datasets. In both cases, this I was simply producing output files to be piped into psql and gnuplot. But with some more flexible quoting mechanisms similar to this, along with some combinators to manage processes, could all but replace bash in a lot of situations.
One benefit of string interpolation is that it can be implemented more efficiently than using string concatenation, and without the boilerplate of setting up any StringBuilders (or otherwise hoping that the JVM performs this translation).

Another, and the main benefit, is simply that it is a much-less-verbose way to generate strings!

(Before reading ahead, note again that I am not endorsing these exact quoting characters. Replace ` and ~ with anything you want, e.g. posix regex's s||| or bash's $$() or Perl's qq() or C#'s @"".)

```scala
println(~
import java.util.*;
`imports`
public static class `classname` {
  public static void main(String[] args) {
    if (args.length > `minCount`) {
      switch (`decodeExpr(~args[`minCount - 1`] << 1~)`) {
        `
          for ((name,contents) <- states) yield {
            ~case `name.toUpperCase`: {
              `contents`
              break;
            }~
          }
        `
      }
    }
  }
}
~)
```

The method using 

```scala
println("""
import java.util.*;
""" + imports + """
public static class """ + classname + """ {
  public static void main(String[] args) {
    if (args.length > """ + minCount + """) {
      switch (""" + decodeExpr(<p>args[{minCount - 1}] &lt;&lt; 1</p>) + """) {
        """ + {
          for ((name,contents) <- states) yield {
            """case """ + name.toUpperCase + """: {
              """ + contents + """
              break;
            }""" +
          }
        } + """
      }
    }
  }
}
""")
```

The former can be more efficient by, e.g., generating code using Scala XML's interpolation facilities.
This became [SIP-11](http://docs.scala-lang.org/sips/pending/string-interpolation.html), and has now been added to trunk under `-Xexperimental`, with a few limitations compared to SIP's proposal. I'm not sure how's the lifecycle of suggestions supposed to be, but given that it became a SIP, shouldn't this be closed?
