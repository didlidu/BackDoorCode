# BackDoorCode

Very stupid code only for data protection practice

This is repository for developers only, if you are interested in clear competition please don't read docs and sources

Please be welocome to contribute

# Use DB:

DROP DATABASE IF EXISTS test;

CREATE DATABASE test;

CREATE TABLE users (
   id INTEGER AUTO_INCREMENT,
   login varchar(255),
   pass varchar(255),
   data varchar(255),
   PRIMARY KEY(id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO users VALUES(NULL,'login','pass','goto A');

to connect:
mysql -u user -h 46.37.146.33 -P 7306 -pqwerty123
