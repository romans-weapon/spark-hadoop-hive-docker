FROM postgres:11.12

RUN export LANGUAGE=en_US.UTF-8

COPY configs/hive-schema-2.3.0.postgres.sql /hive/hive-schema-2.3.0.postgres.sql
COPY configs/hive-txn-schema-2.3.0.postgres.sql /hive/hive-txn-schema-2.3.0.postgres.sql
COPY configs/init-hive-db.sh /docker-entrypoint-initdb.d/init-user-db.sh
