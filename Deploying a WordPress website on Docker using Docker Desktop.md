Deploying a **WordPress website on Docker** using **Docker Desktop** involves setting up **WordPress and MySQL containers** using **Docker Compose**. Below is a step-by-step guide:

-----
**Step 1: Install Docker Desktop**

- Download and install [**Docker Desktop**](https://www.docker.com/products/docker-desktop/) for your operating system.
- Ensure **Docker is running** and check by executing: 
- docker --version
-----
**Step 2: Create a Project Directory**

Open a terminal and create a directory for your WordPress project:

mkdir wordpress-docker

cd wordpress-docker

-----
**Step 3: Create a docker-compose.yml File**

Inside your wordpress-docker folder, create a docker-compose.yml file:

version: '3.8'

services:

`  `db:

`    `image: mysql:5.7

`    `container\_name: wordpress-db

`    `restart: always

`    `environment:

`      `MYSQL\_ROOT\_PASSWORD: rootpassword

`      `MYSQL\_DATABASE: wordpress

`      `MYSQL\_USER: wordpressuser

`      `MYSQL\_PASSWORD: wordpresspassword

`    `volumes:

`      `- db\_data:/var/lib/mysql

`  `wordpress:

`    `depends\_on:

`      `- db

`    `image: wordpress:latest

`    `container\_name: wordpress-app

`    `restart: always

`    `ports:

`      `- "8080:80"

`    `environment:

`      `WORDPRESS\_DB\_HOST: db:3306

`      `WORDPRESS\_DB\_USER: wordpressuser

`      `WORDPRESS\_DB\_PASSWORD: wordpresspassword

`      `WORDPRESS\_DB\_NAME: wordpress

`    `volumes:

`      `- wordpress\_data:/var/www/html

volumes:

`  `db\_data:

`  `wordpress\_data:

-----

Since nano is not available in PowerShell, you can use **Notepad**.

Run this command in PowerShell:

notepad docker-compose.yml

created, run the following command to start the WordPress and MySQL containers:

docker-compose up -d

If you get an error saying docker-compose is not recognized, try:

docker compose up -d

(Note: **Docker Compose v2** now uses docker compose without the hyphen.)

After running the command, open [**http://localhost:8080**](http://localhost:8080/) in your browser to complete the WordPress setup.

