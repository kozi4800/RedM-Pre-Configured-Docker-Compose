
# RedM Server with MariaDB and phpMyAdmin - Docker Compose Setup

This repository provides a Docker Compose configuration for setting up a RedM server on Linux, along with a MariaDB database and phpMyAdmin for database management. The setup is designed to be easily configurable and includes volume mappings to persist server and database data.

## Docker Image

This setup is based on the [Spritsail Docker image for FiveM](https://github.com/spritsail/fivem), which provides a containerized environment for running a FiveM/RedM server.

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
   - If you have a license key, uncomment the `LICENSE_KEY` environment variable and replace `"license-key-here"` with your actual license key.

3. **Add the `server.cfg` File**

   Make sure to add your `server.cfg` file in the specified path and update the Docker Compose file to reflect its location:
   ```yaml
   - ./path/to/server.cfg:/config/server.cfg
   ```

4. **Example Configuration**

   Here's an example of how to configure the `docker-compose.yml` file with detailed comments:

   ```yaml
   version: "3.9"  # Use Docker Compose version 3.9

   services:
       redm:
           image: spritsail/fivem:latest  # Use the latest FiveM Docker image for RedM
           container_name: redm  # Set the container name to "redm"
           environment:
           - NO_LICENSE_KEY=1  # Disable the need for a license key (set to 0 to enable)
           - NO_DEFAULT_CONFIG=1  # Do not load the default server configuration
           - PUID=1000  # Set the user ID for file permissions (optional)
           - PGID=1000  # Set the group ID for file permissions (optional)
           - LICENSE_KEY=your_license_key_here  # Replace with your actual server license key
           volumes:
           - ./config:/config  # Mount the "config" directory for server configuration
           - ./txData:/txData  # Mount the "txData" directory for server data
           - ./server.cfg:/config/server.cfg  # Mount the "server.cfg" file for server settings
           ports:
           - "30120:30120"  # Map the TCP port for the server
           - "30120:30120/udp"  # Map the UDP port for the server
           restart: always  # Restart the container automatically if it stops

       redm_db:
           image: mariadb:10.4.32  # Use a specific version of the MariaDB image
           container_name: redm_db  # Name the container "redm_db"
           environment:
           - MYSQL_ROOT_PASSWORD=your_root_password  # Set the root password for MariaDB
           - TZ=Europe/Paris  # Set the timezone for the database
           - MYSQL_DATABASE=redm_db  # Name of the database to create
           - MYSQL_USER=redm_user  # Username for accessing the database
           - MYSQL_PASSWORD=your_password  # Password for the database user
           ports:
           - "3306:3306"  # Expose the default MariaDB port
           volumes:
           - ./mysql:/var/lib/mysql  # Mount the "mysql" directory for database storage
           restart: always  # Restart the container automatically if it stops

       phpmyadmin:
           image: phpmyadmin  # Use the default phpMyAdmin image
           restart: always  # Restart the container automatically if it stops
           ports:
           - "8080:80"  # Map port 8080 to access phpMyAdmin through a web browser
           environment:
           - PMA_ARBITRARY=1  # Allow phpMyAdmin to connect to any database server
           - UPLOAD_LIMIT=100M  # Set the maximum file upload size to 100 MB
           depends_on:
           - redm_db  # Ensure the "redm_db" service is started before this one
   ```

5. **Start the Docker Compose Services**

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
- Uses the FiveM Docker image from [Spritsail](https://github.com/spritsail/fivem) to run the RedM server.
- Ports `40120` and `30120` (TCP and UDP) are exposed for server communication.
- The configuration, server data, and `server.cfg` are persisted via mounted volumes.
- Includes an optional `LICENSE_KEY` environment variable that can be set.

### MariaDB Service
- Uses the official MariaDB image.
- Configurable environment variables for root password, database name, and user credentials.
- The database data is persisted via a volume mapping to `./mysql`.

### phpMyAdmin Service
- Provides a web-based interface to manage the MariaDB database.
- Accessible at `http://localhost:8080`.

## Notes
- Make sure to secure your environment variables, especially database credentials and license keys.
- You may need to adjust firewall rules to allow external access to the server.

## Troubleshooting

- **Database Connection Issues:** Ensure the environment variables for `MYSQL_USER` and `MYSQL_PASSWORD` are correctly set.
- **RedM Server Not Starting:** Verify that the ports `40120` and `30120` are not already in use by another application.

## Acknowledgements

A special thanks to [Spritsail](https://github.com/spritsail) for their work on the FiveM Docker image, which serves as the foundation for this setup. Their contributions to the community are greatly appreciated!

## Contributing

Feel free to submit issues, suggestions, or pull requests to improve this setup.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
