# Getting Started #

  1. Grab the latest build from the downloads. The file is named **`sqlite4java-NNN.zip`**, where NNN is the svn repository version used to build the library.
  1. The file contains:
    * `sqlite4java.jar` - place it on your CLASSPATH;
    * Native libraries (`*.dll, *.so, *.jnilib`) - place them in the same directory with `sqlite4java.jar`;
    * `sqlite4java-src.zip` - Java sources, add this file as the library sources in your IDE;
    * `sqlite4java-docs.zip` - javadocs for the reference (see also <a href='http://almworks.com/sqlite4java/javadoc/index.html'>javadocs online</a>).
  1. Run `java -jar sqlite4java.jar` to see the version of the library, the version of SQLite and compile-time options. This also verifies that native library loads normally on your platform.
  1. Get acquainted with classes SQLiteConnection and SQLiteStatement. Here's a short code sample:
```
    SQLiteConnection db = new SQLiteConnection(new File("/tmp/database"));
    db.open(true);
    ...
    SQLiteStatement st = db.prepare("SELECT order_id FROM orders WHERE quantity >= ?");
    try {
      st.bind(1, minimumQuantity);
      while (st.step()) {
        orders.add(st.columnLong(0));
      }
    } finally {
      st.dispose();
    }
    ...
    db.dispose();
```
  1. If you're doing a GUI application, create a separate thread for running SQLite and organize job queue.