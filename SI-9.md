Aladdin: *[http://scala-webapps.epfl.ch/bugtracking/bugs/displayItem.do?id=1290 bug 1290], [http://scala-webapps.epfl.ch/bugtracking/contribs/display.do?id=765 contrib 765]_

== Code ==

{code}
$$ cat Structural.scala


// for Gilles

def f[T](p: { def x[U <: T](t: U): Int }, t: T): Int = p.x(t)

class Base { val m = 9 }
class Derived extends Base {}

// either is sufficient
f[Int](new Object { def x[U <: Int](t: U) = t*t }, 4)
f[Derived](new Object { def x[V <: Base](t: V) = t.m }, new Derived())



$$ scala -version
Scala code runner version 2.6.0-RC2 -- (c) 2002-2007 LAMP/EPFL

$$ scala -nocompdaemon Structural.scala
Exception in thread "main" java.lang.Error: U cannot be instantiated from AnyRef{def x[U <: Int](U): Int}


(or
> Exception in thread "main" java.lang.Error: U cannot be instantiated from AnyR
ef{def x[U <: Derived](U): Int}
)


	at scala.tools.nsc.symtab.Types$$AsSeenFromMap.throwError$$0(Types.scala:2263)
	at scala.tools.nsc.symtab.Types$$AsSeenFromMap.instParam$$0(Types.scala:2266)
	at scala.tools.nsc.symtab.Types$$AsSeenFromMap.toInstance$$0(Types.scala:2281)
	at scala.tools.nsc.symtab.Types$$AsSeenFromMap.apply(Types.scala:2290)
	at scala.tools.nsc.symtab.Types$$AsSeenFromMap.apply(Types.scala:2221)
	at scala.List$$.loop$$0(List.scala:244)
	at scala.List$$.mapConserve(List.scala:261)
	at scala.tools.nsc.symtab.Types$$TypeMap.mapOver(Types.scala:2132)
	at scala.tools.nsc.symtab.Types$$AsSeenFromMap.apply(Types.scala:2292)
	at scala.tools.nsc.symtab.Types$$AsSeenFromMap.apply(Types.scala:2221)
	at scala.tools.nsc.symtab.Types$$TypeMap.mapOver(Types.scala:2141)
	at scala.tools.nsc.symtab.Types$$AsSeenFromMap.apply(Types.scala:2292)
	at scala.tools.nsc.symtab.Types$$Type.asSeenFrom(Types.scala:381)
	at scala.tools.nsc.symtab.Types$$Type.memberInfo(Types.scala:388)
	at scala.tools.nsc.symtab.Types$$class.scala$$tools$$nsc$$symtab$$Types$$$$specializesSym(Types.scala:3076)
	at scala.tools.nsc.symtab.Types$$$$anonfun$$75.apply(Types.scala:3069)
	at scala.tools.nsc.symtab.Types$$$$anonfun$$75.apply(Types.scala:3069)
	at scala.List.exists(List.scala:912)
	at scala.tools.nsc.symtab.Types$$class.specializesSym(Types.scala:3068)
	at scala.tools.nsc.symtab.SymbolTable.specializesSym(SymbolTable.scala:11)
	at scala.tools.nsc.symtab.Types$$Type.specializes(Types.scala:486)
	at scala.tools.nsc.symtab.Types$$$$anonfun$$68.apply(Types.scala:3004)
	at scala.tools.nsc.symtab.Types$$$$anonfun$$68.apply(Types.scala:3004)
	at scala.List.forall(List.scala:896)
	at scala.tools.nsc.symtab.Types$$class.isSubType0(Types.scala:3004)
	at scala.tools.nsc.symtab.Types$$class.isSubType(Types.scala:2879)
	at scala.tools.nsc.symtab.SymbolTable.isSubType(SymbolTable.scala:11)
	at scala.tools.nsc.symtab.Types$$Type.$$less$$colon$$less(Types.scala:469)
	at scala.tools.nsc.typechecker.Typers$$Typer.adapt(Typers.scala:732)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2940)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedBlock(Typers.scala:1315)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed1(Typers.scala:2703)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2938)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedArg(Typers.scala:1497)
	at scala.tools.nsc.typechecker.Typers$$Typer$$$$anonfun$$61.apply(Typers.scala:1513)
	at scala.tools.nsc.typechecker.Typers$$Typer$$$$anonfun$$61.apply(Typers.scala:1513)
	at scala.List$$.map2(List.scala:277)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedArgs(Typers.scala:1513)
	at scala.tools.nsc.typechecker.Typers$$Typer.doTypedApply(Typers.scala:1567)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedApply$$0(Typers.scala:2276)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed1(Typers.scala:2836)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2938)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2971)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedStat$$0(Typers.scala:1463)
	at scala.tools.nsc.typechecker.Typers$$Typer$$$$anonfun$$57.apply(Typers.scala:1493)
	at scala.tools.nsc.typechecker.Typers$$Typer$$$$anonfun$$57.apply(Typers.scala:1493)
	at scala.List$$.loop$$0(List.scala:244)
	at scala.List$$.mapConserve(List.scala:261)
	at scala.List$$.loop$$0(List.scala:248)
	at scala.List$$.mapConserve(List.scala:261)
	at scala.List$$.loop$$0(List.scala:248)
	at scala.List$$.mapConserve(List.scala:261)
	at scala.List$$.loop$$0(List.scala:248)
	at scala.List$$.mapConserve(List.scala:261)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedStats(Typers.scala:1493)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedBlock(Typers.scala:1314)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed1(Typers.scala:2703)
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
	at scala.tools.nsc.typechecker.Typers$$Typer.typedStats(Typers.scala:1493)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedTemplate(Typers.scala:1111)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedModuleDef(Typers.scala:1013)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed1(Typers.scala:2673)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2938)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2971)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedStat$$0(Typers.scala:1463)
	at scala.tools.nsc.typechecker.Typers$$Typer$$$$anonfun$$57.apply(Typers.scala:1493)
	at scala.tools.nsc.typechecker.Typers$$Typer$$$$anonfun$$57.apply(Typers.scala:1493)
	at scala.List$$.loop$$0(List.scala:244)
	at scala.List$$.mapConserve(List.scala:261)
	at scala.tools.nsc.typechecker.Typers$$Typer.typedStats(Typers.scala:1493)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed1(Typers.scala:2666)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2938)
	at scala.tools.nsc.typechecker.Typers$$Typer.typed(Typers.scala:2971)
	at scala.tools.nsc.typechecker.Analyzer$$typerFactory$$$$anon$$1.apply(Analyzer.scala:39)
	at scala.tools.nsc.Global$$GlobalPhase.applyPhase(Global.scala:243)
	at scala.tools.nsc.Global$$GlobalPhase$$$$anonfun$$2.apply(Global.scala:232)
	at scala.tools.nsc.Global$$GlobalPhase$$$$anonfun$$2.apply(Global.scala:232)
	at scala.Iterator$$class.foreach(Iterator.scala:375)
	at scala.collection.mutable.ListBuffer$$$$anon$$0.foreach(ListBuffer.scala:255)
	at scala.tools.nsc.Global$$GlobalPhase.run(Global.scala:232)
	at scala.tools.nsc.Global$$Run.compileSources(Global.scala:528)
	at scala.tools.nsc.ScriptRunner.compile$$0(ScriptRunner.scala:302)
	at scala.tools.nsc.ScriptRunner.withCompiledScript(ScriptRunner.scala:337)
	at scala.tools.nsc.ScriptRunner.runScript(ScriptRunner.scala:362)
	at scala.tools.nsc.MainGenericRunner$$.main(MainGenericRunner.scala:165)
	at scala.tools.nsc.MainGenericRunner.main(MainGenericRunner.scala)
{code}

== What happened ==

Compiler crash

== What expected ==

