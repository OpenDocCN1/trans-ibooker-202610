As you can see, the new relative date formats work quite well! We now briefly return to the cURL extension.

# Understanding cURL extension changes

The cURL extension leverages libcurl (http://curl.haxx.se/) to provide powerful and highly efficient HTTP client capabilities. In PHP 8, you must have libcurl version 7.29 (or above) installed on your server's operating system.

Another difference in PHP 8 is that this extension now uses objects rather than resources. This change was described earlier in this chapter, in Table 7.1, PHP 8 resource-to-object migration. An example was shown in the Potential code break involving is_resource() section. A side effect of this change is that any of the curl*close() functions are redundant, as the connection is closed when the object is unset or otherwise goes out of scope.

Let's now have a look at changes to the COM extension.

# Reviewing COM extension changes

**Component Object Model (COM)** is a Windows-only extension that enables programming code written in one language to call and interoperate with code written in any other COM-aware programming language. This information is important for any PHP developers who plan to develop PHP applications that run on Windows servers.

The most significant change to the COM extension is that case sensitivity is now automatically enforced. Accordingly, you can no longer import from type libraries any constants that are case-insensitive. In addition, you can no longer specify $case_insensitive, the second argument to the com_load_typelib() function, as FALSE.

Along these lines, COM extension php.ini settings that deal with case sensitivity have been altered. These include the following:

+   - `com.autoregister_casesensitive`: Permanently enabled in PHP 8.
+   - `com.typelib_file`: Any type libraries whose names end with either `#cis` or `#case_insensitive` no longer cause the constants to be treated as case-insensitive.

One change is a new php.ini setting, com.dotnet_version. This setting sets the **.NET** version to use for dotnet objects. We now examine other extension changes of note.