# Comparison to Other Java Wrappers for SQLite #
_As of 2010-06-01_

Current list of SQLite wrappers for Java can be found in [SQLite Wiki](http://www.sqlite.org/cvstrac/wiki?p=SqliteWrappers).

**Note on JDBC drivers**: we specifically have created a wrapper for SQLite, not a JDBC driver, so that we can have features important for a GUI application -- good and **optimizable** performance (we can add performance hacks as needed), low garbage generation, predictable and responsive behavior. The tradeoff is that without JDBC, application loses independence from database vendor. But in many cases, this independence is very illusionary and you cannot easily migrate to a different database.

For that reason, we don't compare sqlite4java with JDBC drivers here. You either need a JDBC driver, or a wrapper.

### 1. Javasqlite Wrapper/JDBC driver from Christian Werner, http://www.ch-werner.de/javasqlite ###

This is probably the most widely used library. We first tried to use it for our purposes, but dropped it for the following reasons:
  * Little regard to how Java works, for example, by using `native finalize()` method as a destructor (!), which would generate exceptions, and theoretically can ruin the database;
  * Very Java-unfriendly naming of classes and methods;
  * Absense of bulk-load performance hacks, which makes loading queries with large results really slow.

However, the library has certain advantages:
  * Support for user-defined SQL functions via callbacks (yet, one wouldn't expect great performance from a JNI callback-supported SQL function);
  * Support for authorizer and progress handler.

### 2. SQLite JDBC Driver from Xerial, http://www.xerial.org/trac/Xerial/wiki/SQLiteJDBC ###

Probably the best and most up-to-date JDBC driver for SQLite as of the time of this writing.

### 3. Sqlite wrapper from TK-Software, http://tk-software.home.comcast.net/~tk-software/# ###

Largely outdated, Java-unfriendly and lacks some basic functions, like `bind` methods.

### 4. SqliteJDBC from David Crawshaw, http://www.zentus.com/sqlitejdbc/ ###

This is a JDBC driver. Besides, pure java version is said to be much slower than the original SQLite.

### 5. SQLite for Java from Rodolfo d'Ettorre, http://rodolfo_3.tripod.com/index.html ###

Only for Windows, very basic functionality, outdated.

### 6. SQLite JDBC Driver for Mysaifu JVM, http://sourceforge.jp/projects/sqlitejdbc ###

Hard to get any information on this one.

### 7. SQLJet from TMate Software, http://www.sqljet.com ###

This is not a real SQLite wrapper, but a pure Java for working with SQLite databases on the low level.

### 8. Android SDK, http://developer.android.com/reference/android/database/sqlite/package-summary.html ###

Android SDK seems to have a good wrapper interfaces in package `android.database.sqlite`, yet it's not clear whether it can be used outside Android OS.