# wordpress-docker
A simple and instant WordPress development environment using Docker and MySQL.

## Spin up an EC2 instance using Amazon Linux 2023

```
sudo dnf update -y
sudo dnf install -y docker curl
sudo systemctl enable --now docker
sudo usermod -aG docker ec2-user
newgrp docker

docker version
docker run --rm hello-world
```

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

## Use the following template as a reference for the Docker Compose file, save as `docker-compose.yaml`

```
services:
  db:
    image: mysql:8.0
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
      - "8080:80"
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
