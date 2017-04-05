I would like to be able to import directly into implicit scope. For example, rather than:

{code:java}
import collection.breakOut
val squares: Map[Int, Int] = Seq(1,2,3).map{ a => (a, a * a) }(breakOut)
{code}

I'd rather:

{code:java}
import implicit collection.breakOut
val squares: Map[Int, Int] = Seq(1,2,3) map { a => (a, a * a) }
{code}
Your input is appreciated, but I'm afraid we are no longer accepting "Suggestion" tickets on JIRA.
Available mechanisms for making suggestions include:
* the Scala mailing lists
* SIP (Scala Improvement Process) and SLIP (Scala Library Improvement Process)
* a pull request on GitHub implementing the suggestion
