# Create a new proxy

## Core 1

HTTP API:

```bash
curl -X POST localhost:6666/proxies -H "Content-Type: application/json" -d '{ "name": "transaction_core_1", "listen": "[::]:16001", "upstream": "core1:16000", "enabled": true}'
```

CLI:

```
toxiproxy-cli -h localhost:6666 create --listen '[::]:16001' --upstream core1:16000 transaction_core_1
```

## Core 2

HTTP API:

```bash
curl -X POST localhost:6666/proxies -H "Content-Type: application/json" -d '{ "name": "transaction_core_2", "listen": "[::]:26001", "upstream": "core2:26000", "enabled": true}'
```

CLI:
```
toxiproxy-cli -h localhost:6666 create --listen '[::]:26001' --upstream core1:26000 transaction_core_2
```

## Core 3

HTTP API:

```bash
curl -X POST localhost:6666/proxies -H "Content-Type: application/json" -d '{ "name": "transaction_core_3", "listen": "[::]:36001", "upstream": "core3:36000", "enabled": true}'
```

CLI:

```
toxiproxy-cli -h localhost:6666 create --listen '[::]:36001' --upstream core1:36000 transaction_core_3
```

# Create or replace a list of proxies
POST /populate

## Core {1,2,3}
```bash
curl -X POST localhost:6666/populate \
    -H "Content-Type: application/json" \
    -d '[
        { "name": "transaction_core_1", "listen": "[::]:16001", "upstream": "core1:16000", "enabled": true},
        { "name": "transaction_core_2", "listen": "[::]:26001", "upstream": "core2:26000", "enabled": true},
        { "name": "transaction_core_3", "listen": "[::]:36001", "upstream": "core3:36000", "enabled": true}
        ]'
```

# Update a proxy's fields

## Core 1
```bash
curl -X POST localhost:6666/proxies/transaction_core_1 -H "Content-Type: application/json" -d '{"listen": "[::]:16001", "upstream": "core1:16000", "enabled": false}'
```
## Core2
```bash
curl -X POST localhost:6666/proxies/transaction_core_2 -H "Content-Type: application/json" -d '{"listen": "[::]:26001", "upstream": "core2:26000", "enabled": false}'
```
## Core3
```bash
curl -X POST localhost:6666/proxies/transaction_core_3 -H "Content-Type: application/json" -d '{"listen": "[::]:36001", "upstream": "core2:36000", "enabled": false}'
```

# Delete an existing proxy

HTTP API:

```bash
curl -X DELETE localhost:6666/proxies/transaction_core_X -H "Content-Type: application/json"
```

CLI:

```bash
toxiproxy-cli -h localhost:6666 delete discovery_core_X
```

# Create a new toxic

## Latency

HTTP API:

```bash
curl -X POST localhost:6666/proxies/transaction_core_X/toxics -H "Content-Type: application/json" -d '{ "type": "latency", "attributes":{"latency":30000,"jitter":1000}, "stream":"upstream"}'
```

CLI:

```bash
toxiproxy-cli -h localhost:6666 toxic add -t latency -a latency=30000 -a jitter=1000 --upstream transaction_core_X
```

## Bandwidth

HTTP API:

```bash
curl -X POST localhost:6666/proxies/transaction_core_X/toxics -H "Content-Type: application/json" -d '{ "type": "bandwidth", "attributes":{"rate":10}, "stream":"upstream"}'
```

CLI:

```bash
toxiproxy-cli -h localhost:6666 toxic add -t bandwidth -a rate=50000 --upstream transaction_core_X
```

## Slow close

HTTP API:

```bash
curl -X POST localhost:6666/proxies/transaction_core_X/toxics -H "Content-Type: application/json" -d '{ "type": "slow_close", "attributes":{"delay":20000}, "stream":"upstream"}'
```

CLI:

```bash
toxiproxy-cli -h localhost:6666 toxic add -t slow_close -a delay=20000 --upstream transaction_core_X
```

## Timeout

HTTP API:

```bash
curl -X POST localhost:6666/proxies/transaction_core_X/toxics -H "Content-Type: application/json" -d '{ "type": "timeout", "attributes":{"timeout":10000}, "stream":"upstream"}'
```

CLI:

```bash
toxiproxy-cli -h localhost:6666 toxic add -t timeout -a timeout=10000 --upstream transaction_core_X
```

## Reset peer

HTTP API:

```bash
curl -X POST localhost:6666/proxies/transaction_core_X/toxics -H "Content-Type: application/json" -d '{ "type": "reset_peer", "attributes":{"timeout":1000}, "stream":"upstream"}'
```

CLI:

```bash
toxiproxy-cli -h localhost:6666 toxic add -t reset_peer -a timeout=10000 --upstream transaction_core_X
```

## Slicer

HTTP API:

```bash
curl -X POST localhost:6666/proxies/transaction_core_X/toxics -H "Content-Type: application/json" -d '{ "type": "slicer", "attributes":{"average_size":100, "size_variation":10,"delay":100}, "stream":"upstream"}'
```

CLI:

```bash
toxiproxy-cli -h localhost:6666 toxic add -t slicer -a average_size=100 -a size_variation=10 -a delay=100 --upstream transaction_core_X
```

## Limit data

HTTP API:

```bash
curl -X POST localhost:6666/proxies/transaction_core_X/toxics -H "Content-Type: application/json" -d '{ "type": "limit_data", "attributes":{"bytes":100}, "stream":"upstream"}'
```

CLI:

```bash
toxiproxy-cli -h localhost:6666 toxic add -t limit_data -a average_size=100 -a bytes=100 --upstream transaction_core_X
```

# Update an active toxic (except stream)

## Update toxic
## Latency

HTTP API:

```bash
curl -X POST localhost:6666/proxies/transaction_core_X/toxics/latency_upstream -H "Content-Type: application/json" -d '{ "type": "latency", "attributes":{"latency":20000,"jitter":800}}'
```

CLI:

```
toxiproxy-cli -h localhost:6666 toxic update -n latency_upstream  -a latency=50000 transaction_core_X
```

# Remove an active toxic

## CoreX

HTTP API:

```bash
curl -X DELETE localhost:6666/proxies/transaction_core_X/toxics/latency_upstream
```

CLI:
```bash
toxiproxy-cli -h localhost:6666 toxic delete -n latency_upstream transaction_core_X
```


# Enable all proxies and remove all active toxics
POST /reset

```bash
curl -X POST localhost:6666/reset
```
