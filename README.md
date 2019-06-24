# lanedbs
This is a simple docker-compose project to help run local instances of mysql or 
postgresql databases when developing apps.

## Setup
* Install docker
* Install docker-compose
* Ensure the `dbnet` network is trusted in your local firewall.
* Create these dirs:
  * <project_root>/data/mysql
  * <project_root>/data/pgadmin
  * <project_root>/data/postgresql
* Run `docker-compose up -d`

## Adding Other DB's

You can add more databases, either other versions of MySQL/PostgreSQL, or 
entirely different databases like Redis or Elasticsearch.

Just add a new server to the docker-compose.yml file.

For example, to set up MySQL 8:

(I have not tested this.)

Add `<project_root>/data/mysql` directory for the data.

Then add the service like so:

```yaml
services:
# ... existing services
  mysql8:
    image: mysql:8
    container_name: lanedbs_mysql8
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
    volumes:
      - type: bind
        source: ./data/mysql8
        target: /var/lib/mysql
      - type: bind
        source: ./mysql8.custom.cnf
        target: /etc/mysql/conf.d/99-custom.cnf
    ports:
      - 127.0.0.1:43888:3306
    networks:
      - dbnet
```