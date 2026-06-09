# nifi-registry-playground

## ClickHouse demo flow

This repository now includes a NiFi Registry demo flow that reads from ClickHouse through JDBC.

- Bucket metadata: `nifi-flows/ClickHouseUsersDemo/bucket.yml`
- Flow snapshot: `nifi-flows/new-bucket/ClickHouseUsersDemo.json`

The demo flow contains:

- `ExecuteSQL` to query `system.users`
- `LogMessage` for success metrics
- `LogMessage` for JDBC/query failures
- `DBCPConnectionPool` configured for ClickHouse JDBC

Before importing the flow in NiFi, adjust these properties on the `ClickHouseConnectionPool` controller service:

- `Database Connection URL`: example `jdbc:clickhouse://clickhouse:8123/default`
- `Database Driver Class Name`: `com.clickhouse.jdbc.ClickHouseDriver`
- `Database Driver Locations`: path to the ClickHouse JDBC jar, for example `/opt/nifi/drivers/clickhouse-jdbc.jar`
- `Database User`: demo value is `demo_reader`
- `Password`: demo value is `demo_password`

The default query is:

```sql
SELECT name, storage, auth_type
FROM system.users
LIMIT 10
```

If your ClickHouse version exposes different columns, update the `SQL Query` property on the `ReadClickHouseUsers` processor after import.
