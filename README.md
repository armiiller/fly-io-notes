# Machine Management

## Restart App
[fly apps restart](https://fly.io/docs/flyctl/apps-restart/)

```bash
fly apps restart <app-name>
```

## Scaling

### Scale Count
[fly scale count](https://fly.io/docs/flyctl/scale-count/)

```bash
fly scale count <num> [--region sea]
```

### Scale Memory
[fly scale memory](https://fly.io/docs/flyctl/scale-memory/)

```bash
fly scale memory 2048
```

# Postgres

## Check DB Sizes
```sql
SELECT
    table_name,
    pg_size_pretty(table_size) AS table_size,
    pg_size_pretty(indexes_size) AS indexes_size,
    pg_size_pretty(total_size) AS total_size
FROM (
    SELECT
        table_name,
        pg_table_size(table_name) AS table_size,
        pg_indexes_size(table_name) AS indexes_size,
        pg_total_relation_size(table_name) AS total_size
    FROM (
        SELECT ('"' || table_schema || '"."' || table_name || '"') AS table_name
        FROM information_schema.tables
    ) AS all_tables
    ORDER BY total_size DESC
) AS pretty_sizes;
```

## Expand Drive Space
[Fly.io: Increase diskspace for postgresql](https://community.fly.io/t/increase-diskspace-for-postgresql/8964)
[fly volumes extend](https://fly.io/docs/flyctl/volumes-extend/)

```bash
# 180 is the target size in GB
fly volumes extend <volume-id> -s 180
```

# Wireguard
I've run into issues where IPs stop resolving. Create a different wireguard tunnel.

[Fly private networking](https://fly.io/docs/networking/private-networking/)

```bash
fly wireguard create
shortname.conf
sudo mv shortname.conf /etc/wireguard/
sudo chown root:root /etc/wireguard/shortname.conf

# should go in ~/.bashrc
alias fly-up="sudo wg-quick up ashortname"
alias fly-down="sudo wg-quick down shortname"
```