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
* In `docker-compose.yml` change the value of `APP_URL` to `APP_URL=http://<Local IP>:6875` (For instance `APP_URL=http://192.168.1.125:6875` where `192.168.1.125` is your local ip address). You can use `ipconfig` to determine the local ip address (i.e. the value of the `IPv4 Address` entry). **It is absolutely crucial to provide the `http://` prefix and the port `6875`. Do not use `https`.**
* Replace `image: lscr.io/linuxserver/mariadb` with `image: mysql`.
* In `docker-compose.yml` adjust the values of `DB_PASS=<yourdbpass>`, `MYSQL_ROOT_PASSWORD=<yourdbpass>` and `MYSQL_PASSWORD=<yourdbpass>`
* Good practice: provide a fixed version for bookstack (e.g. `image: lscr.io/linuxserver/bookstack:23.05.2`) and mysql (e.g. `image: mysql:8.0.33`). Available versions are listed [here](https://hub.docker.com/r/linuxserver/bookstack/tags) and [here](https://hub.docker.com/_/mysql).
* Save the file.
* In the `Windows Powershell` run `docker-compose up -d` (no admin rights required).

## Execution
* Open the `Docker Desktop GUI` and click on the link `6875:80` of the `bookstack container`. This opens the corresponding ip address in the browser (e.g. http://192.168.1.125:6875/login).
* **Be patient! It might take a while until the containers can be accessed!** (~30 seconds after `docker-compose up -d` has finished its execution).
* Login into bookstack with the default email `admin@admin.com` and the default password `password` as mentioned in the [bookstack installation instructions](https://www.bookstackapp.com/docs/admin/installation/#manual).

## Share in local network
Add `%ProgramFiles%\Docker\Docker\resources\com.docker.backend.exe` to the windows firewall. The corresponding information can be found when clicking under `Control Panel\All Control Panel Items\Windows Defender Firewall` on `Allow an app or feature through Windows Defender Firewall`).

## Tested with
* `Docker Desktop`: `v4.20.1`
* `bookstack` container: `ef5500acb1c2c6f830da1509b7d844e555f24f548cd329ffef0cb56c89b92d43`
* `bookstack_db` container: `fd8c7020d2239059feb4f06769fac9009bc5da57f9d28409fdd60f265120044b`
* `lscr.io/linuxserver/bookstack` image: `26d9a2577846`
* `mysql` image: `91b53e2624b4`

If you experience any problems you might want to have a look at this [issue](https://github.com/linuxserver/docker-bookstack/issues/125).

## Other documentation / wiki software packages
* https://en.wikipedia.org/wiki/Comparison_of_wiki_software
* https://github.com/awesome-selfhosted/awesome-selfhosted#note-taking--editors
* https://github.com/awesome-selfhosted/awesome-selfhosted#wikis
