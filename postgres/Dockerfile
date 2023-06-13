# debian based PostgreSQL container from docker hub
FROM postgres:13-bullseye
RUN if [ ! -d /docker-entrypoint-initdb.d ]; then mkdir /docker-entrypoint-initdb.d; fi

# environment variables used for initialising iCAT database
ENV IRODS_ICAT_DBNAME=ICAT
ENV IRODS_ICAT_DBUSER=irods
ENV IRODS_ICAT_DBPASS=test123
ENV POSTGRES_PASSWORD=4bc8ad65301ba218

COPY init-icat-db.sh /docker-entrypoint-initdb.d
