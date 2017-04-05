Here are the headers when I try to look at source file in trunk via the web:

http://lampsvn.epfl.ch/svn-repos/scala/scala/trunk/src/compiler/scala/tools/nsc/interpreter/PackageCompletion.scala

```scala
HTTP/1.1 200 OK
Date: Sun, 14 Feb 2010 19:14:41 GMT
Server: Apache
Last-Modified: Mon, 08 Feb 2010 23:09:49 GMT
ETag: "20832//scala/trunk/src/compiler/scala/tools/nsc/interpreter/PackageCompletion.scala"
Accept-Ranges: bytes
Content-Length: 7028
Content-Type: application/octet-stream
```

That last means "don't display this in the browser, make them save it off to a file and fire up some external viewer" which is super annoying when we're talking about text after all.  Can this be changed?
