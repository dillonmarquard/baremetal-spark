# baremetal-spark
Deploy and configure a spark cluster with postgres metastore, iceberg rest catalog and seaweedfs

## Requirements
### Local
* docker (https://www.docker.com/get-started/)
* gradle (https://docs.gradle.org/current/userguide/installation.html)

### Future
* k3d (https://k3d.io/stable/)

## Setup
```shell
gradle downloadJars
docker compose up --build --detach
docker exec -it spark-iceberg /opt/spark/sbin/start-master.sh
docker exec -it spark-iceberg /opt/spark/sbin/start-worker.sh spark://spark-iceberg:7077
```

## Examples
### Spark SQL
```shell
docker exec -it spark-iceberg /opt/spark/bin/spark-sql \
    --load-spark-defaults \
    --master spark://spark-iceberg:7077 \
    --conf spark.sql.catalog.icicle.credential=admin:secret \
    --conf spark.sql.catalog.icicle.s3.access-key-id=admin \
    --conf spark.sql.catalog.icicle.s3.secret-access-key=secret \
    --conf spark.sql.catalog.icicle.client.region=us-east-1 \
    --conf spark.sql.catalog.icicle.rest-metrics-reporting-enabled=false \
    --conf spark.cores.max=1 \
    --conf spark.sql.defaultCatalog=icicle
```

### PySpark
```shell
docker exec -it spark-iceberg /opt/spark/bin/pyspark \
    --load-spark-defaults \
    --master spark://spark-iceberg:7077 \
    --conf spark.sql.catalog.icicle.credential=admin:secret \
    --conf spark.sql.catalog.icicle.s3.access-key-id=admin \
    --conf spark.sql.catalog.icicle.s3.secret-access-key=secret \
    --conf spark.sql.catalog.icicle.client.region=us-east-1 \
    --conf spark.sql.catalog.icicle.rest-metrics-reporting-enabled=false \
    --conf spark.cores.max=1 \
    --conf spark.sql.defaultCatalog=icicle
```