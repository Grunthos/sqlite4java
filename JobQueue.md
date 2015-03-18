### About Job Queue ###

The sqlite4java's job queue implementation helps in the following situations:
  * You need shared access to the same SQLiteConnection from different threads (as opposed to each thread [a pool](in.md) having its own SQLiteConnection).
  * You need to perform database operations in a GUI application, and you don't want to access the database in your GUI event queue thread (a very good idea).

In sqlite4java, SQLiteConnection is confined to the thread that opens the connection, so you can't simply share it among threads. The job queue's function is to start a separate thread, which will host the connection, and queue job tasks from other threads for asynchronous execution.

### Lifecycle ###

1. Start database queue
```
  SQLiteQueue queue = new SQLiteQueue(databaseFile);
  queue.start();
```

2. Place a job in the queue for execution
```
  queue.execute(new SQLiteJob<Object>() {
    protected Object job(SQLiteConnection connection) throws SQLiteException {
      // this method is called from database thread and passed the connection
      connection.exec(...);
      return null;
    }
  });
```

3. Stop the queue and wait until all pending jobs are finished executing
```
  queue.stop(true).join();
```

### Blocking Read: Use SQLiteJob's Future Interface ###

If you need to read something from SQLite and continue execution, you can use `get()` methods, provided by SQLiteJob's implementation of `java.util.concurrent.Future` interface.

Or, if you don't want to catch all the exceptions thrown by the `Future.get()`, you can use convenience method called `complete`:
```
  final String tableName = ...;
  int count = queue.execute(new SQLiteJob<Integer>() {
    protected Integer job(SQLiteConnection connection) throws SQLiteException {
      SQLiteStatement st = connection.prepare("SELECT count(*) FROM " + tableName);
      try {
        return st.step() ? st.columnInt(0) : 0;
      } finally {
        st.dispose();
      }
    }
  }).complete();
```

### Asynchronous Read: Override `jobFinished()` Method ###

If you need to do a job and then use the result to update UI, you don't want to block the execution of UI thread with `complete()` method. Instead:

```
  final String tableName = ...;
  queue.execute(new SQLiteJob<Integer>() {
    protected Integer job(SQLiteConnection connection) throws SQLiteException {
      SQLiteStatement st = connection.prepare("SELECT count(*) FROM " + tableName);
      try {
        return st.step() ? st.columnInt(0) : 0;
      } finally {
        st.dispose();
      }
    }
    
    protected void jobFinished(Integer result) {
      if (result != null) 
        callback.tableCount(tableName, result);
      else
        callback.problemHappened();
    }
  });
```

Wonder why you should override `jobFinished()` instead of simply calling `callback` at the end of `job()` method?

Because `jobFinished()` is called **always**, whether job has finished successfully, or it has thrown an exception, even when the job has been cancelled and its `job()` method actually never ran. Ignoring errors and cancellation may leave your UI application in some intermediate state.

There are also `jobCancelled()`, `jobError()` and `jobStarted()` methods, specifically designed for overriding. In all these methods you can inspect `this` instance of the `SQLiteJob` for its status.

### Job Queue and Transactions ###

Job queue implementation does not enforce any transaction policy - your jobs may start and commit/rollback transactions as needed. But it is strongly advised that transaction boundaries are kept within a single job, that is, if you `BEGIN TRANSACTION` in a job, you make sure you do `COMMIT` or `ROLLBACK` in the end.

If you leave transaction unfinished at the end of the job, locks will remain held (which may prevent concurrent connections from working with database), and you possible wouldn't know which job will execute next in the context of this unfinished transaction.

You can force transactions to rollback if they were not committed by overriding `afterExecute` method in the SQLiteQueue:
```
  SQLiteQueue queue = new SQLiteQueue(databaseFile) {
    protected void afterExecute(SQLiteJob job) {
      rollback();
    }
  };
```

### Extending Basic Implementation ###

SQLiteQueue is written with possibility for extending in mind. If you need more functionality than is provided in the basic version, inspect the protected methods assortment and it may be possible to do what you need by overriding default implementation.