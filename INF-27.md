= problem =
SomeRandomScala someone referred to.

= analysis =
It affects readability, and considering that the wiki isn't really used that much it seems to be not a good default to convert these words to wiki links automatically.

= enhancement recommendation =
Quoting [http://trac.edgewall.org/wiki/CamelCase]
```scala
There's an option (ignore_missing_pages in the [wiki] section of TracIni) 
to simply ignore links to missing pages when the link is written 
using the CamelCase style, instead of that word being replaced by 
a gray link followed by a question mark.
```

I think this could be a good solution with a minimal amount of work.

Another enhancement would be to link these words to ScalaDoc instead, but I guess this is too much work for too less effort...
