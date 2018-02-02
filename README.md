# Docker-Postgres-And-Java

- Create Docker Compose file anywere:

      touch postgres.yml

- Paste the following text to this file.

```yml
version: '3'
    services:
        db:
            container_name: projectname-postgres
            image: postgres:latest
            environment:
                POSTGRES_DB: projectnameDB
                POSTGRES_USER: projectnameUser
                POSTGRES_PASSWORD: MDNmODNmMDg5MjliMjMxZjdkOGI5ZTZhMGQxZGQ0
                LC_ALL: C.UTF-8
        pgadmin:
            container_name: projectname-pgadmin
            image: thajeztah/pgadmin4:latest
            ports:
                - "5050:5050"
            links:
                - db
```

- Create and start containers:

      docker-compose -f /path/to/file/postgres.yml up -d
    
- Check the running containers:

      docker ps | grep projectname # Each our container starts with "projectname"
      
- Get the IP address of running containers:

  * Find the IP address of the Postgres **projectname-postgres**:

        docker inspect projectname-postgres | grep IPAddress

    Now you can use this IP in the DB connection. For example:

        db.jdbcurl=jdbc:postgresql://172.53.0.3:5432/projectnameDB
        db.username=projectnameUser
        db.password=MDNmODNmMDg5MjliMjMxZjdkOGI5ZTZhMGQxZGQ0
        db.host=172.53.0.3

    All DB credentials provided in the environment. See **postgres.yml**.

  * Find the IP address of the PgAdmin **projectname-pgadmin**:

        docker inspect projectname-pgadmin | grep IPAddress

    You can use this IP address with port _5050_ or _"any IPv4 address at all"_ - 0.0.0.0:5050.<br>
    Now open this address in any browser and create a connection. You should use as the DB address **db**. For example:

        Host name/address: db
        Port: 5432
        Maintenance database: projectnameDB
        Username: projectnameUser
        Password: MDNmODNmMDg5MjliMjMxZjdkOGI5ZTZhMGQxZGQ0

 - If you need to access to the Postgres container, follow this step:

        docker exec -u postgres -it projectname-postgres bash
        
 - If you need to import DB dump, follow this step:

        docker exec -u postgres -i projectname-postgres psql projectnameDB < /path/to/your/local/dump.sql
