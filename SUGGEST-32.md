When combining {{List}} and {{Option}} in a for comprehension a confusing type error is reported.

I was writing code similar to this example:
{code:scala}
object Test {
  val list = List(1,2,3)
  val map = Map(1 -> List('a', 'b', 'c'), 3 -> List())
  for {
    // Pick a number from the list.
    number <- list

    // Lookup a list of chars in a map using the number as key.
    // Get returns an Option so use a generator to discard Nones.
    chars <- map.get(number)

    // Iterate over the chars.
    char <- chars
  } yield char
}
{code}
Compiling it, results in this error message:
{code}
$ scalac -version
Scala compiler version 2.10.3 -- Copyright 2002-2013, LAMP/EPFL
$ scalac test.scala 
test.scala:13: error: type mismatch;
 found   : List[Char]
 required: Option[?]
    char <- chars
         ^
one error found
$ 
{code}

The example above can be reduced to this code:
{code:scala}
object Test {
  for {
    xs <- Some(List(1,2,3))
    x <- xs
  } yield x
}
{code}
which can be mechanically rewritten to
{code:scala}
object Test {
  Some(List(1,2,3)) flatMap (xs => xs) map (x => x)
}
{code}
Compiling this code...
{code}
$ scalac test.scala 
test.scala:2: error: type mismatch;
 found   : List[Int]
 required: Option[?]
  Some(List(1,2,3)) flatMap (xs => xs) map (x => x)
                                   ^
one error found
$ 
{code}
...reveals the cause of the problem: Unlike other collections, {{Option}}'s {{flatMap}} cannot produce anything but an {{Option}}.

Converting the {{Option}} to a {{List}} using {{toList}} is a workaround but it requires insight into the collection implementation to discover this workaround.
This is just about a duplicate of SI-627 which was closed as Wont Fix. We really don't have a good way to even improve the error messages in this case given the way that for-comprehensions are specced and implemented.
This is not the same issue. In the issue you refer to the problem is that the resulting monad type of the for comprehension is the type of the first monad used in a generator. In my example that would be perfectly acceptable but my program doesn't even compile.

However, in this issue the underlying problem is that {{flatMap}} for {{Option}}
{code:scala}
def flatMap[B](f: A => Option[B]): Option[B]
{code}
is less flexible than flatMap for other monad types ({{TraversableLike}})
{code:scala}
def flatMap[B, That](f: A => GenTraversableOnce[B])(implicit bf: CanBuildFrom[Repr, B, That]): That
{code}

Manually converting the Option to an iterable removes the type error:
{code:scala}
  for {
    xs <- Option.option2Iterable(Some(List(1,2,3)))
    x <- xs
  } yield x
{code}

{{flatMap}} for {{Option}} should be enhanced to be able to produce other monad types like {{flatMap}} for other monadic types.
Hi Jason, thank you for the link. After reading the entire discussion I must agree completely with Kevin Wright. To quote his example
{code:scala}
scala> a map fn flatten
res3: Iterable[Int] = List(4, 4, 4, 4)

scala> a flatMap fn
<console>:8: error: type mismatch;
 found   : List[Int]
 required: Option[?]
       a flatMap fn
                 ^
{code}
This violates the general contract that
{code:scala}
xs flatMap f === xs map f flatten
{code}
To quote Kevin once more:
{quote}
I'm thinking this should be flagged up as a bug, it violates the principle of least surprise and is clearly going to catch people out as Scala adoption grows.
{quote}

Is there another bug report for this issue that I can watch?
I don't have much to add to the discussion – I believe the most relevant points has been made already. Also, you know more about the collections library, so I believe you when you say
{quote}
Personally, I just don't think there is a good way to solve this problem without compromising type safety for direct use of option and/or type inference.
{quote}

Kevin did propose a couple of other solutions, e.g. plan C
{quote}
Or even plan C.  I know it's a bit of extreme hackery, but I'm starting to think that the right fix is to add a sneaky .toSeq on options when desugaring a for-comprehension (and dealing with the maintenance tax of doing so).  This won't help the use-case for working directly with flatMap, but that's a less painful problem anyway - it's so much more obvious what's happening from the error message there.
{quote}

Finally, I would suggest that we keep this bug report open or create a separate one to keep in mind there is a problem that is unresolved.
Your input is appreciated, but I'm afraid we are no longer accepting "Suggestion" tickets on JIRA.
Available mechanisms for making suggestions include:
* the Scala mailing lists
* SIP (Scala Improvement Process) and SLIP (Scala Library Improvement Process)
* a pull request on GitHub implementing the suggestion
