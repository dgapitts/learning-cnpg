# First steps with pgbench and psql

## running pgbench installation - via `kubectl exec`

```
~/projects/learning-cnpg $ kubectl exec  cluster-with-metrics-1  -- pgbench -i
Defaulted container "postgres" out of: postgres, bootstrap-controller (init)
dropping old tables...
NOTICE:  table "pgbench_accounts" does not exist, skipping
NOTICE:  table "pgbench_branches" does not exist, skipping
NOTICE:  table "pgbench_history" does not exist, skipping
NOTICE:  table "pgbench_tellers" does not exist, skipping
creating tables...
generating data (client-side)...
100000 of 100000 tuples (100%) done (elapsed 0.16 s, remaining 0.00 s)
vacuuming...
creating primary keys...
done in 0.66 s (drop tables 0.01 s, create tables 0.15 s, client-side generate 0.31 s, vacuum 0.12 s, primary keys 0.07 s).
~/projects/learning-cnpg $ kubectl exec  cluster-with-metrics-1  -- pgbench -i -s 3
Defaulted container "postgres" out of: postgres, bootstrap-controller (init)
dropping old tables...
creating tables...
generating data (client-side)...
100000 of 300000 tuples (33%) done (elapsed 0.15 s, remaining 0.31 s)
200000 of 300000 tuples (66%) done (elapsed 0.50 s, remaining 0.25 s)
300000 of 300000 tuples (100%) done (elapsed 0.97 s, remaining 0.00 s)
vacuuming...
creating primary keys...
done in 1.74 s (drop tables 0.01 s, create tables 0.01 s, client-side generate 1.10 s, vacuum 0.24 s, primary keys 0.39 s).
```

## other commands to be documented

```
  519  for i in {1..10};do uptime;kubectl exec  cluster-with-metrics-1  -- pgbench -t 1000;sleep 3;done
  520  kubectl exec  cluster-with-metrics-1  -- pgbench -i -s 9
  521  kubectl exec  cluster-with-metrics-1  -- psql
  522  kubectl exec  cluster-with-metrics-1  -- psql -d pgbench
  523  kubectl exec  cluster-with-metrics-1  -- psql
  524  kubectl exec -it cluster-with-metrics-1  -- /bin/sh
  525  kubectl exec  cluster-with-metrics-1  -- psql
  526  kubectl exec  cluster-with-metrics-1  -- pgbench -i -s 9
```