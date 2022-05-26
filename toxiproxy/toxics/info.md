# List existing proxies and their toxics

HTTP API: `curl localhost:6666/proxies -H "Content-Type: application/json" | jq .`
CLI: `toxiproxy-cli -h localhost:6666 list`

# Show the proxy with all its active toxics

HTTP API: `curl localhost:6666/proxies/raft_core_1 -H "Content-Type: application/json" | jq .`
CLI: `toxiproxy-cli -h localhost:6666 inspect raft_core_1`


# List active toxics

HTTP API: `curl localhost:6666/proxies/raft_core_1/toxics -H "Content-Type: application/json" | jq .`
CLI: `toxiproxy-cli -h localhost:6666 inspect raft_core_1`

# Get an active toxic's fields

HTTP API: `curl localhost:6666/proxies/raft_core_1/toxics -H "Content-Type: application/json" | jq .`
CLI: `toxiproxy-cli -h localhost:6666 inspect raft_core_1`

# Returns the server version number

HTTP API: `curl localhost:6666/version -H "Content-Type: application/json"`
CLI: `toxiproxy-cli -h localhost:6666 --version`

# Returns Prometheus-compatible metrics

HTTP API: `curl localhost:6666/metrics -H "Content-Type: application/json"`