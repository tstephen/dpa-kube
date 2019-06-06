- create user and db 
  ```
  postgresdb=# create role flowable with password 'flowable'; 
  CREATE ROLE
  postgresdb=# alter role flowable with login ; 
  ALTER ROLE
  postgresdb=# create database flowabledb owner flowable;
  CREATE DATABASE
  grant all on database  flowabledb to flowable;
  GRANT
  ```
