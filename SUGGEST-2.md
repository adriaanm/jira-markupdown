I have an application that creates a Stream with Stream.iterate, but as it processes the items in the stream, it sometimes pushes new items onto the front of the stream. (The application uses +:, which didn't work as I expected, and for which I submitted #4697).

In the course of tracking down the problem, I created my own featherweight Stream implementation (attached), which contains only the Stream facilities I'm actually using.

In writing my Stream implementation, it occurred to me that when I'm pushing an item onto the front of an already-evaluated stream, it's probably not very efficient to wrap the stream in a thunk and then unwrap it again when I need it. Instead, I created a stream composed of two different kinds of Cons cells -- LazyCons takes a by-name tail, as in the standard implementation, but EagerCons takes a by-value tail (like a List cell), and is chosen in cases like +: where the tail is already evaluated.

If you run the timing test archontophoenix.stream.TimeStreams in the attached file, the numbers suggest that using EagerCons can save some time in cases like mine. Whether it saves enough time to be worth integrating something like this into the standard library is another question, but I thought I'd throw it out there.
Your input is appreciated, but I'm afraid we are no longer accepting "Suggestion" tickets on JIRA.

Available mechanisms for making suggestions include:
- the Scala mailing lists
- SIP (Scala Improvement Process) and SLIP (Scala Library Improvement Process)
- a pull request on GitHub implementing the suggestion
