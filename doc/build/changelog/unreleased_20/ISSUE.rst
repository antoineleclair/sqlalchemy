.. change::
    :tags: bug, asyncio
    :tickets: ISSUE
    :versions: 2.1.9

    Fixed issue where a cancellation received while an asyncio dialect was
    executing a statement would leave the driver cursor of that statement
    unclosed.  The connection is invalidated in this case, but the cursor
    object can outlive it through the traceback of the cancelled task; with
    aiosqlite, a ``SELECT`` left this way keeps a read lock on the database,
    so that the next commit on any other connection fails with "database is
    locked" until the cursor is garbage collected.  The cursor is now closed
    before the cancellation propagates.
