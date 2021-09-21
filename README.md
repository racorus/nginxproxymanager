# nginxproxymanager
how to install it on Ubuntu 20.04 CT
Install Nginx Proxy Manager on Ubuntu CT

sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release -y

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg — dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo “deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable” | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose -y


sudo systemctl enable docker.service
sudo systemctl start docker.service
sudo systemctl enable containerd.service

cd /nginxproxymanager
nano docker-compose.yml

####################################33
version: “3”
services:
app:
image: ‘jc21/nginx-proxy-manager:latest’
restart: unless-stopped
ports:
# Public HTTP Port:
— ‘80:80’
# Public HTTPS Port:
— ‘443:443’
# Admin Web Port:
— ‘81:81’
# Add any other Stream port you want to expose
# — ‘21:21’ # FTP
environment:
# These are the settings to access your db
DB_MYSQL_HOST: “db”
DB_MYSQL_PORT: 3306
DB_MYSQL_USER: “npm”
DB_MYSQL_PASSWORD: “npm”
DB_MYSQL_NAME: “npm”
# If you would rather use Sqlite uncomment this
# and remove all DB_MYSQL_* lines above
# DB_SQLITE_FILE: “/data/database.sqlite”
# Uncomment this if IPv6 is not enabled on your host
# DISABLE_IPV6: ‘true’
volumes:
— ./data:/data
— ./letsencrypt:/etc/letsencrypt
depends_on:
— db
db:
image: ‘jc21/mariadb-aria:latest’
restart: unless-stopped
environment:
MYSQL_ROOT_PASSWORD: ‘npm’
MYSQL_DATABASE: ‘npm’
MYSQL_USER: ‘npm’
MYSQL_PASSWORD: ‘npm’
volumes:
— ./data/mysql:/var/lib/mysql

####################################################

docker-compose up -d

Default Account
Email: admin@example.com
Password: changeme
