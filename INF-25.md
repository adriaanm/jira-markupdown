On http://www.scala-lang.org/tools/eclipse/index.html:

_* Create a new Scala project "hello"
First select the "New → Other..." item in the "File" menu, then chose "Scala Project" in the folder "Scala Wizards". Enter a name in the "Project name" field and press the "Finish" button (if asked, agree to switch to the Scala perspective)._

_* Create a new Scala package in source folder "src"
Right-click on the "hello" project in the "Package Explorer" tab of the projects pane and select the "New → Package" menu item; in the "New Package" window, enter "hello" in the "Name" field and press the "Finish" button._

_* Create a Scala object "HelloWorld" with main method
First expand the "hello" project tree and right-click on the "test" package; then select the "New → Scala Object" menu item and enter "HelloWorld" in the "Object name" field. Press the "Finish" button._

_* Extend the code to print a message
Make the HelloWorld object extend Appplication and add a println statement to the console._

_* Create a Run configuration for the Scala Project
Open the menu item "Run → Open Run Dialog..." and double-click "Scala Application". Set the "Main class" to hello.HelloWorld and hit the "Run" expand the "hello" project tree and right-click on the "test" package; then select the "New → Scala Object" menu item and enter "HelloWorld" in the "Object name" field. Press the "Finish" button._

I propose this to be changed to:

- Create a new Scala project "hello"
First select the "New → Other..." item in the "File" menu, then chose "Scala Project" in the folder "Scala Wizards". Enter 'hello' in the "Project name" field and press the "Finish" button (if asked, agree to switch to the Scala perspective).

- Create a new Scala package in source folder "src"
Right-click on the "hello" project in the "Package Explorer" tab of the projects pane and select the "New → Package" menu item; in the "New Package" window, enter "hello" in the "Name" field and press the "Finish" button.

- Create a Scala object "HelloWorld" with main method
First expand the "hello" project tree and right-click on the "hello" package; then select the "New → Scala Object" menu item and enter "HelloWorld" in the "Object name" field. Press the "Finish" button.

- Extend the code to print a message
Make the HelloWorld object extend Appplication and add a println statement to the console.

```scala
package hello
object HelloWorld extends Application {
  println("Hello, World!")
}

```

- Create a Run configuration for the Scala Project
Open the menu item "Run → Open Run Dialog..." and double-click "Scala Application". Set the "Main class" to hello.HelloWorld and hit the "Run" expand the "hello" project tree and right-click on the "hello" package. You should see the string 'Hello, World!' in the console view.
It's not obvious what you've changed other than correcting the package name mentioned under the third bullet (it was "test" it should be "hello").

Other than that the page contains a lot of outdated information. For instance, I don't think it makes sense to continue to refer to the old "stable" plugin, and the link to the 22nd May new plugin snapshot isn't really all that advisable either (the Eclipse update site at the end of that URL dates from 6th May in any case). Eclipse 3.4 should be probably be the recommended version now. I'd also like to see a link to the plugin hacking documentation [http://lampsvn.epfl.ch/trac/scala/wiki/EclipsePlugin here].

I think it would probably be best if Sean and I coordinate on updating this before the new Scala website goes live assuming we both have write access to the content.
