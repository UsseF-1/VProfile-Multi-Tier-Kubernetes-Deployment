FROM mysql:8.0.33

LABEL "Project"="Vprofile"
LABEL "Author"="Youssef"

ENV MYSQL_ROOT_PASSWORD="YOUR_DB_PASSWORD"
ENV MYSQL_DATABASE="YOUR_DB_NAME"

ADD db_backup.sql docker-entrypoint-initdb.d/db_backup.sql