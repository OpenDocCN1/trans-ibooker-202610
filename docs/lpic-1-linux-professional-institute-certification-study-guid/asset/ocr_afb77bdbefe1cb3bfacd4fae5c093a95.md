**zip** The zip utility is different from the other data compression utilities in that it operates on multiple files. If you have ever created a zip file on a Windows operating system, then you've used this file format. Multiple files are packed together in a single file, often called a folder or an archive file, and then compressed. Another difference from the other Linux compression utilities is that zip does not replace the original file(s). Instead, it places a copy of the file(s) into an archive file.

To archive and compress files with zip, type **zip** followed by the final archive file's name, which traditionally ends in a `.zip` extension. After the archive file, type one or more names of files you desire to place into the compressed archive, separating them with a space. The original files remain intact, but a copy of them is placed into the compressed zip archive file. To reverse the operation, type **unzip** followed by the compressed archive file's name.

It's helpful to see a side-by-side comparison of the various compression utilities using their defaults. In Listing 4.25, an example on a CentOS Linux distribution is shown.

# Listing 4.25: Comparing the various Linux compression utilities

```
# cp /var/log/wtmp wtmp
#
# cp wtmp wtmp1
# cp wtmp wtmp2
# cp wtmp wtmp3
# cp wtmp wtmp4
#
# ls -lh wtmp?
-rw-r--r--. 1 root root 210K Oct 9 19:54 wtmp1
-rw-r--r--. 1 root root 210K Oct 9 19:54 wtmp2
-rw-r--r--. 1 root root 210K Oct 9 19:54 wtmp3
-rw-r--r--. 1 root root 210K Oct 9 19:54 wtmp4
#
# gzip wtmp1
# bzip2 wtmp2
# xz wtmp3
# zip wtmp4.zip wtmp4
adding: wtmp4 (deflated 96%)
#
# ls -lh wtmp?.*
-rw-r--r--. 1 root root 7.7K Oct 9 19:54 wtmp1.gz
-rw-r--r--. 1 root root 6.2K Oct 9 19:54 wtmp2.bz2
-rw-r--r--. 1 root root 5.2K Oct 9 19:54 wtmp3.xz
-rw-r--r--. 1 root root 7.9K Oct 9 19:55 wtmp4.zip
#
# ls wtmp?
wtmp4
#
```