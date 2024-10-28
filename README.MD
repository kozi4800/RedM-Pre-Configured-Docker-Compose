
# RedM Server with MariaDB and phpMyAdmin - Docker Compose Setup

This repository provides a Docker Compose configuration for setting up a RedM server using FiveM, along with a MariaDB database and phpMyAdmin for database management. The setup is designed to be easily configurable and includes volume mappings to persist server and database data.

## Prerequisites

Before using this setup, ensure you have the following installed:
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Getting Started

1. **Clone the Repository**

   ```bash
   git clone https://github.com/yourusername/redm-docker-compose.git
   cd redm-docker-compose
   ```

2. **Configure Environment Variables**

   Update the environment variables in the `docker-compose.yml` file with your preferred settings:
   - Replace `your_root_password`, `your_database`, `your_username`, and `your_password` with actual values.
   - Optionally, uncomment and customize the artifact version by modifying the following line:
     ```yaml
     # image: spritsail/fivem:10230  # Uncomment this line and specify the artifact version of your choice
     ```

3. **Start the Docker Compose Services**

   Run the following command to start the RedM server, MariaDB, and phpMyAdmin:
   ```bash
   docker-compose up -d
   ```

   The services should now be running:
   - RedM server available on ports `40120` and `30120`.
   - MariaDB available on port `3306`.
   - phpMyAdmin accessible at `http://localhost:8080`.

## Stopping the Services

To stop the services, use:
```bash
docker-compose down
```

This will stop and remove the containers, but your data will be preserved in the mounted volumes.

## Configuration Details

### RedM Service
- Uses the FiveM Docker image to run the RedM server.
- Ports `40120` and `30120` (TCP and UDP) are exposed for server communication.
- The configuration and server data are persisted via mounted volumes.

### MariaDB Service
- Uses the official MariaDB image.
- Configurable environment variables for root password, database name, and user credentials.
- The database data is persisted via a volume mapping to `./mysql`.

### phpMyAdmin Service
- Provides a web-based interface to manage the MariaDB database.
- Accessible at `http://localhost:8080`.

## Notes
- Make sure to secure your environment variables, especially database credentials.
- You may need to adjust firewall rules to allow external access to the server.

## Troubleshooting

- **Database Connection Issues:** Ensure the environment variables for `MYSQL_USER` and `MYSQL_PASSWORD` are correctly set.
- **RedM Server Not Starting:** Verify that the ports `40120` and `30120` are not already in use by another application.

## Contributing

Feel free to submit issues, suggestions, or pull requests to improve this setup.
