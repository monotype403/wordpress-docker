# wordpress-docker
A simple and instant WordPress development environment using Docker and MySQL.

## After spinning up an EC2 instance using Amazon Linux 2023, update the server:

```
sudo dnf update -y
```

## Check that you are in the home directory (i.e. `/home/ssm-user` or `/home/ec2-user`):

```
cd ~
pwd
```

## Create a folder named `wordpress`, and go inside it:

```
mkdir wordpress && cd wordpress
```

## Check if `curl` exists in the server

```
which curl
```

## If `curl` does not exist, install it. Otherwise, proceed to the next step:

```
sudo dnf install -y curl
```

## Install Docker and enable it
```
sudo dnf install -y docker
sudo systemctl enable --now docker
```

## 

### If SSM is used for auth:
```
sudo usermod -aG docker ssm-user
```

### If an SSH key is used for auth:
```
sudo usermod -aG docker ec2-user
```

### Apply group permissions and verify the installation
Run the following command to activate the Docker group permissions for your current session:

```bash
newgrp docker
```

Now, verify that Docker runs without sudo:

```bash
docker version
docker run --rm hello-world
```

*Note: You can now proceed to run your `docker compose` commands directly in this terminal session.*


## Install Docker Compose:

```
## Install docker compose
sudo mkdir -p /usr/libexec/docker/cli-plugins/
sudo mkdir -p /usr/libexec/docker/cli-plugins/
sudo curl -SL "https://github.com/docker/compose/releases/latest/download/docker-compose-linux-$(uname -m)" -o /usr/libexec/docker/cli-plugins/docker-compose

sudo chmod +x /usr/libexec/docker/cli-plugins/docker-compose
sudo ln -s /usr/libexec/docker/cli-plugins/docker-compose /usr/local/bin/docker-compose
docker-compose --version
```

## Use the following template as a reference for the Docker Compose file, save as `docker-compose.yaml`:

```
services:
  db:
    image: mysql:8.4
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: SecureRootPassword123*()*&&*
      MYSQL_DATABASE: wordpress_db
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: Strong+WordPressUserPassword987
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    restart: always
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: Strong+WordPressUserPassword987 # Must match MYSQL_PASSWORD above
      WORDPRESS_DB_NAME: wordpress_db
    volumes:
      - wp_data:/var/www/html
    depends_on:
      - db

volumes:
  db_data:
  wp_data:

```

## Bring the Docker container up to serve the website:

```
docker compose up -d
```

## Check if Wordpress is already accessible inside the private network

```
curl -I http://localhost:80/
```
### If the output in the terminal is similar to this, then it is running:

```
HTTP/1.1 302 Found
Date: Sat, 26 Sep 2026 08:14:02 GMT
Server: Apache/2.4.68 (Debian)
X-Powered-By: PHP/8.3.35
Expires: Wed, 11 Jan 1984 05:00:00 GMT
Cache-Control: no-cache, must-revalidate, max-age=0, no-store, private
X-Redirect-By: WordPress
Location: http://localhost/wp-admin/install.php
Content-Type: text/html; charset=UTF-8
```

## Copy the IP address or public DNS of your EC2 instance, and access the following link in your browser:

```
http://[EC2_IP_OR_DNS]/wp-admin/install.php
```
<img width="1089" height="719" alt="Wordpress_Docker_Setup" src="https://github.com/user-attachments/assets/e3f98ccb-705f-4154-9f2f-ec5f4a136895" />

Your WordPress website is now ready to be configured. Finish the configuration immediately, or terminate the EC2 instance if you will not be fully configuring this.
