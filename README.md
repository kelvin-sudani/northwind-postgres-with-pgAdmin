# PostgreSQL and pgAdmin Setup with Docker Compose

This README guide provides instructions on how to set up a PostgreSQL database and pgAdmin, a web-based database management tool, using Docker Compose. It includes steps for automatic configuration of database connections in pgAdmin.

## Prerequisites

- Docker and Docker Compose installed on your machine.
- Basic understanding of Docker, PostgreSQL, and pgAdmin.

## Files Overview

- `docker-compose.yml`: Docker Compose configuration file.
- `init.sql`: SQL script to initialize and populate the database.
- `pgadmin_servers.json`: JSON file to pre-configure pgAdmin with database connections.

## Step-by-Step Instructions

### 1. Create the `init.sql` Script

Create an `init.sql` file in your project directory. This script should contain SQL commands to create and populate your database tables.

Example `init.sql`:

```sql
CREATE TABLE example_table (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50),
  age INT
);

INSERT INTO example_table (name, age) VALUES ('John Doe', 30);
```

### 2. Create the pgAdmin Servers Configuration

Create a `pgadmin_servers.json` file to define the connection details for your PostgreSQL database.

Example `pgadmin_servers.json`:

```json
{
  "Servers": {
    "1": {
      "Name": "My PostgreSQL Server",
      "Group": "Servers",
      "Host": "db",
      "Port": 5432,
      "MaintenanceDB": "postgres",
      "Username": "user",
      "Password": "password",
      "SSLMode": "prefer"
    }
  }
}
```

### 3. Set Up Your Docker Compose File

Create a `docker-compose.yml` file with configuration for PostgreSQL and pgAdmin services.

Example `docker-compose.yml`:

```yaml
version: '3.8'
services:
  db:
    image: postgres:latest
    environment:
      POSTGRES_DB: mydatabase
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    ports:
      - '5432:5432'

  pgadmin:
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@admin.com
      PGADMIN_DEFAULT_PASSWORD: root
    volumes:
      - ./pgadmin_servers.json:/pgadmin4/servers.json
    ports:
      - '5050:80'
```

### 4. Run Docker Compose

Start your Docker Compose services:

```bash
docker-compose up -d
```

### 5. Access pgAdmin

Open your web browser and navigate to `http://localhost:5050`. Log in with the pgAdmin credentials defined in your `docker-compose.yml` file.

### 6. Verify Database Setup

Your PostgreSQL server should be pre-configured in pgAdmin. You can browse your database, tables, and run SQL queries through the pgAdmin interface.

## Conclusion

This setup provides a convenient way to manage your PostgreSQL databases with pgAdmin through Docker Compose, with automatic connection configuration for ease of use.

---

Feel free to adjust this README template to better fit your specific project requirements or preferences.
