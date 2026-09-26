# wordpress-docker
A simple and instant WordPress development environment using Docker and MySQL.


```
sudo dnf update -y
sudo dnf install -y docker curl
sudo systemctl enable --now docker
sudo usermod -aG docker ec2-user
newgrp docker

docker version
docker run --rm hello-world
```
