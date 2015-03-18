### Build 392 ###

  * SQLite upgraded to 3.8.7
  * Improved support for Android, added binary for x86-based Android.
  * Improved support for Mac OS X, fixed a number of issues with locating and loading binary.
  * Added OSGi bundle.
  * Added support for column metadata - see `SQLiteConnection.getTableColumnMetadata()` and `SQLiteColumnMetadata` class.
  * Added method `SQLiteConnection.isReadOnly()` to check if the database is opened on a read-only medium or with read-only flag.
  * Added support for non-ASCII characters in SQL (`exec`, `prepareStatement`).
  * Added `SQLiteConnection.setLimit()` method.
  * Added `SQLiteConnection.bind(...)` methods that take parameter names instead of parameter indexes.

Backwards compatibility note:

  * Dropped support for Mac OS X 10.4 or earlier, and PowerPC-based Macs.

### Build 282 ###

  * SQLite upgraded to 3.7.10
  * Added support for [SQLite Backup API](http://www.sqlite.org/backup.html) - see `SQLiteConnection.initializeBackup()` and `SQLiteBackup` class.
  * Added support for android with the new binary sqlite4java-android-armv7.so (no need to rename the binary).
  * Added support for extension loading - see `SQLiteConnection.setExtensionLoadingEnabled()` and `SQLiteConnection.loadExtension()`.
  * R-Tree support in SQLite compiled in.
  * Automatic searching for compatible binaries is rewritten and should be more stable. (It looks into sqlite4java.library.path, directory with sqlite4java.jar and java.library.path.)
  * Fixed issues with searching Long Array virtual table for non-integer values.

### Build 213 ###

  * SQLite upgraded to 3.7.4
  * SQLiteLongArray support in native code is completely rewritten. It is now reliable and optimized (you can specify that the numbers are sorted and/or unique to speed up lookups).

### Build 201 ###

  * New `SQLiteQueue` and `SQLiteJob` classes - a job queue implementation for accessing single-threaded SQLite connection in a multi-threaded or GUI application ([#3](http://code.google.com/p/sqlite4java/issues/detail?id=3)).
  * `SQLiteConnection.dispose()` method contract changed: it is now not allowed to call dispose from a different (non-confining) thread, due to possible JVM crash ([#18](http://code.google.com/p/sqlite4java/issues/detail?id=18)). If called from different thread, the method will fail softly.
  * New `SQLiteConnection.getOpenFlags()` method return the flags that were used to open the connection.
  * Updated contract for `SQLiteStatement.columnCount()`: now it can return the number of result columns before running the statement, or after statement has completed. However, the result in those cases is not stable (see javadocs)([#13](http://code.google.com/p/sqlite4java/issues/detail?id=13)).
  * Added binaries for Mac OS X 10.4 (Intel and PPC) ([#6](http://code.google.com/p/sqlite4java/issues/detail?id=6)).

Thanks to Olivier Monaco for patches and suggestions.

### Change Log Started ###