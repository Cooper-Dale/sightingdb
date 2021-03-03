FROM rust:latest
LABEL  Corporation <sebastien@honeynet.org>
RUN apt update -q
RUN apt -qy --no-install-recommends install git apt-transport-https ca-certificates pkg-config openssl libssl-dev jed
RUN update-ca-certificates
WORKDIR /opt/
#Důležitá změna, kdy je vybráno klonování z větve store_to_disc
RUN git clone --branch store_to_disk https://github.com/Cooper-Dale/sightingdb.git /opt/sightingdb
WORKDIR /opt/sightingdb
RUN cargo build --release
COPY sightingdb.conf /etc/sightingdb/
RUN mkdir /etc/sightingdb/ssl
WORKDIR /etc/sightingdb/ssl
COPY ssl-answers /etc/sightingdb/ssl/
RUN openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout key.pem -out cert.pem < /etc/sightingdb/ssl/ssl-answers
WORKDIR /opt/sightingdb/
RUN mkdir /var/lib/sightingdb/
RUN mkdir /var/log/sightingdb/

EXPOSE 9999
COPY sightingdb-entrypoint.sh /usr/local/bin/
#Přidán řádek, který dává práva ke spuštění ...
RUN ["chmod", "+x", "/usr/local/bin/sightingdb-entrypoint.sh"]
#Testuji zde chybu, entrypoint.sh nemá oprávnění ke spuštění
ENTRYPOINT ["/usr/local/bin/sightingdb-entrypoint.sh"]

