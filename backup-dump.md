# Making a postgresql database dump from a remote or local machine

## Add the postgresql repo information to your remote machine (relatively to the database)

In reality in this case the remote machine will be the database, justo you to know (or for me to remember later).

### Install postgresql client software to 'remote' machine

The version should be the same as your database engine.

```bash
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update

# Only if you need full Postgresql installation
#sudo apt-get -y install postgresql

#For remote backup we need client's command line utils
sudo apt-get install postgresql-client-12
```

[Check the source just in case](https://www.postgresql.org/download/linux/ubuntu/)

## Making the backup (dump)

### pg_dump tool

What do we need:

- database server's IP address `${IP_ADDR}`
- database application port `${IP_PORT}`
- database username `${DB_USERNAME}`
- database name `${DB_NAME}`

```bash
pg_dump -h ${IP_ADDR} -p ${IP_PORT} -Fc -U ${DB_USERNAME} ${DB_NAME} -f report_db.sql
```

`-Fc` - this key is to create custom format. Output a custom-format archive suitable for input into pg_restore. [see here](https://www.postgresql.org/docs/12/app-pgdump.**html**) 
