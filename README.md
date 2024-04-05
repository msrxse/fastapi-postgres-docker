# About

- Fullstack Python-FastAPI and React application to manage contacts.
- Uses postgreSQL in Docker container

# Creating postgres database in docker container

1. Create docker container (will host database)

- `docker pull postgres:alpine` // download image

- `docker images` // to see what images are installed

- `docker run --name [name-of-container] -e POSTGRES_PASSWORD=[your-password] -d -p [port:port] postgres:alpine` // create the container

- `docker ps` // To see what container you have

2. Create database within container

- `docker exec -it [name-of-container] bash` //now we are in bash mode (-it = interactive mode)

---

- `ba5fb3ed6aab:/# ls` // ls to list files
- `ba5fb3ed6aab:/# psql -U postgres` jump into postgres

---

- `postgres=# create database [database-name];` // now create database (dont miss last semicolon)
- `CREATE DATABASE`
- `postgres=# create user [user] with encrypted password '[password]';` // create user with a password for this database
- `CREATE ROLE`
- `postgres=# grant all privileges on database [database-name] to [user];` // give privileges to this user
- `GRANT`
- `postgres=# \c [database-name]` // now we can jumpt into our database
- `You are now connected to database "fastapi_database" as user "postgres".`

---

- `fastapi_database=#`

---

3. Now database exists inside this container but we want to make it accessible outside

- `fastapi_database-# psql -h localhost -p 5432 postgres`// Now db available outside container and we can jumpt into out fastApi application

## NOTES:

- [name-of-container] = fastapi-postgres
- [database-name] = fastapi_database
- [user] = myuser
- [password] = password
- [port] = 5432

# Python FastAPI application

- `python3 -m venv venv` // Create python environment
- `source venv/bin/activate` // Next activate it
- `(venv) ➜  fastApi-postgres-docker git:(master) ✗` // youll see a hint telling you about your new environment
- `pip3 install "fastapi[all]" SQLAlchemy psycopg2-binary` // make necessary installations
- `pip install -U black` // install black to format code more readible

# Run any new tables (added in services)

- Eg. to run \_add_tables()
- `cd backend`
- `python` // open python interpreter
- `import services` // import the services file
- `services._add_tables()` // run the file
- run `exit()` to exit interpreter

We now added the new tables to the database and to test you can open the running database in container and check tables/rows in table

- `fastapi_database-# \dt` // list the tables
- `List of relations
 Schema |   Name   | Type  | Owner
--------+----------+-------+--------
 public | contacts | table | myuser
(1 row)
`
- `fastapi_database-# select * from contacts;` // list all rows in contacts table
- `id | first_name | last_name | phone_number | date_created
----+------------+-----------+--------------+--------------
(0 rows)
`
- In case you have this error.
  `psycopg2.errors.InsufficientPrivilege: permission denied for table ...`
- For me, this command worked:
- `postgres=# GRANT postgres TO myuser;`

# To start server

- `cd backend`
- `uvicorn main:app --reload`

# To re-access database from the console

- 1.➜  fastapi-postgres-docker `docker exec -it fastapi-postgres bash` // run bash into this container
- 2. b0bdea7e696f:/# ` psql -U postgres` // jump into postgres
- 3. postgres=# `\c fastapi_database` // now jump into database
You are now connected to database "fastapi_database" as user "postgres".
`fastapi_database-#`

# TODOS

- [ ] Add docker-compose