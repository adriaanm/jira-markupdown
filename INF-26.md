For example �md5sum -c scala-2.8.0.RC1-installer.jar.md5� yields �md5sum: scala-2.8.0.RC1-installer.jar.md5: keine korrekt formatierte MD5‐Pr�fsummenzeile gefunden� (no correctly formatted MD5-checksum found)

Instead the program �md5sum� expects the MD5-file-format in the case of �scala-2.8.0.RC1-installer.jar� to look like �6f64e8faa1a4076620ad0739aa72effb  scala-2.8.0.RC1-installer.jar�. In general the input to �md5sum� should be the earlier generated output for the same file to calculate the MD5-checksum for.

Now the above program invocation of �md5sum -c ...� succeeds with �scala-2.8.0.RC1-installer.jar: Ok�.

How about changing the MD5-file-format for future releases of Scala distributions?
There are a number of md5-based programs, each with slightly different options and formats, therefore we will not include additional data in the checksum.
If you want to perform the verification using md5sum, just use:
{code}
FILE=your_filename
echo "$(cat ${FILE}.md5) ${FILE}" | md5sum -c -
{code}
