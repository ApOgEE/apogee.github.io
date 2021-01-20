---
layout: post
title: MariaDB CLI dengan Docker
date: 2021-01-21 00:59:00 -0000
tags: blog docker mariadb nota
---

Cara nak run mariadb server dalam docker.
```
docker run --rm -d -p 3306:3306 \
-e MYSQL_ROOT_PASSWORD='testing123' \
mariadb:latest
```

kasi dia run kat background

Lepas tu check container id:
```
docker ps
```

dan masuk
```
docker exec -it ${CONT_ID} mysql -u root -p
```

Masukkan password tadi
```
> show databases;
> create database mydata;
> CREATE USER 'phpcoder'@'localhost' IDENTIFIED BY 'phpcoder1234';
> GRANT ALL PRIVILEGES ON `phpcoder`.`*` TO 'phpcoder'@'localhost';
> GRANT ALL PRIVILEGES ON `mydata`.* TO 'phpcoder'@'localhost';

> CREATE USER 'phpcoder'@'%' IDENTIFIED BY 'phpcoder1234';
> GRANT ALL PRIVILEGES ON `phpcoder`.* TO 'phpcoder'@'%';
> GRANT ALL PRIVILEGES ON `mydata`.* TO 'phpcoder'@'%';
> FLUSH PRIVILEGES;
```

### Create table
```
> CREATE TABLE IF NOT EXISTS twitter_crawl(
    id INT AUTO_INCREMENT PRIMARY KEY,
    post_url VARCHAR(255) NOT NULL,
    content TEXT
) ENGINE=INNODB;

```

dan masuk
```
docker exec -it ${CONT_ID} mysql -u phpcoder -p
```
Buat database
```
> CREATE DATABASE phpcoder;
```

Cara nak tengok schema.
```
> describe twitter_crawl;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | int(11)      | NO   | PRI | NULL    | auto_increment |
| post_url | varchar(255) | NO   |     | NULL    |                |
| content  | text         | YES  |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
3 rows in set (0.001 sec)
```
## Docker dump DB
```
docker exec -it ${CONT_ID} mysqldump -u root --password='testing123' -B mydata > db_backup/mydata.sql
```

## Docker restore DB
```
cat db_backup/mydata.sql | docker exec -i ${CONT_ID} /usr/bin/mysql -u root --password='testing123' mydata
```