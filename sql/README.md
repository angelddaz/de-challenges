Assuming you are using an Ubuntu Shell, as specified in this repos README, you can bring up a postgres CLI to attempt all of these challenges.

The pain of copying and pasting into a terminal is worth the experience gained with CLI familiarity, on DDL (building tables from scratch), and bash.

## Making sure your Postgres Server is running
Before attempting to bring up your postgres CLI, make sure your server is running with the following commands:

`$ sudo service postgresql status`

And if needed,

`$ sudo service postgresql restart`

## logging into your postgres server as user `postgres`
The following command will bring up the postgres server connection.

`$ sudo -u postgres psql`

From here, you should be able to Google any DDL or DML needed to create test datasets, tables, schemas for all challenges.

## Helpful commands
`\q` exit
`\d` describe
`\dt+` describe tables and be verbose
