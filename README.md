# Bookstack-Windows-Docker-Desktop
Tutorial to run [Bookstack](https://github.com/BookStackApp/BookStack) under Windows using Docker Desktop

## Installation
* Go to the [Docker Desktop Webpage](https://www.docker.com/products/docker-desktop/) and download `Docker Desktop for Windows`.
* Go to the [installation instructions of Bookstack](https://www.bookstackapp.com/docs/admin/installation/). Under the [Docker section](https://www.bookstackapp.com/docs/admin/installation/#docker) click on the corresponding [GitHub Repository of LinuxServer.io](https://github.com/linuxserver/docker-bookstack). There you find the following `docker-compose information`:
```
---
version: "2"
services:
  bookstack:
    image: lscr.io/linuxserver/bookstack
    container_name: bookstack
    environment:
      - PUID=1000
      - PGID=1000
      - APP_URL=https://bookstack.example.com
      - DB_HOST=bookstack_db
      - DB_PORT=3306
      - DB_USER=bookstack
      - DB_PASS=<yourdbpass>
      - DB_DATABASE=bookstackapp
    volumes:
      - ./bookstack_app_data:/config
    ports:
      - 6875:80
    restart: unless-stopped
    depends_on:
      - bookstack_db
  bookstack_db:
    image: lscr.io/linuxserver/mariadb
    container_name: bookstack_db
    environment:
      - PUID=1000
      - PGID=1000
      - MYSQL_ROOT_PASSWORD=<yourdbpass>
      - TZ=Europe/London
      - MYSQL_DATABASE=bookstackapp
      - MYSQL_USER=bookstack
      - MYSQL_PASSWORD=<yourdbpass>
    volumes:
      - ./bookstack_db_data:/config
    restart: unless-stopped
```
* Navigate to a location where you want to store `bookstack-docker` (e.g. `F:\bookstack-docker`) and create a `docker-compose.yml` file (i.e. `F:\bookstack-docker\docker-compose.yml`) with the contents of the `docker-compose information` shown above.
* In `docker-compose.yml` change the value of `APP_URL` to the local ip address (e.g. `APP_URL=192.168.1.125`). You can Use `ipconfig` to determine the local ip address (i.e. the value of the `IPv4 Address` entry).
* In `docker-compose.yml` adjust the values of `DB_PASS=<yourdbpass>`, `MYSQL_ROOT_PASSWORD=<yourdbpass>` and `MYSQL_PASSWORD=<yourdbpass>`
* Save the file.
* Run `docker-compose up -d` (no admin rights required).

## Execution
* Open the `Docker Desktop GUI` and click on the link `6875:80` of the `bookstack container`. This opens the corresponding ip address in the browser (e.g. http://192.168.1.125:6875/login).
* **BE PATIENT! IT MIGHT TAKE A WHILE UNTIL THE CONTAINERS CAN BE ACCESSED!** (~30 seconds after `docker-compose up -d` has finished its execution).
* Login into bookstack with the default email `admin@admin.com` and the default password `password`.

If you experience any problems you might want to have a look at this [issue](https://github.com/linuxserver/docker-bookstack/issues/125).
