# Podman Stats Receiver

The Podman Stats receiver queries the Podman service API to fetch stats for all running containers 
on a configured interval.  These stats are for container
resource usage of cpu, memory, network, and the
[blkio controller](https://www.kernel.org/doc/Documentation/cgroup-v1/blkio-controller.txt).

Supported pipeline types: metrics

> :information_source: Requires Podman API version 3.3.1+ and Windows is not supported.

## Configuration

The following settings are required:

- `endpoint` (default = `unix:///run/podman/podman.sock`): Address to reach the desired Podman daemon.

The following settings are optional:

- `collection_interval` (default = `10s`): The interval at which to gather container stats.

Example:

```yaml
receivers:
  podman_stats:
    endpoint: http://example.com/
    collection_interval: 10s
```

The full list of settings exposed for this receiver are documented [here](./config.go)
with detailed sample configurations [here](./testdata/config.yaml).

## Metrics

The receiver emits the following metrics:

	container.memory.usage.limit
	container.memory.usage.total
	container.memory.percent
	container.network.io.usage.tx_bytes
	container.network.io.usage.rx_bytes
	container.blockio.io_service_bytes_recursive.write
	container.blockio.io_service_bytes_recursive.read
	container.cpu.usage.system
	container.cpu.usage.total
	container.cpu.percent
	container.cpu.usage.percpu

## Building

This receiver uses the official libpod Go bindings for Podman. In order to include
this receiver in your build, you'll need to make sure all non-Go dependencies are
satisfied or some features are exluded. You can use the below mentioned build tags to
exclude the non-Go dependencies. This receiver does not use any features enabled
by these deps so excluding these does not affect the functionality in any way.

Recommended build tags to use when including this receiver in your build:

- `containers_image_openpgp`
- `exclude_graphdriver_btrfs`
- `exclude_graphdriver_devicemapper`