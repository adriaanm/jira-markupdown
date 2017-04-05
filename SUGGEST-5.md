Someone complained that ScalaTest 1.6.1 was not working on JDK 1.5. Their exception was:

Exception in thread "main" java.lang.NoSuchMethodError:
java.lang.String.isEmpty()Z
       at org.scalatest.NodeFamily$.getTestName(NodeFamily.scala:99)


I double checked and the ant file that generated it does say target="jvm-1.5" inside every scalac element. I also checked and isEmpty was added to java.lang.String in JDK 1.6.

I tried this with just scalac 2.9.0 (Sorry I don't have 2.9.0-1 to try it on), and I got the same problem. I used the source file Simple.scala:

class Simple {
  println("hi".isEmpty())
}

I compiled it with:

scalac -target:jvm-1.5 Simple.scala

When I do a javap -v Simple, I see the call to java.lang.String.isEmpty:

public Simple();
  Code:
   Stack=2, Locals=1, Args_size=1
   0:	aload_0
   1:	invokespecial	#10; //Method java/lang/Object."<init>":()V
   4:	getstatic	#16; //Field scala/Predef$.MODULE$:Lscala/Predef$;
   7:	ldc	#18; //String hi
   9:	invokevirtual	#24; //Method java/lang/String.isEmpty:()Z
   12:	invokestatic	#30; //Method scala/runtime/BoxesRunTime.boxToBoolean:(Z)Ljava/lang/Boolean;
   15:	invokevirtual	#34; //Method scala/Predef$.println:(Ljava/lang/Object;)V
   18:	return
  LineNumberTable: 
   line 2: 0
   line 3: 4


