FROM python:3.10-bullseye

LABEL MAINTAINER = Michael Kluge <michael.kluge@tu-dresden.de>

RUN apt update
RUN apt upgrade -y
RUN apt install -y sudo lsb-release python3-pip
RUN pip3 install --upgrade pip
RUN wget -qO - https://packages.irods.org/irods-signing-key.asc | sudo apt-key add -
RUN echo "deb [arch=amd64] https://packages.irods.org/apt/ $(lsb_release -sc) main" | sudo tee \
/etc/apt/sources.list.d/renci-irods.list
RUN apt update
RUN apt upgrade -y
RUN apt install -y irods-server irods-database-plugin-postgres
RUN pip3 install distro psutil
RUN python3 /var/lib/irods/scripts/setup_irods.py --json_configuration_file catalog_service_provider.json

## data volumes
VOLUME "/etc/irods" "/var/lib/irods/iRODS/server/log" "/var/lib/irods/Vault"

## network ports
EXPOSE 4321 $IRODS_ZONE_PORT $IRODS_CONTROLPLANE_PORT $IRODS_DATA_PORT_RANGE_BEG-$IRODS_DATA_PORT_RANGE_END

## entrypoint
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["irods"]
