
# 🚀 RedM Server with MariaDB and phpMyAdmin - Docker Compose Setup

This repository provides a Docker Compose configuration for setting up a pre-configured RedM server on Linux using the Spritsail FiveM Docker image, along with a MariaDB database and phpMyAdmin for database management. 🛠️ The setup is designed to be easily configurable and includes volume mappings to persist server and database data. 🗃️

## 📋 Description

This Docker Compose file allows for the easy installation and deployment of a pre-configured RedM server. With this setup, you can quickly set up a complete environment, including script management, mods, and specific configurations, providing a ready-to-use game server. 🎮

### 🎯 Objectives:
- **RedM Server**: Simplifies the installation and startup of a RedM server with a pre-configured setup, allowing administrators to focus on adding content and adjusting server settings instead of building everything from scratch.
- **MariaDB**: Provides a database to store server data such as player information, statistics, and persistent configurations. 📊
- **phpMyAdmin**: Offers a graphical interface for easily managing the MariaDB database via a web browser, allowing for the creation, modification, and administration of server data. 🌐

### 💡 Advantages of this Docker Compose Setup:
- **Automation**: Deploys all three services (RedM, MariaDB, phpMyAdmin) with a single command, making server installation and maintenance more accessible. 🔧
- **Centralized Configuration**: Simplifies parameter management and data volumes through a single `docker-compose.yml` file, providing a turnkey solution. 📝
- **Data Persistence**: Mounted volumes ensure that server configurations and database data are preserved even if the containers are stopped or restarted. 🔄
- **Flexibility**: Supports custom configurations, including using a specific `server.cfg` file for RedM server settings, and allows the use of a license key for advanced features. 🔑

## 🚀 Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/redm-docker-compose.git
   cd redm-docker-compose
   ```

2. **Configure the `docker-compose.yml` file:**
   - Update the environment variables for MariaDB and add your RedM license key.

3. **Start the services:**
   Use the following command to start the RedM server, MariaDB, and phpMyAdmin:
   ```bash
   docker-compose up -d
   ```

   The services should now be running:
   - 🌐 RedM server available on ports `40120` and `30120`.
   - 🗄️ MariaDB available on port `3306`.
   - 💻 phpMyAdmin accessible at `http://localhost:8080`.

## 🛑 Stopping the Services

To stop the services, use:
```bash
docker-compose down
```
This will stop and remove the containers, but your data will be preserved in the mounted volumes. 📦

## ⚙️ Configuration Details

### 🎮 RedM Service
- Uses the FiveM Docker image from [Spritsail](https://github.com/spritsail/fivem) to run the RedM server.
- Ports `40120` and `30120` (TCP and UDP) are exposed for server communication.
- Configuration, server data, and `server.cfg` are persisted via mounted volumes.
- Includes an optional `LICENSE_KEY` environment variable that can be set.

### 🗃️ MariaDB Service
- Uses the official MariaDB image.
- Configurable environment variables for root password, database name, and user credentials.
- The database data is persisted via a volume mapping to `./mysql`.

### 🌐 phpMyAdmin Service
- Provides a web-based interface to manage the MariaDB database.
- Accessible at `http://localhost:8080`.

## 📝 Notes
- Make sure to secure your environment variables, especially database credentials and license keys. 🔒
- You may need to adjust firewall rules to allow external access to the server. 🔧

## 🔧 Troubleshooting

- **Database Connection Issues:** Ensure the environment variables for `MYSQL_USER` and `MYSQL_PASSWORD` are correctly set.
- **RedM Server Not Starting:** Verify that the ports `40120` and `30120` are not already in use by another application.

## 🙏 Acknowledgements

Special thanks to [Spritsail](https://github.com/spritsail) for their work on the FiveM Docker image, which serves as the foundation for this setup. Their contributions are greatly appreciated! 👏

## 🤝 Contributing

Feel free to submit issues, suggestions, or pull requests to improve this setup. 💡
