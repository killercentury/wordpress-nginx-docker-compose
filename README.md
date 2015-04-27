# WordPress on Nginx with Docker

The repo demonstrates how to use docker compose to launch a WordPress with the official FPM + Nginx images.

## Preparation
If you haven't created a data volume container called "mysql-dev-data" or initialize the MySQL with password before, Please follow below instructions to set up the basic environment required by this demo. **You only need to go through this preparation step once! It only helps your to setup a proper containerized MySQL if you haven't and you can re-use it for different projects on your machine.** (Replace the "PASSWORD" with your own password.)

### 1. Create a data volume container for MySQL
```
docker create --name mysql-dev-data -v /var/lib/mysql busybox
```

### 2. Initialize your MySQL with password
```
docker run --name mysql --volumes-from=mysql-dev-data -e MYSQL_ROOT_PASSWORD=PASSWORD -d -p 3306:3306 mysql
```

### 3. Create a ".env.dev" file
Put the database password as the environment variable in this ".env.dev" file. (The reason why ".env.dev" is not in VCS is because it contains credentials that should not be in VCS as the general pratices.)
```
WORDPRESS_DB_PASSWORD=PASSWORD
```

### 4. Stop/Remove MySQL container
Your initial MySQL password has been stored in the "mysql-dev-data" data volume container at this point, so you can stop or remove the previous MySQL container now.
```
docker stop mysql
```

## Run
```
docker-compose up
```

## Notice
1. The default Docker Compose config will use a nginx config with sendfile off for boot2docker due to a bug in Virtual Box. You can replace it with the "nginx.conf" if you are using Linux.
2. If you are going to use WordPress on a subdirectory, you can use similar config as "server.subdirectory.conf" by replacing "sub" with the name of your subdirectory.
