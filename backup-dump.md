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



