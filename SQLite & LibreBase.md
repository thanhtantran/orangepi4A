# SQLite & LibreBase
## Pre-requisites
`sudo apt-get install unixodbc-dev  unixodbc libsqliteodbc sqlite3 libsqlite3-dev` & ensure proper libraries are referred by `cat /etc/odbcinst.ini`
```
[SQLite]
Description=SQLite ODBC Driver
Driver=libsqliteodbc.so
Setup=libsqliteodbc.so
UsageCount=1

[SQLite3]
Description=SQLite3 ODBC Driver
Driver=libsqlite3odbc.so
Setup=libsqlite3odbc.so
UsageCount=1
```
## User db file
db location defined via `nano ~/.odbc.ini`
It basically defines a friendly name of this database, a description, Which ODBC driver to use and a database URI (in this case path).
```
[sitara_bmr]
Description=ELZ Voltages
Driver=SQLite
Database=/home/ukhan/Binaries/bmr_2012.db
# optional lock timeout in milliseconds
Timeout=2000
```
Now we are ready to connect to our database from Base. Check screenshot attached
## Resolving Bug
In case after pressing *test conection* it gave error that it cannot find `.so` file then `sudo nano /etc/odbcinst.ini`
```
[SQLite]
Description=SQLite ODBC Driver
Driver=/usr/lib/aarch64-linux-gnu/odbc/libsqlite3odbc-0.9998.so
Setup=/usr/lib/aarch64-linux-gnu/odbc/libsqlite3odbc-0.9998.so
UsageCount=1

[SQLite3]
Description=SQLite3 ODBC Driver
Driver=/usr/lib/aarch64-linux-gnu/odbc/libsqlite3odbc-0.9998.so
Setup=/usr/lib/aarch64-linux-gnu/odbc/libsqlite3odbc-0.9998.so
UsageCount=1
```
To get proper location of `.so` file use `dpkg -L libsqliteodbc`
