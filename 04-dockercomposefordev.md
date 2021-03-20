# How to use Docker Compose

Running docker from the command line is fine when experimenting with one container. However, docker commands can become massive very easily, and to remedy that we use a docker-compose configuration file.

## Your first docker-compose file

### A note about PHP

PHP is an interpreted language. Unlike Java, C, or C++ you can update PHP files on the fly. The following instructions will take advantage of this feature.

Feel free to use this configuration for your assignment.

```yaml
version: '3.8'

services:
  app:
    build: 
        dockerfile: Dockerfile-dev
        context: .
    ports:
    - 8080:80
    volumes:
    - .:/var/www/html
    depends_on:
    - mysql
  mysql:
    image: docker.io/mariadb:10.3
    volumes:
    - db:/var/lib/mysql
    - ./mysql/dumpfile_location:/docker-entrypoint-initdb.d/
    environment:
      MYSQL_ROOT_PASSWORD: root
    ports:
    - 3306:3306

volumes:
  db:
```

This docker-compose file does the following actions automatically:

* Creates 2 services called app and MySQL
* For app:
  * It looks in the same directory it's in for a `Dockerfile-dev` file, and builds the docker file
  * It automatically binds port 80 in the container to port 8080 on the host
  * It will mount the current directory it's in to `/var/www/html/` on the container
  * It will wait for the MySQL container to start up
* For MySQL:
  * It will download and run a MariaDB container
  * At the bottom there is a volumes deceleration. This tells docker to make a persistent volume where we can store our database. It mounts that volume to `/var/lib/mysql` on the container. 
  * It mounts a folder `./mysql/dumpfile_location` on the host under `/docker-entrypoint-initdb.d/` on the container. One feature of the container we are using, is it will look under this directory for any sql files, and run them.
  * It sets an environment variable called `MYSQL_ROOT_PASSWORD` to 'root'. In this container, this will set the root password to 'root'. **Warning, if you set a password to root in real life, you will get hacked**
  * It binds port 3306 on the host to port 3306 on the container.

## Try it out

To try this out, create a new folder, and make a file called `docker-compose.yaml`. Copy the above configuration into said file.

Copy the below Dockerfile into a new file called `Dockerfile-dev`. This is a container I made with some handy defaults, and XDebug, PHP's step debugger similar to Java.

```text
FROM docker.io/kingsukhoi/phpwdebugger:latest

RUN apt-get update -y

RUN docker-php-ext-install pdo pdo_mysql
```

Finally create a file called `index.php` and put be below content in it.

```php
<?
define('DBHOST', 'mysql');
define('DBUSER', 'root');// DO NOT USE THE ROOT USER IN REAL LIFE.
define('DBPASS', 'root');//DO NOT DO THIS PASSWORD IN REAL LIFE.
define('DBCONNSTRING',"mysql:host=" . DBHOST . ";charset=utf8mb4;");
try{
    $pdo = new PDO(DBCONNSTRING,DBUSER,DBPASS);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
}
catch(PDOException $pe){
    die($pe->getMessage());
}
$sql = "SELECT 'HELLO WORLD!!!'";
$stmt = $pdo->prepare($sql);
$stmt->execute();
$result = $stmt->fetch();
?>
<!DOCTYPE html>
<html>
    <body>
        <h1>My First PHP Container</h1>
        <p><? echo $result[0] ?></p>
    </body>
</html>
```

Finally in that folder, run `docker-compose up -d`. By default docker-compose will find a file called docker-compose.yaml, and execute that. The `-d` options makes it run in the backgroud.

Visit `localhost:8080` to run this file.

To shutdown these containers, run `docker-compose down`. This will stop all the containers, and preserve all the data in the MySQL database.

If you need to delete the MySQL volume, run `docker-comopose down -v`, and that will delete all volumes.

