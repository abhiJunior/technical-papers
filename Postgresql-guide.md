# PostgreSQL from Zero — Bootcamp Review Guide

A step-by-step reference for: given a CSV file, create a user, create a database, load the CSV, and run queries — all from a plain Linux terminal, no GUI.

---

## 0. The one confusion to clear up first: `sudo` vs `psql -U ...`

This trips everyone up at first. There are **two different "logins" happening**, and you must not mix them up.

| | Who are you? | When do you use it? | Can it create/drop users & databases? |
|---|---|---|---|
| `sudo -u postgres psql ...` | The Postgres **superuser** (`postgres`) | Admin tasks: creating a user, creating a database, dropping a user, dropping a database | ✅ Yes |
| `psql -U ipl_user -d ipl_db -h localhost -W` | A **normal role** (`ipl_user`) | Everyday work: loading CSVs, running `SELECT`/`INSERT`/etc. queries on the database you own | ❌ No — a normal user can't create other users or drop databases |

**Simple rule of thumb:**
- Setting things up or tearing things down (user/database level) → `sudo -u postgres psql -f file.sql`
- Working with data inside a database you already own (tables, queries) → `psql -U <your_user> -d <your_db> -h localhost -W -f file.sql`

You will **switch back and forth** between these two identities throughout the exercise. That's normal and expected — it's not a mistake.

---

## 1. Two ways to run SQL: one-shot vs interactive shell

`psql` can be used two ways:

**A) One-shot (non-interactive)** — runs a `.sql` file top to bottom, prints output, then returns you to bash:
```bash
psql -U ipl_user -d ipl_db -h localhost -W -f sql/some_script.sql
```
Use this for anything you've already written and saved as a `.sql` file (setup scripts, cleanup scripts, the CSV loader).

**B) Interactive shell** — drops you into a live prompt where you type SQL and see results immediately:
```bash
psql -U ipl_user -d ipl_db -h localhost -W
```
Your terminal prompt changes to something like:
```
ipl_db=>
```
This means **you are now inside psql, not bash**. Regular bash commands (`ls`, `cd`) won't work here. You type SQL statements, each ending in `;`, and press Enter after each one.

To leave and go back to bash:
```sql
\q
```

**Important:** Inside the interactive shell, "meta-commands" starting with `\` (like `\dt`, `\l`, `\du`) only take arguments on the **same line**. Don't paste a `\dt` and a `SELECT ...` together as one block — run them one at a time, pressing Enter after each.

---

## 2. Step 1 — Create the user and database

You'll be handed a CSV. Before touching it, set up a place for it to live.

**Write `sql/01_create_database.sql`:**
```sql
CREATE USER ipl_user
WITH PASSWORD 'ipl_password';

CREATE DATABASE ipl_db
OWNER ipl_user;
```

Notes:
- Use **single quotes `' '`** for the password — it's a text value. Double quotes `" "` mean something different in SQL (an identifier name) and will error.
- `OWNER ipl_user` makes this user the owner, so they automatically have full rights over it.

**Run it as the superuser** (creating users/databases is an admin action):
```bash
sudo -u postgres psql -f sql/01_create_database.sql
```

Expected output:
```
CREATE ROLE
CREATE DATABASE
```

**Verify it worked** (still as superuser, one-off inline commands with `-c`):
```bash
sudo -u postgres psql -c "\du"
sudo -u postgres psql -c "\l"
```
- `\du` = list roles/users
- `\l` = list databases (check `ipl_db` shows `ipl_user` as Owner)

---

## 3. Step 2 — Load the CSV into a table

**Look at the CSV first.** Open it and check the header row (first line) — that tells you the exact column names and how many columns there are. Your `CREATE TABLE` must match this.

```bash
head -5 data/yourfile.csv
```

**Write `sql/02_load_csv.sql`.** General template:
```sql
DROP TABLE IF EXISTS your_table;

CREATE TABLE your_table (
    col1   INTEGER PRIMARY KEY,
    col2   TEXT,
    col3   DATE,
    col4   INTEGER
    -- match this exactly to the CSV header, in order
);

\copy your_table FROM 'data/yourfile.csv' WITH (FORMAT csv, HEADER true)

SELECT COUNT(*) FROM your_table;
```

Key things to remember:
- **Every statement ends with `;`** — a missing semicolon makes psql glue the next line onto the same statement and throw confusing errors.
- `\copy` (not `COPY`) — `\copy` runs from *your* machine reading the file with your normal permissions. `COPY` (no backslash) runs on the Postgres *server* and needs the file to be readable by the server process — usually more hassle. Always prefer `\copy` for this kind of exercise.
- **If your table has more/fewer columns than the CSV** (e.g. an auto-generated `id` that isn't in the CSV), list the columns explicitly so they line up correctly:
  ```sql
  \copy your_table (col2, col3) FROM 'data/yourfile.csv' WITH (FORMAT csv, HEADER true)
  ```
- **If two tables are related** (one has a `REFERENCES other_table(id)` foreign key), always `DROP` the table with the foreign key *first*, then the one it points to. Same order in reverse when creating: parent table first, then the one referencing it.

**Run it as your normal user, not the superuser** — this is everyday data work in a database you already own:
```bash
psql -U ipl_user -d ipl_db -h localhost -W -f sql/02_load_csv.sql
```
- `-h localhost` forces password-based login (without it, Postgres tries to match your *Linux* username to a role, which fails since your Linux username isn't `ipl_user`).
- `-W` makes it prompt you for the password.

Expected output ends with something like:
```
CREATE TABLE
COPY 636
 count
-------
   636
```

---

## 4. Step 3 — Run queries

Once loaded, you can either:

**A) Write query files and run them one at a time:**
```bash
psql -U ipl_user -d ipl_db -h localhost -W -f sql/03_query_something.sql
```

**B) Or go interactive** and type queries live — good for the review itself, since you can react to what they ask on the spot:
```bash
psql -U ipl_user -d ipl_db -h localhost -W
```
```sql
\dt                              -- list all tables
\d your_table                    -- show a table's column structure
SELECT * FROM your_table LIMIT 5;
SELECT COUNT(*) FROM your_table;
SELECT col2, COUNT(*) FROM your_table GROUP BY col2 ORDER BY COUNT(*) DESC;
```
Leave with `\q` when done.

---

## 5. Step 4 — Clean up

When you're fully done (end of the review, or before rebuilding from scratch), tear everything down.

**Write `sql/04_cleanup.sql`:**
```sql
-- Disconnect anyone still connected to the database
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE datname = 'ipl_db' AND pid <> pg_backend_pid();

-- Dropping the database removes ALL its tables automatically
DROP DATABASE IF EXISTS ipl_db;

-- Drop the user last (must happen after the database is gone,
-- since the user owns it)
DROP USER IF EXISTS ipl_user;
```

**Before running this, make sure you're not currently sitting inside `ipl_db=>`.** If you are, exit first:
```sql
\q
```

Then run cleanup as the superuser:
```bash
sudo -u postgres psql -f sql/04_cleanup.sql
```

---

## 6. Quick command cheat sheet

| Command | What it does |
|---|---|
| `sudo -u postgres psql -f file.sql` | Run a SQL file as the Postgres admin (user/database-level tasks) |
| `psql -U user -d db -h localhost -W -f file.sql` | Run a SQL file as a normal role (table/data-level tasks) |
| `psql -U user -d db -h localhost -W` | Open an interactive SQL shell as that role |
| `\q` | Quit the interactive shell, back to bash |
| `\dt` | List tables in the current database |
| `\d table_name` | Show a table's columns and types |
| `\l` | List all databases |
| `\du` | List all roles/users |
| `\c dbname` | Switch to a different database (inside psql) |

---

## 7. Common errors and what they actually mean

| Error | What's really happening | Fix |
|---|---|---|
| `role "yourname" does not exist` | You ran `psql` with no `-U`, so it tried to log you in as a role matching your Linux username | Add `-U <role> -h localhost -W` |
| `fe_sendauth: no password supplied` | You used `-w` (lowercase, "never prompt") instead of `-W` (uppercase, "always prompt") | Use `-W` |
| `cannot drop table X because other objects depend on it` | Another table has a foreign key pointing at X | Drop the dependent table first |
| `relation "X" already exists` | A previous `DROP TABLE` failed (often due to the FK issue above), so `CREATE TABLE` hit a table that's still there | Fix the underlying `DROP` order, then rerun |
| `invalid input syntax for type integer: "some text"` during `\copy` | The CSV's columns don't line up with the table's columns in order | List columns explicitly: `\copy table (col_a, col_b) FROM ...` |
| `No such file or directory` on `\copy` | Wrong relative path, or not running psql from the right folder | Run `pwd` and `ls data/` to confirm you're in the project root and the file is where you think |
| `syntax error at or near "..."` | Almost always a missing `;` at the end of the previous statement | Check every statement above the error ends with `;` |