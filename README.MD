
# RedM Server with MariaDB and phpMyAdmin - Docker Compose Setup

This repository provides a Docker Compose configuration for setting up a RedM server on linux, along with a MariaDB database and phpMyAdmin for database management. The setup is designed to be easily configurable and includes volume mappings to persist server and database data.

## Docker Image

This setup is based on the [Spritsail Docker image for FiveM](https://github.com/spritsail/fivem), which provides a containerized environment for running a FiveM server.

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

   Here's an example of how to configure the `docker-compose.yml` file:

   ```yaml
   version: "3.9"

   services:
       redm:
           image: spritsail/fivem:latest
           container_name: redm
           environment:
           - NO_LICENSE_KEY=1
           - NO_DEFAULT_CONFIG=1
           - PUID=1000  # Set your user ID (optional)
           - PGID=1000  # Set your group ID (optional)
           - LICENSE_KEY=your_license_key_here  # Replace with your actual license key
           volumes:
           - ./config:/config
           - ./txData:/txData
           - ./server.cfg:/config/server.cfg
           ports:
           - "30120:30120"
           - "30120:30120/udp"
           restart: always

       redm_db:
           image: mariadb:10.4.32
           container_name: redm_db
           environment:
           - MYSQL_ROOT_PASSWORD=your_root_password
           - TZ=Europe/Paris
           - MYSQL_DATABASE=redm_db
           - MYSQL_USER=redm_user
           - MYSQL_PASSWORD=your_password
           ports:
           - "3306:3306"
           volumes:
           - ./mysql:/var/lib/mysql
           restart: always

       phpmyadmin:
           image: phpmyadmin
           restart: always
           ports:
           - "8080:80"
           environment:
           - PMA_ARBITRARY=1
           - UPLOAD_LIMIT=100M
           depends_on:
           - redm_db
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
