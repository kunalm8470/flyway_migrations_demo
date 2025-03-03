# flyway_migrations_demo
Demo project to try out flyway migrations demo

There is a GitHub workflow which will migrate the database.

Pre-requisites before running the GitHub actions workflow is to add the below secrets.

- `DATABASE_HOST`
- `DATABASE_NAME`
- `DATABASE_SCHEMA`
- `DATABASE_USERNAME`
- `DATABASE_PASSWORD`

The workflow will do the below tasks -

- Copy the source code to the GitHub actions runner.
- Download the Flyway [Command Line compressed utility](https://documentation.red-gate.com/flyway/reference/usage/command-line) for Linux OS from this [link](https://download.red-gate.com/maven/release/com/redgate/flyway/flyway-commandline/11.3.4/flyway-commandline-11.3.4-linux-x64.tar.gz).
- Decompress the Flyway compressed command line utility and move the contents of it into the GitHub actions runner root.
- Copy the migration scripts into the `flyway/migrations` directory.
- And run the below command
`flyway migrate -url="jdbc:postgresql://<DATABASE_HOST>/<DATABASE_NAME>?currentSchema=<DATABASE_SCHEMA>" -user=<DATABASE_USERNAME> -password=<DATABASE_PASSWORD> -locations="migrations"`
- Once the above command is ran, flyway will create and maintain a separate table called `flyway_schema_history` which will have the order of the migrations run.

To create a new migration we need to write it in -
`V1__<Description_of_the_migration>.sql`

To undo a migration we need to write it in -
`U1__<Description_of_the_migration>.sql`

Note: The version numbers in creating and undoing migrations should be incrementing examples like:

E.g: `V1 > V2 > V3`, `V0.1 > V0.2 > V0.3`
and
`U1 > U2 > U3`, `U0.1 > U0.2 > U0.3`
