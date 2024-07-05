# Wordpress Seite über Docker
## WordPress-Website mit containisierten Umgebung mithilfe von Docker bereitstellen

## 1 Docker installieren
### Grundanforderung: Docker auf meinem Windows Rechner
### Installation überprüfen: 
```bash 
docker --version
```
## 2 WordPress Container erstellen/konfigurieren

### MariaDb-Image von Docker Hub heruntergeladen (für die DB von WordPress)
```bash 
docker pull mariadb 
```
### MariaDB Container starten
```bash 
docker run --name my-db -e MYSQL_ROOT_PASSWORD=db-password -d mariadb
```

## Das gleiche noch mit wordpress
### Wordpress-Image von Docker Hub heruntergeladen 
```bash
docker pull wordpress
```
### Wordpress Container gestartet und mit der zuvor erstellten MariaDB-Datenbank verbunden. Der Container läuft auf Port 8080
```bash
docker run --name my-wordpress -p 8080:80 --link my-db:mariadb -d wordpress
``` 

### DB für Wordpress aufsetzen
```sql
CREATE DATABASE wordpress DEFAULT CHARACTER SET uft8 COLLATE utf8_unicode_ci;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'tbz2024'
GRANT ALL ON wordpress.* TO 'wp_user'@'localhost';
```


### Einstellungen übers GUI 
### Dann habe ich mich bei der WordPress-Installation auf dem Localhost angemeldet 
![Auf Wordpress Localhost angemeldet](<Erstanmeldung Wordpress.png>)

![Willkommen auf Wordpress](willkommenWordpress.png)

## Docker Setup
### Neue Datei erstellen : docker-compose_mysql_wordpress.yaml
### Folgender Inhalt ist in der Datei:
```yaml
version: "3" 

services:
  
  db:
    image: mysql:debian
   
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: tbz2022
      MYSQL_DATABASE: WordPressDataBase
      MYSQL_USER: WordPressUser
      MYSQL_PASSWORD: tbz2022
    
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    restart: always
 
    ports:
      - "8000:80"
      
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: WordPressUser
      WORDPRESS_DB_PASSWORD: tbz2022
      WORDPRESS_DB_NAME: WordPressDataBase

    volumes:
      ["./:/var/www/html"]
volumes:
  mysql: {}

  ```

  

