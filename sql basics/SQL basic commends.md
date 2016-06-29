# SQL commands and Python 

## Basic operations in sql

The basic fundtions that a sql database to accomplish is CRUD: create, read, update and delete. I'm going to look into the details of these four parts.

### Create

command:

> CREATE TABLE Users(name VARCHAR(128), email VARCHAR(128))

### Insert

command:

> INSERT INTO User(name, email) VALUES ('KRISTIN', 'kt@gmail.com')

### Read

command:

SELECT * FROM Users WHERE email = 'kt@gmail.com'

### Update

command:

> UPDATE Users SET name = 'Charles' WHERE email = 'csev@gmail.com'

### Join

command:

> SELECT * FROM Users JOIN Universities on Users.university_id = Universities.id

### Delete

command:

> DELETE FROM Users WHERE email = 'kf@gmail.com'

From the above commands we can have a quick impression: __sql commands are operating on the rows inside a table__

## Use python to manipulate a sql table

Now learn how python manipulate a sql table by analysing some codes:

```python
import sqlite3                # import the package

conn = sqlite3.connect('emaildb.sqlite')     # connect to an existing sql database
cur = conn.cursor()                          # set the cursor
 
cur.execute('''DROP TABLE IF EXISTS Counts''')   # drop the table if it exists

cur.execute('''CREATE TABLE Counts (email TEXT, count INTEGER)''')   
# created the table.  
# All the commands are the same as the original sql ones

fname = 'mbox-short.txt'
fh = open(fname)
for line in fh:
    if not line.startswith('From: ') : continue
    pieces = line.split()
    email = pieces[1]
    print email

    cur.execute('SELECT count FROM Counts WHERE email = ? ', (email, ))    
    # this is very interesting. The ? mark means it needs to be filled. 
    # (email, ) is to fill it. It has to be a tuple because otherwise 
    # the compiler thinks it  an expression

    row = cur.fetchone()
    if row is None:
        cur.execute('''INSERT INTO Counts (email, count) 
                VALUES ( ?, 1 )''', ( email, ) )
    else : 
        cur.execute('UPDATE Counts SET count=count+1 WHERE email = ?', (email, ))

    conn.commit()

# https://www.sqlite.org/lang_select.html
sqlstr = 'SELECT email, count FROM Counts ORDER BY count DESC LIMIT 10'

print
print "Counts:"
for row in cur.execute(sqlstr) :
    print str(row[0]), row[1]

cur.close()
```

From the above, we can see __cur.execute()__ is the most important operation in python to manipulate a sql table. The content inside () is almost the same original sql commands.

