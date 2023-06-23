FROM python:3.9-bullseye

LABEL MAINTAINER = Michael Kluge <michael.kluge@tu-dresden.de>

RUN apt update
RUN apt upgrade -y
RUN apt install -y sudo lsb-release python3-pip systemctl bind9-host netcat netcat-openbsd
# i think this is wrong ... DNS will not work yet
#RUN host icat-db
RUN pip3 install --upgrade pip
RUN wget -qO - https://packages.irods.org/irods-signing-key.asc | sudo apt-key add -
RUN echo "deb [arch=amd64] https://packages.irods.org/apt/ $(lsb_release -sc) main" | sudo tee \
/etc/apt/sources.list.d/renci-irods.list
RUN apt update
RUN apt upgrade -y
RUN apt install -y irods-server irods-database-plugin-postgres
RUN pip3 install distro psutil pyodbc
COPY docker-entrypoint.sh /docker-entrypoint.sh
COPY catalog_service_provider.json /tmp/catalog_service_provider.json

## data volumes
VOLUME "/etc/irods" "/var/lib/irods/iRODS/server/log" "/var/lib/irods/Vault"

## network ports
ENV IRODS_ZONE_PORT=1247
ENV IRODS_CONTROLPLANE_PORT=1248
ENV IRODS_DATA_PORT_RANGE_BEG=20000
ENV IRODS_DATA_PORT_RANGE_END=20199
EXPOSE 4321 $IRODS_ZONE_PORT $IRODS_CONTROLPLANE_PORT $IRODS_DATA_PORT_RANGE_BEG-$IRODS_DATA_PORT_RANGE_END

## entrypoint
ENV IRODS_ICAT_DBSERVER=icat-db
ENV IRODS_ICAT_DBPORT=5432
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["irods"]
