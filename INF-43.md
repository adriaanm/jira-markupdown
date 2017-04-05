If you lost your Scala Trac password, you will not be able to login to the site again (as in 'never ever'). You may request the password and a new one will be sent to you but at next login trac will end in a redirect loop. It is even difficult to register a new login because of problems with clearing the password/cookies. (Although that probably may be due to my inability to use my browser effectively.)

See http://trac-hacks.org/ticket/3233. 

A new version of the account manager plugin is needed or as a workaround 
{code}
[account-manager] force_change_passwd = false 
{code}
will help probably.
Ok, finally, I was able to reproduce the bug. I made a small modification in the python code.
To be able to reset your password, you need to quit your browser and start it again *before* asking for a new password, so that you don't inherit your old session.
