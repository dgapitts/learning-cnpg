# cnpg plugin status



```
~/projects/learning-cnpg $ kubectl cnpg status cluster-with-metrics
Cluster Summary
Name:                cluster-with-metrics
Namespace:           default
System ID:           7319502381962096666
PostgreSQL Image:    ghcr.io/cloudnative-pg/postgresql:16.1
Primary instance:    cluster-with-metrics-1
Primary start time:  2024-01-02 14:07:38 +0000 UTC (uptime 224h47m42s)
Status:              Cluster in healthy state
Instances:           3
Ready instances:     3
Current Write LSN:   0/25000060 (Timeline: 1 - WAL File: 000000010000000000000025)

Certificates Status
Certificate Name                  Expiration Date                Days Left Until Expiration
----------------                  ---------------                --------------------------
cluster-with-metrics-ca           2024-04-01 14:01:30 +0000 UTC  80.63
cluster-with-metrics-replication  2024-04-01 14:01:30 +0000 UTC  80.63
cluster-with-metrics-server       2024-04-01 14:01:30 +0000 UTC  80.63

Continuous Backup status
Not configured

Physical backups
No running physical backups found

Streaming Replication status
Replication Slots Enabled
Name                    Sent LSN    Write LSN   Flush LSN   Replay LSN  Write Lag  Flush Lag  Replay Lag  State      Sync State  Sync Priority  Replication Slot
----                    --------    ---------   ---------   ----------  ---------  ---------  ----------  -----      ----------  -------------  ----------------
cluster-with-metrics-2  0/25000060  0/25000060  0/25000060  0/25000060  00:00:00   00:00:00   00:00:00    streaming  async       0              active
cluster-with-metrics-3  0/25000060  0/25000060  0/25000060  0/25000060  00:00:00   00:00:00   00:00:00    streaming  async       0              active

Unmanaged Replication Slot Status
No unmanaged replication slots found

Managed roles status
No roles managed

Tablespaces status
No managed tablespaces

Instances status
Name                    Database Size  Current LSN  Replication role  Status  QoS         Manager Version  Node
----                    -------------  -----------  ----------------  ------  ---         ---------------  ----
cluster-with-metrics-1  164 MB         0/25000060   Primary           OK      BestEffort  1.22.0           dbcluster01-control-plane
cluster-with-metrics-2  164 MB         0/25000060   Standby (async)   OK      BestEffort  1.22.0           dbcluster01-control-plane
cluster-with-metrics-3  164 MB         0/25000060   Standby (async)   OK      BestEffort  1.22.0           dbcluster01-control-plane
```
