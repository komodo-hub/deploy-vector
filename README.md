# Deploy Vector Docker Logs Collector

Part of the [Komodo Hub collection.](https://github.com/komodo-hub/komodo-hub)

Runs Vector as a [docker logs collector](https://vector.dev/docs/reference/configuration/sources/docker_logs/), pointing to a Grafana Loki endpoint.

https://hub.docker.com/r/timberio/vector

## Parsers

With this configuration, Vector picks up docker logs only for containers given the `vector.parser` label. Using this label, you choose which log parser is used.

### Parser Options

- `vector.parser=basic`
  - Minimal parsing. Compatible with any logging format
- `vector.parser=key-value`
  - Parses logs formatted as `key=value` pairs.
- `vector.parser=json`
  - Parses logs formatted as `JSON`. Ideally the json has a `level`
		field containing the logging level of the message, eg. INFO / ERROR.
- `vector.parser=rust`
	- Parses logs produced by the rust `log` or `tracing-subscriber` crates.
- `vector.parser=mongo`
  - Parses logs produced by Mongo deployments.

## Komodo Resource TOML

```toml
[[stack]]
name = "vector"
[stack.config]
repo = "komodo-hub/deploy-vector"
environment = """
  ## Route alerts to Loki at this endpoint
  LOKI_ENDPOINT = http://loki:3100
  ## Attach to custom network to reach Loki.
  NETWORK_MODE = bridge
  ## https://hub.docker.com/r/timberio/vector
  VECTOR_TAG = latest-debian
  ## https://docs.docker.com/engine/logging/configure
  LOGGING_DRIVER = local
"""
```