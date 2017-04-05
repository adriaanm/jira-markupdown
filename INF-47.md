That would make it easier to keep track of changesets that finally fixed tickets. Otherwise you always have to grep through the changeset comments to find the relavant code changes.

See an example hook at http://trac.edgewall.org/browser/trunk/contrib/trac-post-commit-hook
Ok, done.

Documentation for the script :

It searches commit messages for text in the form of:
  command SI-1
  command SI-1, SI-2
  command SI-1 & SI-2
  command SI-1 and SI-2
Instead of the short-hand syntax "SI-1", "ticket:1" can be used as well, e.g.:
  command ticket:1
  command ticket:1, ticket:2
  command ticket:1 & ticket:2
  command ticket:1 and ticket:2
In addition, the ':' character can be omitted and issue or bug can be
used instead of ticket.
You can have more than one command in a message. The following commands
are supported. There is more than one spelling for each command, to make
this as user-friendly as possible.
  close, closed, closes, fix, fixed, fixes
    The specified issue numbers are closed with the contents of this
    commit message being added to it.
  references, refs, addresses, re, see
    The specified issue numbers are left in their current status, but
    the contents of this commit message are added to their notes.
A fairly complicated example of what you can do is with a commit message
of:
Changed blah and foo to do this or that. Fixes SI-10 and SI-12, and refs SI-12.
This will close SI-10 and SI-12, and add a note to SI-12.




