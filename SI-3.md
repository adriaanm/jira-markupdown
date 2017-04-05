Aladdin: *[http://scala-webapps.epfl.ch/bugtracking/bugs/displayItem.do?id=1297 bug 1297]*

== Code ==

{code}
adriaan-moors-computer:~ adriaan$$ ledit ~/src/scala/trunk/build/quick/bin/scala
Welcome to Scala version 2.6.0.12630.20070822-153214.
Type in expressions to have them evaluated.
Type :help for more information.

scala> def foo[A, B, C](l: List[A], f: A => B=>B, g: B=>B=>C): List[C] = l map (g compose f)
{code}

== What happened ==

{code}
Exception in thread "main" java.util.NoSuchElementException: head of empty list
        at scala.Nil$$.head(List.scala:1193)
        at scala.Nil$$.head(List.scala:1190)
        at scala.tools.nsc.symtab.Types$$class.undoTo(Types.scala:2726)
        at scala.tools.nsc.symtab.Types$$class.isSubType(Types.scala:2888)
        at scala.tools.nsc.symtab.SymbolTable.isSubType(SymbolTable.scala:11)
        at scala.tools.nsc.symtab.Types$$Type.$$less$$colon$$less(Types.scala:469)
        at scala.tools.nsc.symtab.Types$$class.isSubArgs$$0(Types.scala:2950)
        at scala.tools.nsc.symtab.Types$$class.isSubArgs$$0(Types.scala:2951)
        at scala.tools.nsc.symtab.Types$$class.isSubType0(Types.scala:2955)
        at scala.tools.nsc.symtab.Types$$class.isSubType(Types.scala:2886)
        at scala.tools.nsc.symtab.SymbolTable.isSubType(SymbolTable.scala:11)
        at scala.tools.nsc.symtab.Types$$Type.$$less$$colon$$less(Types.scala:469)
        at scala.tools.nsc.typechecker.Infer$$Inferencer.isCompatible(Infer.scala:360)
        at scala.tools.nsc.typechecker.Infer$$Inferencer.isWeaklyCompatible(Infer.scala:364)
        at scala.tools.nsc.typechecker.Infer$$Inferencer.protoTypeArgs(Infer.scala:437)
        at scala.tools.nsc.typechecker.Typers$$Typer.doTypedApply(Typers.scala:1601)
        at scala.tools.nsc.typechecker.Typers$$Typer.typedApply$$0(Typers.scala:2276)
        at scala.tools.nsc.typechecker.Typers$$Typer.typed1(Typers.scala:2836)
        at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2938)
        at scala.tools.nsc.typechecker.Typers$$Typer.typedArg(Typers.scala:1497)
        at scala.tools.nsc.typechecker.Typers$$Typer.typedArgToPoly$$0(Typers.scala:1606)
        at scala.tools.nsc.typechecker.Typers$$Typer$$$$anonfun$$69.apply(Typers.scala:1615)
        at scala.tools.nsc.typechecker.Typers$$Typer$$$$anonfun$$69.apply(Typers.scala:1615)
        at scala.List$$.map2(List.scala:277)
        at scala.tools.nsc.typechecker.Typers$$Typer.doTypedApply(Typers.scala:1615)
        at scala.tools.nsc.typechecker.Typers$$Typer.typedApply$$0(Typers.scala:2276)
        at scala.tools.nsc.typechecker.Typers$$Typer.typed1(Typers.scala:2836)
        at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2938)
        at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2982)
        at scala.tools.nsc.typechecker.Typers$$Typer.transformedOrTyped(Typers.scala:3031)
        at scala.tools.nsc.typechecker.Typers$$Typer.typedDefDef(Typers.scala:1241)
        at scala.tools.nsc.typechecker.Typers$$Typer.typed1(Typers.scala:2679)
        at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2938)
        at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2971)
        at scala.tools.nsc.typechecker.Typers$$Typer.typedStat$$0(Typers.scala:1463)
        at scala.tools.nsc.typechecker.Typers$$Typer$$$$anonfun$$57.apply(Typers.scala:1493)
        at scala.tools.nsc.typechecker.Typers$$Typer$$$$anonfun$$57.apply(Typers.scala:1493)
        at scala.List$$.loop$$0(List.scala:244)
        at scala.List$$.mapConserve(List.scala:261)
{code}

*What expected*

no exception (if I uncomment my change to undoTo, the crash disappears, but it does not type check) Burak pointed out why it doesn't type check.. this works fine (i.e. no type error and no crash):

def foo[A, B, C](l: List[A], f: A => B=>B, g: (B=>B)=>C): List[C] = l map (g compose f)

