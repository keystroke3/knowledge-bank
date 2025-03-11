# Postgress Problems

- FATAL: Peer authentication failed for user "postgres"
    - check that the database host is valid
    - create a new superuser other than postgres:
        - log into the postgres shell and connect to postgres db:
        ```bash 
        sudo -u postgres psql postgres
        ```
        - create user
        ```postgress
        CREATE ROLE user_name SUPERUSER LOGIN PASSWORD 'password';
        ```
    - map a regular user to your system user account in `pg_ident.conf`
        ```
        user1   system-username postgres-username
        ```
- Postgress is not listening
    - ensure postgress  is running:
    ```bash
    sudo systemctl status postgresql
    ```
    - ensure 5432 is open:
    - add `host all all 0.0.0.0/0 md5` to `pg_hba.conf`
    - change `listening_address` to `'*'`

- permission denied for schema public
    - If the database is empty, you can delete it and create it again making a user the owner:
    ```postgres
    DROP DATABASE <db_name>;
    CREATE DATABASE <db_name> OWNER <user_name>;
    ```
    - If the db is populated and cant be dropped, you can grant permission on public to the user for the database in question:
    ```bash
    sudo -u postgres psql -c 'GRANT all ON SCHEMA public TO <username>' <dbname>
    ```
