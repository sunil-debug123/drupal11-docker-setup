# Dockerized Drupal 11 Template

This repository provides a complete setup for running a Drupal 11 project with Docker. It includes:
- PHP 8.3 with Apache
- MySQL 8.0
- phpMyAdmin for database management
- OPcache for performance improvements

## Folder Structure

```
├── db_data/                 # MySQL database storage (persistent data)
├── docker/
│   ├── apache/
│   │   ├── default.conf    # Apache VirtualHost configuration
│   │   ├── ports.conf      # Apache ports configuration
│   ├── php/
│       ├── Dockerfile      # PHP 8.3 + Apache Dockerfile
│       ├── php.ini         # PHP configuration file
├── drupal/                  # Drupal project folder (codebase)
├── .env                     # Environment variables
├── .gitignore               # Ignored files and folders for Git
├── docker-compose.yml       # Docker Compose configuration
└── README.md                # Project documentation
```

---

## Prerequisites

- Docker and Docker Compose installed on your system.
- A Drupal codebase placed in the `drupal/` directory. Ensure it includes the `web/` folder.

---

## Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/sunil-debug123/drupal11-docker-setup
cd drupal11-docker-setup
```

### 2. Configure Environment Variables
Create a `.env` file in the root directory and add the following variables:
```env
MYSQL_ROOT_PASSWORD=your_root_password
MYSQL_DATABASE=drupal
MYSQL_USER=drupal_user
MYSQL_PASSWORD=your_password
```

### 3. Install Composer Dependencies
Navigate to the `drupal/` directory and install required dependencies:
```bash
cd drupal
docker compose exec drupal composer install 
```

### 4. Build and Start Containers
Run the following commands:
```bash
docker compose build --no-cache
docker compose up -d
```

### 5. Access the Application
- **Drupal Site:** [http://localhost:8080](http://localhost:8080)
- **phpMyAdmin:** [http://localhost:8081](http://localhost:8081)
  - Username: `drupal_user`
  - Password: `your_password`

---

## Configuration Details

### Apache
- **DocumentRoot:** `/var/www/html/web`
- **Configuration Files:** Located in `docker/apache/`
  - `default.conf`: VirtualHost setup
  - `ports.conf`: Port configuration

### PHP
- PHP 8.3 with OPcache enabled.
- Custom configuration in `docker/php/php.ini`:
  ```ini
  zend_extension=opcache.so
  opcache.enable=1
  opcache.memory_consumption=128
  opcache.interned_strings_buffer=8
  opcache.max_accelerated_files=10000
  opcache.revalidate_freq=2
  opcache.fast_shutdown=1
  opcache.enable_cli=1
  ```

### MySQL
- MySQL 8.0 with persistent data stored in `db_data/`.

### phpMyAdmin
- Available at [http://localhost:8081](http://localhost:8081).

---

## Commands

### Start the Environment
```bash
docker compose up -d
```

### Stop the Environment
```bash
docker compose down
```

### Rebuild the Containers
If you update the Dockerfiles or configuration files:
```bash
docker compose build --no-cache
```

### View Logs
- Apache Logs:
  ```bash
  docker compose exec drupal tail -f /var/log/apache2/error.log
  ```
- MySQL Logs:
  ```bash
  docker compose logs db
  ```

### Access the Drupal Container
```bash
docker compose exec drupal bash
```

---

## Troubleshooting

### "Not Found" Error
- Ensure the `web/` folder exists inside the `drupal/` directory.
- Verify the `DocumentRoot` in `default.conf` matches `/var/www/html/web`.
- Restart the containers after making changes:
  ```bash
  docker compose restart drupal
  ```

### OPcache Not Enabled
- Ensure OPcache is installed in the `docker/php/Dockerfile`:
  ```dockerfile
  RUN docker-php-ext-install opcache
  ```
- Verify OPcache settings in `php.ini`.

### phpMyAdmin Platform Issue
If you're on an `arm64` architecture, ensure the platform is explicitly set:
```yaml
platform: linux/amd64
```

---

## Customization

### Add Additional PHP Extensions
Edit the `Dockerfile` in `docker/php/` and add required extensions:
```dockerfile
RUN docker-php-ext-install extension_name
```

### Modify Apache Configuration
Edit files in `docker/apache/`, then rebuild and restart the containers:
```bash
docker compose build --no-cache && docker compose up -d
```

---

## Contributing
Feel free to submit issues or create pull requests for improvements.

---

## License
This project is open-source and available under the [MIT License](LICENSE).

