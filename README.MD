# Install & set up Postgres database server

## Install

```bash
sudo apt update
sudo apt full-upgrade
sudo apt install postgresql
```

### Alternative installation from Postgresql repos

```bash
wget -q -O- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor --output /usr/share/keyrings/pgdg.gpg
echo "deb [signed-by=/usr/share/keyrings/pgdg.gpg] http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
sudo apt update
sudo apt install -y postgresql-15
```

[Source](https://www.postgresql.org/download/linux/ubuntu/)

## Check what are the ports used by postgresql

```shell
sudo netstat -naptu | grep 5432
# or
sudo netstat -naptu | grep postgres
# or
sudo ss -atnp | grep 5432
# or
sudo ss -atp | grep postgres
```

## Check what are the processes running for postgres

```shell
ps -aux | grep postgres
# or
top -u postgres
```

### Postgresql services status

```shell
sudo service postgresql status
# or
sudo systemctl status postgresql
```

## Manage status of postgresql processes

```shell
# check status
sudo systemctl status postgresql
# stop the process
sudo systemctl stop postgresql
# start the process
sudo systemctl start postgresql
# restart the process
sudo systemctl restart postgresql
# reload the process whatever this means
sudo systemctl reload postgresql
```

## Run as `postgres` user. Can create a new user as well here

Switch to `postgres` user with appropriate role to manage the database server.

`sudo su postgres`

Create another user with the rights to manage the db if you need.

`createuser pi -P --interactive`

This enters to the Postgres' management

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
host    all             all             0.0.0.0/0            md5
```
Line 1 will allow connections from the **same host** to **all databases** using username and password.
Line 2 will allow connections to the **database1** from the **same network** as Postgres server using username and password.
Line 3 will allow connections to the **database2** from the **host 192.168.1.22** using username and password.
Line 4 will allow connections to **all** databases **from any** host (0.0.0.0/0) using username and password.

## Authentication methods

- `trust` - anyone who can connect to the server is authorized to access the database
- `peer` - use client's operating system user name as database user name to access it.
- `md5` - password-based authentication

## Applying the settings

Postgres should be restarted for the settings in `pg_hba.conf`.

```bash
sudo service postgresql restart
```

## Updating `postgresql.conf`

Add as last line:

```bash
listen_addresses = '*'
```

## Official page

https://www.postgresql.org/docs/9.1/auth-methods.html


