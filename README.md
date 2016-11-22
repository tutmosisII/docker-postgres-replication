Replicación en Streaming
==============================

Se basa en [Replicación de postgres](https://github.com/nebirhos/docker-postgres-replication)

Etiquetas
----

Las imagenes se toman de la [Imágen oficial de postgres](https://hub.docker.com/_/postgres/).

Etiquetas Soportadas:

* 9.4 

Migrar una base de datos existente a un esquema replicado
--------------------------------------------------

```
+----------------+
|     DOCKER     |
+----------------+       +----+
|                |       |    +-----------+
|  POSTGRESQL    |       |    CURRENT 	  |		
|                +------>| PGDATA  FOLDER |
|    MASTER	     |   	 |				  |
+----------------+       +----------------+
        |
	   \ /  (Wal replication)
	    |
+----------------+
|     DOCKER     |
+----------------+
|  POSTGRESQL    |
|    STANDBY	 |
+----------------+
```

Trabajando con Docker Compose
-----------------------

```
docker-compose up
```


Trabajando con Docker
---------------

Corriendo inicialmente el Nodo Master:

```
docker run -p 127.0.0.1:5432:5432 --name postgres-master nebirhos/postgres-replication
```


Corriendo el esclavo(s) de Postgres:

```
docker run -p 127.0.0.1:5433:5432 --link postgres-master \
           -e POSTGRES_MASTER_SERVICE_HOST=postgres-master \
           -e REPLICATION_ROLE=slave \
           -t nebirhos/postgres-replication
```


Notas
-----

La replicación se configura con los scripts localizados en `/docker-entrypoint-initdb.d` .
Dejando intactos los scripts originales de postgres.