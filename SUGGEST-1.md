It seems lots of test frameworks go to great lengths to reinvent the core syntax of Scala using other expressions (e.g. === in ScalaTest or with frameworks like hamcrest in Java) all to just be able to generate nice error messages if an assertion fails.

It would be awesome if the assert() and require() methods in Scala could generate a meaningful error message describing what failed.

for example

{code}
scala> val f = "Hey"
f: java.lang.String = Hey

scala> assert(f == "Bar")
java.lang.AssertionError: assertion failed
        at scala.Predef$$.assert(Predef.scala:87)
{code}

It would be nice if the default assertion message was more than just "assertion failed" but it showed the actual assertion expression ("f == "Bar") so it was more obvious what failed. Then this would work with any Scala expression...

{code}
assert(countries.any(_.population > 1000)) 
{code}

then if this failed...

{code}
java.lang.AssertionError: assertion failed: countries.any(_.population > 1000)
{code}


For this to work the assert()/require() methods would need the AST to be passed in somehow; or maybe this could be done using some CompilerPlugin to add an extra parameter to assert, giving the AST (or the String representation of the AST maybe?)

e.g. maybe calls like this

{{assert(someExpression)}}

could be replaced to this

{{assert(someExpression, "someExpression")}}

so that the proper error message would be generated?


Agreed! Some kind of expression tree support would be great; I tried searching the list but couldn't find details on if there was a working implementation around. 

But getting expression trees working is a big bit of work (which I'd really like BTW!). I was wondering if a simpler solution could be done first - where a compiler plugin/patch could just generate a String representation of the code within the assert() parens (to avoid having to have a full completed AST model etc). That way the AST model / expression tree stuff can change quite a bit - but all that is passed to assert() / require() as an implicit parameter is basically a string (or a class wrapping a string).

However this is done, this relatively small change would make all kinds of testing (JUnit/ScalaTest/Specs) *much* better - it would immediately make Scala the way to test any Java/Scala code - particularly with the existing testing DSLs like ScalaTest/Specs et al
Yay, my colleague Hiram Chirino has implemented it as a compiler plugin....
https://github.com/fusesource/jvmassert
