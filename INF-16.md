As per title. Noticed this because the source link for OpenHashMap is broken as it wasn't present in 2.7.1. 
There were two leftover references to 2.7.1 in the files:

META-INF/MANIFEST.MF[[BR]]
src/compiler/scala/tools/nsc/doc/script.js

they should both be references to 2.7.2, specifically 2.7.2.RC2 at the moment. I am getting svn commit errors right now, I'll check with Fabien why and commit soon.

