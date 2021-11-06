# Install & set up postgres

## Install

```bash
sudo apt update
sudo apt full-upgrade
sudo apt install postgresql
```

## Run as postgres user. Can create a new user as well here

`sudo su postgres`

`createuser pi -P --interactive`


This enters to the postgres management

`psql`

