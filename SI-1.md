Aladdin: *[http://scala-webapps.epfl.ch/bugtracking/bugs/displayItem.do?id=1280 bug 1280], [http://scala-webapps.epfl.ch/bugtracking/contribs/display.do?id=752 contrib 752]*

== Code ==

File _test.scala_:
{code}
object Test
{code}

In shell:
{code}
$$ scaladoc test.scala
{code}

Result:

{code}
Exception in thread "main" java.lang.NullPointerException
        at scala.tools.nsc.doc.ModelFrames$$class.$$init$$(ModelFrames.scala:27)
        at scala.tools.nsc.doc.DocDriver.(DocDriver.scala:18)
        at scala.tools.nsc.Main$$generator$$1$$.(Main.scala:88)
        at scala.tools.nsc.Main$$.generator$$0(Main.scala:88)
        at scala.tools.nsc.Main$$.process(Main.scala:92)
        at scala.tools.nsc.Main$$.main(Main.scala:107)
        at scala.tools.nsc.Main.main(Main.scala)
{code}
