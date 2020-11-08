# docker-phpdev

Project template for PHP development in a Docker environment.

Usage:

- Put project files in app/

- Build

  `````$ docker-compose build`````

- Start containers

  `````$ docker-compose up`````
  
- Run commands within the cli container

  ```
  $ docker-compose exec cli composer install -vvv
  $ docker-compose exec cli vendor/bin/simple-phpunit
  ```
  
NOTES:

- Add PHP modules and/or other CLI tools in cli/Dockerfile
- The containers can be reached from other services via their container name.
- Using with Symfony

  Change web/000-default.conf

    ```fcgi://cli:9000/app/$1```
    
    to
    
    ```fcgi://cli:9000/app/public/$1```
    
  Change web service volumes in docker-compose.yml
   
   ```- ./app:/var/www/html```
   
   to
   
   ```- ./app/public:/var/www/html```
   
  Run something like the following if creating a project from scratch
  
  ```
  $ docker-compose exec cli composer create-project symfony/skeleton .
  ```
- The container names need to be unique when using this template for multiple projects

  Rename the containers

    from
    ```
    container_name: cli
    container_name: web
    container_name: db
    ```
    
    to
    
    ```
    container_name: cli-<project name>
    container_name: web-<project name>
    container_name: db-<project name>
    ```
  
  Change host port numbers but keep container port numbers
  
    ```
    - "8001:80"
    - "4431:443"
    - "33061:3306"
    ```