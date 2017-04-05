Abstract: Declaring a case class saves boilerplate, but is inflexible because support for case classes is built-in. Various use cases require small variations of case classes. I argue that one should support them through a separate compiler plugin, activated by a "@case" annotation. Existing bugs which could be addressed should be linked here.

Programming in Scala (2nd edition, Sec. 1.1 page 51) states that "Instead of providing all constructs you might ever need in one "perfectly complete" language, Scala puts the tools for building such constructs into your hands".
However, case classes are an exception, and one often needs to generate only part of what "case" generates.
More in general, since Scala provides nothing resembling a macro language, neither syntactic nor lexical abstractions, Scala's syntax has quite a number of complex special cases - for instance case classes.
Lacking macros, Scala supports compiler plugins and annotations; I propose that case classes are deprecated and replaced by a plugin desugaring appropriate annotations and easy to extend.
An even better alternative would be to implement case classes through something like Template Haskell.
Thanks, that's exactly what I hoped for. In particular, http://scalamacros.org/usecases/data-types-a-la-carte.html says that "certain auto-implementations are frequently useful on their own, but, currently, it's all or nothing - either you declare your class as a case class and live with all restrictions being imposed, or you don't get any of the auto-generated goodies", which is a good description of what I meant above.

However, Odersky commented that case classes aren't as easy to generate through macros - http://www.scala-lang.org/node/8371#comment-35390 - but I guess that depends on the specific system (a macro transforming a class definition would be able to check if the user defines an {{equals}} method). Would your proposal be able to address this problem?
Jason Zaugg: sorry, missed your comment. However, if Eugene's is already planning to address this, I have nothing to add by opening a new discussion on scala-language. I hope the bugtracker entry is good as a meta-bug for issues like SI-5605.
An update on the status: Eugene's Scala'13 paper mentions this in one of the examples. I've also found an implementation here: https://github.com/scalamacros/paradise/blob/2.10.3/tests/src/main/scala/kase.scala

However, we should have an external library prototyping that. Also, we should split {{@kase}} into independent annotations. Eugene, are there already plans for that?
Your input is appreciated, but I'm afraid we are no longer accepting "Suggestion" tickets on JIRA.
Available mechanisms for making suggestions include:
* the Scala mailing lists
* SIP (Scala Improvement Process) and SLIP (Scala Library Improvement Process)
* a pull request on GitHub implementing the suggestion
