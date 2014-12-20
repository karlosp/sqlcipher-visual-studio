SQLCipher for Visual Studio, building platforms x32 and x64
===========================

[sqlcipher-visual-studio][] is a simple port of the open-source [SQLCipher][]
library [[src](https://github.com/sqlcipher/sqlcipher)] for Visual Studio 2013.
Everything should work out of the box to compile x32 and x64 versions of sqlcipher.

  [sqlcipher-visual-studio]: https://github.com/karlosp/sqlcipher-visual-studio
  [SQLCipher]: http://sqlcipher.net/
  [SQLite3]: http://www.sqlite.org/

Versions
--------

`sqlcipher-visual-studio` is a combination of

  * [SQLite 3.8.6 (amalgamation)](http://sourceforge.net/projects/sqlite.mirror/files/SQLite%203.8.6/)
  * [SQLCipher 3.2.0](https://github.com/sqlcipher/sqlcipher/zipball/v3.2.0)

and has been successfully built with the following versions of OpenSSL:

  * [Win32 OpenSSL v1.0.1j](http://slproweb.com/download/Win32OpenSSL-1_0_1j.exe) 
  * [Win64OpenSSL v1.0.1j](http://slproweb.com/download/Win64OpenSSL-1_0_1j.exe)

The easiest way to manualy update with new version of SQLite or sqlcipher.
--------
 On linux or Cygwin run following commans:
  * git clone https://github.com/sqlcipher/sqlcipher.git
  * cd sqlcipher
  * ./configure --enable-tempstore=yes CFLAGS="-DSQLITE_HAS_CODEC" 
  * make

If everything went OK, you should be able to run sqlcipher with following command: ./sqlipher and you will get output like:
<pre>
SQLite version 3.8.6 2014-08-15 11:46:33
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite>
</pre>

Now  copy following source files from current direcotery (linux / Cygwin) -> to sqlcipher-visual-studio clone direcotry:
  * shell.c 		-> Shell/src
  * sqlite3.h 	-> StaticLib/inc
  * sqlite3.c 	-> StaticLib/src

And the final step:
Copy/paste following lines to beggining of file sqlite3.c
<pre>
/******** BEGIN SQLCIPHER-WINDOWS ********/
#define SQLITE_ENABLE_COLUMN_METADATA
#define SQLITE_ENABLE_UNLOCK_NOTIFY
#define SQLITE_HAS_CODEC
#define SQLITE_TEMP_STORE  2
/******** END SQLCIPHER-WINDOWS **********/
</pre>

Now you can build solution in Visual Studio.

Why are included OpenSSL headers and libraries?
--------
Because I just want to work EVERYTHING out of the box ;)
