Aladdin: **[http://scala-webapps.epfl.ch/bugtracking/bugs/displayItem.do?id=1296 bug 1296], [http://scala-webapps.epfl.ch/bugtracking/contribs/display.do?id=772 contrib 772]**

== Code ==

```scala
object Foo extends Application {
  val (a: Boolean, b: String) = Some(Some(4)) getOrElse (false, "Foo")

  println(a+" "+b)
}
```

== What happened ==

```scala
java.lang.ExceptionInInitializerError
        at Foo.main(bad2.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
        at java.lang.reflect.Method.invoke(Method.java:585)
        at scala.tools.nsc.ObjectRunner$$$$anonfun$$0.apply(ObjectRunner.scala:75)
        at scala.tools.nsc.ObjectRunner$$.withContextClassLoader(ObjectRunner.scala:49)
        at scala.tools.nsc.ObjectRunner$$.run(ObjectRunner.scala:74)
        at scala.tools.nsc.MainGenericRunner$$.main(MainGenericRunner.scala:154)
        at scala.tools.nsc.MainGenericRunner.main(MainGenericRunner.scala)
Caused by: scala.MatchError: Some(4)
        at Foo$$.(bad2.scala:2)
        at Foo$$.(bad2.scala)
        ... 10 more
```


== What expected ==

Compilation error
I don't think there's any bug there, the code is type correct. getOrElse is instantiated with 'Product', and Option is a subclass of Product. Then, the pattern match is well typed, since the scrutinee is a supertype of the cases (pair is a product too). If you want to get an error, you need to override the type inferencer:

```scala
val (a: Boolean, b: String) = Some(Some(4)).getOrElse[(Boolean, String)]((false, "Foo"))
```

By the way, why is Option a Product?
