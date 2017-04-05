unapplySeq[T] requires an Option[Seq[T]] output, but Seq is a huge trait with innumerable methods and sub-traits that have nothing to do with implementing the pattern matching. Thus a non-standard library, which uses a different collections library, is unable to return its collections without creating a copy that is a Seq. This is because for a non-standard library collection to extend a Seq requires implementing or including too much functionality from Scala's standard library.

The pattern matching feature has been conflated with the standard Scala library. If Scala wants to be truly open then it should allow for other libraries to work efficiently with compiler features, such as pattern matching. The variable argument extractor doesn't need most of the methods from Seq, it probably only needs a simple iterator. Given Scala claims to be scalable and compositional, I hope the minimum necessary trait for doing variable argument extraction can be defined and supported with a new unapplyXXX extractor.

The same principle could be applied to the use of the Option type for extractors. It would be a better design for the pattern matching compiler feature to not be entangled with the Option type. Instead, an alternative unapplyYYY extractor could return the value or null. I don't like null, but it is a lot less entangled with the standard library.

The point here is that the compiler functionality should not be entangled with the standard library in any way.

P.S. I am working on such a non-standard category theory collections library, that is much less complex than Scalaz, because I use novel subtyping paradigm and higher-kinds. I am also working on a language that compiles to Scala and interopts with Scala. I understand Scala was created to be toolset for experimentation in languages and thus I also assume libraries, so I am hoping I can encourage the removal of unnecessary conflation of library and compiler.
Your input is appreciated, but I'm afraid we are no longer accepting "Suggestion" tickets on JIRA.
Available mechanisms for making suggestions include:
- the Scala mailing lists
- SIP (Scala Improvement Process) and SLIP (Scala Library Improvement Process)
- a pull request on GitHub implementing the suggestion
