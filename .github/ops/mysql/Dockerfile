ARG DIALECT=mysql:8.0

FROM $DIALECT as builder

ARG SERVER=mysqld
ENV MYSQL_ROOT_PASSWORD=pass

# Remove the last line of the entry point script, leaving the initialization code but omitting actually starting the db.
RUN sed -i 's/exec "$@"/echo "not running $@"/' /usr/local/bin/docker-entrypoint.sh
RUN mkdir -p /initialized-db && chown -R mysql:mysql /initialized-db
RUN /usr/local/bin/docker-entrypoint.sh ${SERVER} --datadir /initialized-db

FROM $DIALECT

COPY --from=builder ./initialized-db /var/lib/mysql/