Aladdin: **[http://scala-webapps.epfl.ch/bugtracking/bugs/displayItem.do?id=1294 bug 1294], [http://scala-webapps.epfl.ch/bugtracking/contribs/display.do?id=766 contrib 766]**

== Code ==

```scala
test1.scala:

package foo.bar
class Bar

---
test2.scala:

package foo
import bar._
class Foo

---

> scalac -Ybrowse:namer test1.scala test2.scala
```
In tree, click on the CompilationUnit[test2.scala]/PackageDef/Import => exception gets thrown, and RH information panel is missing.

== What happened ==

```scala
Exception in thread "AWT-EventQueue-0" java.lang.Error: Unknown case: ImportType(bar)
        at scala.tools.nsc.ast.TreeBrowsers$$TypePrinter$$.toDocument(TreeBrowsers.scala:663)
        at scala.tools.nsc.ast.TreeBrowsers$$TreeInfo$$.symbolTypeDoc(TreeBrowsers.scala:545)
        at scala.tools.nsc.ast.TreeBrowsers$$TextInfoPanel.update(TreeBrowsers.scala:220)
        at scala.tools.nsc.ast.TreeBrowsers$$BrowserFrame$$$$anon$$2.valueChanged(TreeBrowsers.scala:178)
        at javax.swing.JTree.fireValueChanged(JTree.java:2399)
        at javax.swing.JTree$$TreeSelectionRedirector.valueChanged(JTree.java:2770)
        at javax.swing.tree.DefaultTreeSelectionModel.fireValueChanged(DefaultTreeSelectionModel.java:629)
        at javax.swing.tree.DefaultTreeSelectionModel.notifyPathChange(DefaultTreeSelectionModel.java:1078)
        at javax.swing.tree.DefaultTreeSelectionModel.setSelectionPaths(DefaultTreeSelectionModel.java:287)
        at javax.swing.tree.DefaultTreeSelectionModel.setSelectionPath(DefaultTreeSelectionModel.java:170)
        at javax.swing.JTree.setSelectionPath(JTree.java:1174)
        at javax.swing.plaf.basic.BasicTreeUI.selectPathForEvent(BasicTreeUI.java:2296)
        at javax.swing.plaf.basic.BasicTreeUI$$Handler.handleSelectionImpl(BasicTreeUI.java:3509)
        at javax.swing.plaf.basic.BasicTreeUI$$Handler.handleSelection(BasicTreeUI.java:3484)
        at javax.swing.plaf.basic.BasicTreeUI$$Handler.mousePressed(BasicTreeUI.java:3465)
        at java.awt.AWTEventMulticaster.mousePressed(AWTEventMulticaster.java:222)
        at java.awt.Component.processMouseEvent(Component.java:5498)
        at javax.swing.JComponent.processMouseEvent(JComponent.java:3135)
        at java.awt.Component.processEvent(Component.java:5266)
        at java.awt.Container.processEvent(Container.java:1966)
        at java.awt.Component.dispatchEventImpl(Component.java:3968)
        at java.awt.Container.dispatchEventImpl(Container.java:2024)
        at java.awt.Component.dispatchEvent(Component.java:3803)
        at java.awt.LightweightDispatcher.retargetMouseEvent(Container.java:4212)
        at java.awt.LightweightDispatcher.processMouseEvent(Container.java:3889)
        at java.awt.LightweightDispatcher.dispatchEvent(Container.java:3822)
        at java.awt.Container.dispatchEventImpl(Container.java:2010)
        at java.awt.Window.dispatchEventImpl(Window.java:1778)
        at java.awt.Component.dispatchEvent(Component.java:3803)
        at java.awt.EventQueue.dispatchEvent(EventQueue.java:463)
        at java.awt.EventDispatchThread.pumpOneEventForHierarchy(EventDispatchThread.java:242)
        at java.awt.EventDispatchThread.pumpEventsForHierarchy(EventDispatchThread.java:163)
        at java.awt.EventDispatchThread.pumpEvents(EventDispatchThread.java:157)
        at java.awt.EventDispatchThread.pumpEvents(EventDispatchThread.java:149)
        at java.awt.EventDispatchThread.run(EventDispatchThread.java:110)
```

== What expected ==

No exception, and display of RH information panel.
