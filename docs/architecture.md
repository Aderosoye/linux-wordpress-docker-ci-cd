# Architecture Notes

## Logical Flow
```text
Internet User
   |
   v
[ Linux Host ]
   |
   +--> Nginx container listens on port 80
            |
            v
      WordPress PHP-FPM container
            |
            v
      MariaDB container
```

## Components

### Linux Host
The host runs Docker Engine and Docker Compose, stores deployment files, accepts SSH connections for remote deployment, and exposes the web service.

### Nginx Container
Nginx acts as the frontend entry point. It serves static assets and forwards PHP requests to the WordPress PHP-FPM container.

### WordPress Container
The WordPress container contains the application runtime and uses environment variables to connect to MariaDB.

### MariaDB Container
MariaDB stores the WordPress database and persists state through a dedicated Docker volume.

## Networking Model
All services share a Docker bridge network named `wpnet`. Internal service resolution is handled by container names, so Nginx can forward PHP requests to `wordpress`, and WordPress can connect to `db`.

## Data Persistence
Two volumes are used:
- `db_data` for MariaDB
- `wp_data` for WordPress application data

## Operational Design Choices
- **Compose over Kubernetes**: right-sized for the scope
- **Nginx + PHP-FPM**: cleaner separation than using a single all-in-one container
- **SSH + rsync deployment**: simple, readable, and effective for a single-host pattern
- **Validation before deploy**: prevents avoidable failures from reaching the server
