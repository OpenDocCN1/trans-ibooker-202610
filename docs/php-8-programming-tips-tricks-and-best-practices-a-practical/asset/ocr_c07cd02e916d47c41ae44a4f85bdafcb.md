+   - `comp_flags`: Specifies compression flags using the desired `ZipArchive::FL_*` constant(s)
+   - `enc_method`: Lets you specify the desired character encoding (using any of the `ZipArchive::FL_ENC_*` flags)
+   - `enc_password`: Lets you specify the encryption password if it is set for this ZIP archive

It's also worth mentioning here that the `remove_path` option prior to PHP 8 had to be a valid directory path. As of PHP 8, this option is a simple string that represents characters to be removed. This allows you to remove filename prefixes as well as undesired directory paths.

While we are still examining options, it's worth noting that two new encoding method class constants have been added: `ZipArchive::EM_UNKNOWN` and `ZipArchive::EM_TRAD_PKWARE`. Also, a new `lastId` property has been added so that you are able to determine the index value of the last ZIP archive entry.

# Other ZipArchive method changes

In addition to the changes mentioned earlier, a few other `ZipArchive` methods have changed in PHP 8. In this section, we summarize other `ZipArchive` method changes, as follows:

+   - `ZipArchive::extractTo()` previously used the current date and time for the modification time. As of PHP 8, this method restores the original file modification time.
+   - `ZipArchive::getStatusString()` returns results even after `ZipArchive::close()` has been invoked.
+   - `ZipArchive::addEmptyDir()`, `ZipArchive::addFile()`, and `ZipArchive::addFromString()` methods all have a new flags argument. You can use any of the appropriate `ZipArchive::FL_*` class constants, combined using bitwise operators.
+   - `ZipArchive::open()` can now open an empty (zero-byte) file.

Now you have an idea of the changes and improvements that have been introduced to the ZIP extension, let's examine changes in the area of regular expressions.