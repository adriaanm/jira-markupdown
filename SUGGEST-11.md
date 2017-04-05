We were  recently reminded that initialization of lazy vals locks on the object holding the lazy val. This makes using lazy vals in a concurrent setting awkward if calculating a value is an expensive operation and thus the lock may be hold for a long time. It is especially expensive
if each (initializing) access to an easily calculated lazy val in the same object is then obstructed by a long-hold lock for calculating another lazy val. This would lead to a rule to use lazy vals only if
needed for performance reasons for long-running calculations but not for smaller calculations. This, however isn't possible because lazy vals are commonly needed to get around the initialization order
issues.

So, lazy vals, while idiomatic, are an obstacle to parallelization. This is a pity because in the common cake pattern you will likely compose several unrelated traits which may contain lazy vals which now can't be initialized in parallel which in turn slows down the initialization of applications built with the cake pattern for their central object. In an application of a certain size debugging these
problem is no fun because the central object will be called from everywhere and to properly parallelize you have to repeatedly start up your program to find all accesses to lazy vals in the call path (or
use a profiler).

One workaround I see right now is to manually touch the cheap lazy-vals to be used concurrently up front to be sure their calculation won't run into a lock. The other one is to use a special
LazyVal class like I used to do in Java and lock on it for the long-running calculations. Both workarounds aren't nearly as usable as the built-in keyword.

This may seem like a special case but IMO this will become more of a trap in the future if more software has to run concurrently. Therefore, lazy vals should be improved to have a lock per lazy val in the default case. A final solution to this problem would have to balance memory use (the per-lazy-val lock has to be saved somewhere) while still maintaining ease of use. David MacIver [once|http://scala-programming-language.1934581.n4.nabble.com/Question-on-lazy-val-td1940749.html] conceived of a way to do so.

Not only lazy val members of the object are concerned but other lazy vals in methods of the object as well when their initialization is lifted to the object. Additionally, initialization of all inner {{object}} also lock on the same object.
IMO, a possible solution should have these features:
 * Lock-free access
 * After initialization no heavier (memory/cpu) footprint than with the current solution
 * Independent locks for each lazy val, so that all lazy vals can be initialized in parallel
 * As little additional footprint as necessary to support that

At https://gist.github.com/1076016 I posted an example file to experiment with various solutions.

{{CurrentLazyHolder}} roughly represents the current solution.

{{SimpleManualLazyHolder}} and {{AdvancedManualLazyHolder}} are two new possible implementations. The idea is to reuse the field, which is going to hold the initialized value later on, to save a lock while initializing. As locks we just use simple objects of a known marker type. The access call-path stays the same as before.

{{SimpleManualLazyHolder}} simply pre-initializes the lock objects in the constructor of the owner. On access and after having checked that the value needs to be initialized the current value of the value field is checked. It may either still contain the lock object in which case the value is not yet initialized and the particular lock is entered to initialize the object. Otherwise, it may already contain the initialized value if another thread just finished initialization. Note that there's still a shared lock needed to update the bitmap (synchronized on the owner).

{{AdvancedManualLazyHolder}} works similarly but tries to defer creation of the lock object to the time when the lazy value is actually calculated. This makes it possible to not introduce any additional memory penalty in cases where lazy vals aren't accessed at all with the drawback of additional complexity.

There's still a solution needed for primitive values: Those are currently boxed and thus, access to them is slower than with the current solution. One could either accept that or introduce another field per primitive lazy val to hold the lock. I'm not sure it goes cheaper than that.

Some may argue that this change will bring a performance drawback to lazy val initialization and one possible answer to that would probably be to provide different implementations of lazy vals (e.g. flagged by annotations) for different situations. 
However, IMO, given how important and common lazy vals have become in Scala, it is important that there's only one lazy val implementation which works in a parallel setting and still is good enough in the common case where only one thread accesses lazy vals. 

One should certainly do the benchmarking before deciding for one solution but my take is, that betting on better ability to parallelize will be more beneficial than trying to tweak the last bits out of an implementation that is (or will be) fairly well optimized by the JVM.
Your input is appreciated, but I'm afraid we are no longer accepting "Suggestion" tickets on JIRA.

Available mechanisms for making suggestions include:
* the Scala mailing lists
* SIP (Scala Improvement Process) and SLIP (Scala Library Improvement Process)
* a pull request on GitHub implementing the suggestion
