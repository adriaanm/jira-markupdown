Trac failed while I was trying to figure out how to search for open tickets on the Eclipse plugin.

==== How to Reproduce ====

While doing a POST operation on `/query`, Trac issued an internal error.

_(please provide additional details here)_


Request parameters:
{code}
{'__FORM_TOKEN': u'65907d901da68c9356bc11c8',
 'add': u'+',
 'add_filter': u_,
 'col': ['id',
         u'summary',
         u'type',
         u'status',
         u'priority',
         u'component',
         u'version'],
 'group': u_,
 'max': u'100',
 'order': u'priority',
 'owner': u'rewbs',
 'owner_mode': u_}
{code}


==== System Information ====

|| *Trac* || `0.11.2.1` ||
|| *Python* || `2.4.4 (SI-1, Apr 16 2008, 17:56:48) ` [[br]] `[GCC 4.1.2 20061115 (prerelease) (Debian 4.1.1-21)]` ||
|| *setuptools* || `0.6c3` ||
|| *SQLite* || `3.3.8` ||
|| *pysqlite* || `2.3.2` ||
|| *Genshi* || `0.5` ||
|| *mod_python* || `3.2.10` ||
|| *Pygments* || `1.1.1` ||
|| *Subversion* || `1.4.2 (r22196)` ||

==== Python Traceback ====
{code}
Traceback (most recent call last):
  File "/var/lib/python-support/python2.4/trac/web/main.py", line 432, in _dispatch_request
    dispatcher.dispatch(req)
  File "/var/lib/python-support/python2.4/trac/web/main.py", line 229, in dispatch
    req.session.save()
  File "/var/lib/python-support/python2.4/trac/web/session.py", line 97, in save
    (self.sid,))
  File "/var/lib/python-support/python2.4/trac/db/util.py", line 50, in execute
    return self.cursor.execute(sql_escape_percent(sql), args)
  File "/var/lib/python-support/python2.4/trac/db/sqlite_backend.py", line 58, in execute
    args or [])
  File "/var/lib/python-support/python2.4/trac/db/sqlite_backend.py", line 50, in _rollback_on_error
    return function(self, *args, **kwargs)
OperationalError: database is locked

{code}
    
