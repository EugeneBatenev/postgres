# Install & set up postgres

## Install

```bash
sudo apt update
sudo apt full-upgrade
sudo apt install postgresql
```

## Run as postgres user. Can create a new user as well here

Switch to postgres user with appropriate role to manage the database server.

`sudo su postgres`

Create another user with the rights to manage the db.

`createuser pi -P --interactive`

This enters to the postgres management

`psql`

Now, you can manage the databases.

try out:

`CREATE DATABASE dtabasename;`

## Update config to allow connections

Open the file `pg_hba.conf`

For example from here if Postgres 12 is installed: `/etc/postgresql/12/main`

`sudo nano pg_hba.conf` 

change this line at the bottom of the file, it should be the first line of the settings:

From default 

```bash
local   all             postgres                                peer
```

to

```bash
local   all             all                                md5
```
This will allow you connecting with all users locally. Then the server is to be restarted.

```bash
sudo service postgresql restart
```

Login into psql and set password for postgres user

```bash
psql -U postgres
```
After you've logged in you need to update the `postgres` user as follows:
 
```SQL
db> ALTER USER postgres with password 'your-pass';
```
The other users (if any) will need a password too.

For Postgres server to accept the connections from other machines file is to be updated as follows: 

```bash
# IPv4 local connections:
host    all             all             samehost                md5
host    database1          all             samenet              md5
host    database2             all          192.168.1.22/24      md5
```
Line 1 will allow connections from the **same host** to **all databases** with username and password.
Line 2 will allow connections to the **database1** from the **same network** as Postgres server with username and password.
Line 3 will allow connections to the **database2** from the **host 192.168.1.22** with username and password.

## Authentication methods

- `trust` - anyone who can connect to the server is authorized to access the database
- `peer` - use client's operating system user name as database user name to access it.
- `md5` - password-based authentication