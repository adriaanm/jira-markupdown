From the main landing page of www.scala-lang.org, the hover menu labeled "Scala Developers" has an entry "Bug Tracking & Testing" that has a sub-entry "Scala Trac".  This leads to https://lampsvn.epfl.ch/trac/scala, which loads, but has a Python error within Trac.

I'm guessing the link should instead be named "Scala Jira" and link to issues.scala-lang.org 
The specific output from Trac that I get is:
{noformat}
Traceback (most recent call last):
  File "/usr/lib/python2.5/site-packages/trac/web/api.py", line 377, in send_error
    'text/html')
  File "/usr/lib/python2.5/site-packages/trac/web/chrome.py", line 739, in render_template
    data = self.populate_data(req, data)
  File "/usr/lib/python2.5/site-packages/trac/web/chrome.py", line 639, in populate_data
    d['chrome'].update(req.chrome)
  File "/usr/lib/python2.5/site-packages/trac/web/api.py", line 195, in __getattr__
    value = self.callbacks[name](self)
  File "/usr/lib/python2.5/site-packages/trac/web/chrome.py", line 494, in prepare_request
    for category, name, text in contributor.get_navigation_items(req):
  File "/usr/lib/python2.5/site-packages/trac/admin/web_ui.py", line 65, in get_navigation_items
    panels, providers = self._get_panels(req)
  File "/usr/lib/python2.5/site-packages/trac/admin/web_ui.py", line 161, in _get_panels
    p = list(provider.get_admin_panels(req))
  File "/usr/lib/python2.5/site-packages/trac/admin/web_ui.py", line 198, in get_admin_panels
    if 'TRAC_ADMIN' in req.perm:
  File "/usr/lib/python2.5/site-packages/trac/perm.py", line 527, in has_permission
    return self._has_permission(action, resource)
  File "/usr/lib/python2.5/site-packages/trac/perm.py", line 541, in _has_permission
    check_permission(action, perm.username, resource, perm)
  File "/usr/lib/python2.5/site-packages/trac/perm.py", line 428, in check_permission
    perm)
  File "/var/trac/LAMP/scala/plugins/authz_policy.py", line 145, in check_permission
  File "/var/trac/LAMP/scala/plugins/authz_policy.py", line 170, in get_authz_file
  File "/usr/lib/python2.5/posixpath.py", line 49, in isabs
    return s.startswith('/')
AttributeError: 'NoneType' object has no attribute 'startswith'
{noformat}
