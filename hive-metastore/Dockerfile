FROM postgres:11.12

COPY configs/hive-schema-2.3.0.postgres.sql /hive/hive-schema-2.3.0.postgres.sql
COPY configs/hive-txn-schema-2.3.0.postgres.sql /hive/hive-txn-schema-2.3.0.postgres.sql
COPY configs/upgrade-2.3.0-to-3.0.0.postgres.sql /hive/upgrade-2.3.0-to-3.0.0.postgres.sql
COPY configs/upgrade-3.0.0-to-3.1.0.postgres.sql /hive/upgrade-3.0.0-to-3.1.0.postgres.sql

COPY configs/init-hive-db.sh /docker-entrypoint-initdb.d/init-user-db.sh
