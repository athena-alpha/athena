# Installation

## Table of Contents
- [Quick Start - Docker Compose](#quick-start---docker-compose)
- [Quick Start - NAS & Other Platforms](#quick-start---nas--other-platforms)
- [Database, Config & Secrets Files](#database-config--secrets-files)
- [Application Settings](#application-settings)
- [Updating](#updating)
- [Uninstallation](#uninstallation)
- [Run In The Background](#run-in-the-background)
- [Run Automatically On Boot](#run-automatically-on-boot)
- [Generating Your Own Self-Signed SSL Certificate](#generating-your-own-self-signed-ssl-certificate)
- [Changing Ports](#changing-ports)
- [Direct Database Access](#direct-database-access)

## Quick Start - Docker Compose
The below commands will download and build the entire project from scratch, locally on your system, in around 60 seconds using the pre-built Docker images. This is the fasteset, easiest way to build Athena without having to install Node.js, PHP... etc all on your computer or server

![okay-lets-ride](images/installation/okay-lets-ride.gif)

- First, install [Docker and Docker Compose](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
- Next clone the repo, build and run [docker-compose.yml](../docker-compose.yml) with the commands below
```bash
git clone https://github.com/athena-alpha/athena.git && cd athena && docker compose up --build
```
> **Note:** The first user to be created will automatically be granted Admin privileges
- Finally open: https://localhost 🎉

## Quick Start - NAS & Other Platforms
- For other platforms like Portainer, TrueNAS SCALE, Dockge or NAS based apps such as Container Station (QNAP), Container (Synology) or Docker Center (ASUSTOR) you can simply copy/paste the [docker-compose.yml](../docker-compose.yml) contents into your platform of choice
> **Note:** The first user to be created will automatically be granted Admin privileges
- Finally open: https://localhost 🎉

## Database, Config & Secrets Files
A single `data` volume is required to persist user data across restarts and upgrades:
- `data/config`: Stores files used to configure the system on first startup (`.env`, `db/init/01-common.sql`, `nginx/nginx.conf`, `nginx/fastcgi_params`, `nginx/certs`).
- `data/db`: Stores the main database files.
- `data/secrets`: Stores sensitive files that are randomly generated on first startup (`mariadb_password`, `master_encryption_key`, `jwt_secret`).
- `data/tmp`: Stores the temporary database files.

## Application Settings
Application settings are viewable via the `Settings -> Admin Panel` area and include:
| Setting | Description |
|---------|-------------|
| `allow_sign_up` | Enables or disables new user sign-ups |
| `site_name` | Name used in emails and other areas |
| `default_theme` | The default theme applied to new users |
| `default_primary_color` | The default primary color applied to new users |
| `default_language` | The default language applied to new users |
| `default_time_zone` | The default time zone applied to new users |
| `default_units` | The default unit system applied to new users |
| `default_currency` | The default currency applied to new users |
| `default_fiscal_year` | The default fiscal year applied to new users |
| `max_sign_in_attempts` | Maximum sign-in attempts before lockout |
| `lockout_duration_seconds` | Duration of lockout after max attempts |
| `min_username_length` | Minimum length that usernames can be |
| `max_username_length` | Minimum length that usernames can be |
| `min_password_length` | Minimum length that passwords can be |
| `max_password_length` | Maximum length that passwords can be |
| `audit_log_retention_days` | Number of days after which audit logs are deleted |
| `system_log_retention_days` | Number of days after which system logs are deleted |
| `api_rate_limit_public_minutes` | The rate limit for public API endpoints in minutes per IP |
| `api_rate_limit_authenticated_minutes` | The rate limit for authenticated API endpoints in minutes per user |

## Updating
- Backup your data (**Seriously, always backup your data!!!**) by going to the `Settings -> Admin Panel -> Application backups` area
- Stop the Athena services
```bash
docker compose down
```
- Ensure your `data` directory is properly backed up
- Update the `athena-client` and `athena-server` Docker images
```bash
docker compose pull
```
- Start the Athena services
```bash
docker compose up -d
```

## Uninstallation
- To uninstall first shut down or stop any services from running
```bash
docker compose down # Shut down the docker services
sudo systemctl stop athena.service # Stop the service
rm /etc/systemd/system/athena.service # Delete the service file
```
- Then simply delete the folder you cloned the repo to

## Run In The Background
```bash
docker compose up -d # To start
docker compose down # To stop
```

## Run Automatically On Boot
- Create a new Systemd service file
```bash
sudo nano /etc/systemd/system/athena.service
```
- Add the following content, making sure to update the `/path/to/my-app/` part
```ini
[Unit]
Description=Athena
Requires=docker.service
After=docker.service

[Service]
Type=oneshot
RemainAfterExit=yes
WorkingDirectory=/path/to/my-app # Replace with the path to your cloned repository
ExecStart=/usr/bin/docker-compose up -d
ExecStop=/usr/bin/docker-compose down
TimeoutStartSec=0

[Install]
WantedBy=multi-user.target
```
- Save the file (`Ctrl+O`, `Enter`, `Ctrl+X` in nano)
- Set permissions for the file
```bash
sudo chmod 644 /etc/systemd/system/athena.service
```
- Reload Systemd, enable and then start the service
```bash
sudo systemctl daemon-reload
sudo systemctl enable athena.service
sudo systemctl start athena.service
```
- See the status of the service or stop it
```bash
sudo systemctl status athena.service
sudo systemctl stop athena.service
```

## Generating Your Own Self-Signed SSL Certificate
Connections use TLS 1.3 and thus, require a locally generated self-signed SSL Certificate. One is randomly generated for you when Athena is first run, however you're free to generate your own personal ones too
```bash
sudo apt-get update # Update
sudo apt install openssl # Install OpenSSL

# Generates your self-signed certificate
# Valid for 10 years, for the "localhost" domain (CN)
# Files created: server.key and server.crt
CERTS_DIR=/data/config/nginx/certs
openssl req -x509 -nodes -days 3650 -newkey rsa:4096 \
  -keyout "$CERTS_DIR/server.key" \
  -out "$CERTS_DIR/server.crt" \
  -subj "/CN=localhost" \
  -addext "subjectAltName=DNS:localhost"
```

## Changing Ports
If you'd like to change the default `443` port (HTTPS) that the Client interface is served on, simply update the [docker-compose.yml](../docker-compose.yml) file.
- Undere the `Nginx -> Ports` area, change the first `443` number to whatever valid port you wish
```yaml
nginx:
    image: nginx:latest
    restart: always
    ports:
      - "443:443" # This maps external port 443 (left) to the internal 443 port (right)
```

## Direct Database Access
If you'd like to directly access the database to view, import, export and alter the raw data:

1. Copy the below service for [phpMyAdmin](https://www.phpmyadmin.net/) into the provided [docker-compose.yml](../docker-compose.yml) file
```yaml
phpmyadmin: # Web interface for direct database access
  image: phpmyadmin:latest
  container_name: athena-phpmyadmin
  restart: unless-stopped
  ports:
    - "8080:80"
  depends_on:
    - db
```

2. Restart the docker compose stack and browse to [http://localhost:8080](http://localhost:8080) (note there is NO `https`)
3. The username is `athena` and the password is shown in the Server boot up logs as it's randomly generated for each deployment. If you cannot find the password in the logs, they are stored in `/data/secrets/<file>`. It should look like:
```
"Generated MARIADB_PASSWORD: 123..."
```
