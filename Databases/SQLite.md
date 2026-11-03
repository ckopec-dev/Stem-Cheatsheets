# SQLite on Ubuntu

[Tutorial](https://www.geeksforgeeks.org/python/python-sqlite/)

## Install 

`sudo apt install sqlite3`

## Optional dev packages

`sudo apt install libsqlite3-dev`

## Show version

`sqlite3 --version`

## Create or open a database file

`sqlite3 mydatabase.db`

## Common SQLite dot-commands (run inside the sqlite3 prompt):
  - .tables – List all tables in the database.
  - .databases – Show the attached database files.
  - .schema – View the CREATE TABLE statements.
  - .backup filename.bak – Back up the database.
  - .exit or .quit – Leave the SQLite shell.

## Example python code

~~~python
import sqlite3

# 1. Connect to a database file (it will be created if it doesn't exist)
# You can also use ':memory:' for a temporary database in RAM
conn = sqlite3.connect("my_database.db")

try:
    # 2. Create a cursor object to execute SQL commands
    cursor = conn.cursor()

    # 3. Create a table
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS users (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            email TEXT UNIQUE,
            age INTEGER
        )
    """)

    # 4. Insert data securely using placeholders (?) to prevent SQL injection
    user_data = ("Alice", "alice@example.com", 30)
    cursor.execute(
        "INSERT INTO users (name, email, age) VALUES (?, ?, ?)", user_data
    )

    # 5. Insert multiple rows at once
    more_users = [
        ("Bob", "bob@example.com", 25),
        ("Charlie", "charlie@example.com", 35)
    ]
    cursor.executemany(
        "INSERT INTO users (name, email, age) VALUES (?, ?, ?)", more_users
    )

    # 6. Commit changes to save them to disk
    conn.commit()

    # 7. Query and fetch data
    cursor.execute("SELECT * FROM users WHERE age > ?", (20,))
    
    # Fetch all rows (returns a list of tuples)
    rows = cursor.fetchall()
    for row in rows:
        print(f"ID: {row[0]}, Name: {row[1]}, Email: {row[2]}, Age: {row[3]}")

except sqlite3.Error as e:
    print(f"An error occurred: {e}")

finally:
    # 8. Always close the connection when finished
    conn.close()

~~~

## Use Parameterized Queries

`cursor.execute("SELECT * FROM users WHERE name = ?", (name_variable,))`

## Use Context Managers for Clean Transactions

~~~python
import sqlite3

conn = sqlite3.connect("my_database.db")
cursor = conn.cursor()

# The transaction is automatically managed here
with conn:
    cursor.execute("UPDATE users SET age = ? WHERE name = ?", (31, "Alice"))

conn.close()
~~~

## Integrating with Pandas

~~~python
import pandas as pd
import sqlite3

conn = sqlite3.connect("my_database.db")

# Read database table directly into a Pandas DataFrame
df = pd.read_sql_query("SELECT * FROM users", conn)

# Write a DataFrame back into a new SQL table
df.to_sql("backup_users", conn, if_exists="replace", index=False)

conn.close()
~~~
