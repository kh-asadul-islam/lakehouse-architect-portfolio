SHOW CATALOGS;

|catalog|
|---|
|da|
|dbacademy|
|samples|
|system|
|workspace|

SHOW SCHEMAS IN da;
|databaseName|
|---|
|bronze|
|default|
|gold|
|information_schema|
|silver|

SHOW TABLES IN da.silver;
|database|tableName|isTemporary|
|---|---|---|
|silver|nyctaxi_pickup_hours|false|
|silver|yellow_trips|false|
|silver|yellow_trips_conformed|false|
|silver|yellow_trips_quarantine|false|

SHOW VOLUMES IN da.bronze;
|database|volume_name|
|---|---|
|bronze|landing|

DESCRIBE EXTENDED da.silver.yellow_trips;
|col_name|data_type|comment|
|---|---|---|
|vendor_id|int|null|
|pickup_ts|timestamp|null|
|dropoff_ts|timestamp|null|
|passenger_count|int|null|
|trip_distance|double|null|
|rate_code_id|int|null|
|store_and_fwd_flag|string|null|
|pu_location_id|int|null|
|do_location_id|int|null|
|payment_type|int|null|
|fare_amount|decimal(10,2)|null|
|extra|decimal(10,2)|null|
|mta_tax|decimal(10,2)|null|
|tip_amount|decimal(10,2)|null|
|tolls_amount|decimal(10,2)|null|
|improvement_surcharge|decimal(10,2)|null|
|total_amount|decimal(10,2)|null|
|congestion_surcharge|decimal(10,2)|null|
|airport_fee|decimal(10,2)|null|
|source_file|string|null|
|ingested_at|timestamp|null|
|cbd_congestion_fee|decimal(10,2)|null|
|pickup_date|date|null|
|trip_minutes|double|null|
|trip_id|string|null|
||||
|# Delta Statistics Columns|||
|Column Names|improvement_surcharge, ingested_at, trip_minutes, tolls_amount, tip_amount, pu_location_id, passenger_count, extra, do_location_id, airport_fee, total_amount, source_file, trip_id, vendor_id, trip_distance, pickup_date, payment_type, fare_amount, pickup_ts, store_and_fwd_flag, cbd_congestion_fee, congestion_surcharge, rate_code_id, mta_tax, dropoff_ts||
|Column Selection Method|first-32||
||||
|# Detailed Table Information|||
|Catalog|da||
|Database|silver||
|Table|yellow_trips||
|Created Time|Tue Oct 20 11:16:39 UTC 2026||
|Last Access|UNKNOWN||
|Created By|Spark ||
|Statistics|530033966 bytes, 10305820 rows||
|Type|MANAGED||
|Location|||
|Provider|delta||
|Owner|asadul0801@hotmail.com||
|Is_managed_location|true||
|Predictive Optimization|ENABLE (inherited from METASTORE metastore_aws_us_east_2)||
|Table Properties|[delta.enableDeletionVectors=true,delta.feature.appendOnly=supported,delta.feature.deletionVectors=supported,delta.feature.invariants=supported,delta.minReaderVersion=3,delta.minWriterVersion=7,delta.parquet.compression.codec=zstd,delta.parquet.format.version=2.12.0,delta.parquet.format.version.afe.internal=2.12.0,io.unitycatalog.tableId=bbad5b25-3390-4fa5-a45b-b004f6dae7b6]||

SELECT table_schema, table_name, table_type FROM da.information_schema.tables
WHERE table_schema IN ('bronze','silver','gold') ORDER BY 1,2;
SELECT table_name, column_name, comment FROM da.information_schema.columns
WHERE table_schema = 'silver' AND comment IS NOT NULL;
|table_schema|table_name|table_type|
|---|---|---|
|bronze|nyctaxi|MANAGED|
|bronze|taxi_zones|MANAGED|
|bronze|yellow_trips|MANAGED|
|silver|nyctaxi_pickup_hours|MANAGED|
|silver|yellow_trips|MANAGED|
|silver|yellow_trips_conformed|MANAGED|
|silver|yellow_trips_quarantine|MANAGED|
