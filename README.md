# pogocache-docker

Unofficial Docker image for [pogocache](https://github.com/pogocache/pogocache) - a high-performance in-memory cache server.

## Features

- Multi-architecture support (amd64, arm64)
- Environment variable-based arguments
- Both Alpine and Debian base images
- Kubernetes deployment examples included
- Automated builds via GitHub Actions

## Quick Start

### Docker Run

```bash
# Basic usage with default settings
docker run -p 9401:9401 ghcr.io/franzramadhan/pogocache:alpine-latest

# With custom configuration via environment variables
docker run -p 9401:9401 \
  -e POGOCACHE_PORT=9401 \
  -e POGOCACHE_THREADS=16 \
  -e POGOCACHE_MAXMEMORY=4g \
  ghcr.io/franzramadhan/pogocache:alpine-latest
```

### Docker Compose

```yaml
version: '3.8'
services:
  pogocache:
    image: ghcr.io/franzramadhan/pogocache:alpine-latest
    ports:
      - "9401:9401"
    environment:
      POGOCACHE_HOST: 0.0.0.0
      POGOCACHE_PORT: 9401
      POGOCACHE_THREADS: 32
      POGOCACHE_MAXMEMORY: 2g
```

## Environment Variable Configuration

**Key Feature:** This Docker image allows you to configure each pogocache arguments through environment variables. The entrypoint script automatically converts these variables to command-line arguments.

### Basic Options

| Environment Variable | Description | Default |
|---------------------|-------------|---------|
| `POGOCACHE_HOST` | Listening host | `0.0.0.0` |
| `POGOCACHE_PORT` | Listening port | `9401` |
| `POGOCACHE_SOCKET` | Unix socket file | none |
| `POGOCACHE_VERBOSE` | Verbose logging (`-v`, `-vv`, `-vvv`) | none |

### Additional Options

| Environment Variable | Description | Default |
|---------------------|-------------|---------|
| `POGOCACHE_THREADS` | Number of threads | `32` |
| `POGOCACHE_MAXMEMORY` | Max memory usage (e.g., `2g`, `512m`) | `80%` |
| `POGOCACHE_EVICT` | Evict keys at maxmemory (`yes`/`no`) | `yes` |
| `POGOCACHE_PERSIST` | Persistence file path | none |
| `POGOCACHE_MAXCONNS` | Maximum connections | `1024` |

### Security Options

| Environment Variable | Description | Default |
|---------------------|-------------|---------|
| `POGOCACHE_AUTH` | Auth token or password | none |
| `POGOCACHE_TLSPORT` | Enable TLS on port | none |
| `POGOCACHE_TLSCERT` | TLS certificate file | none |
| `POGOCACHE_TLSKEY` | TLS key file | none |
| `POGOCACHE_TLSCACERT` | TLS CA certificate file | none |

### Advanced Options

| Environment Variable | Description | Default |
|---------------------|-------------|---------|
| `POGOCACHE_SHARDS` | Number of shards | `4096` |
| `POGOCACHE_BACKLOG` | Accept backlog | `1024` |
| `POGOCACHE_QUEUESIZE` | Event queue size | `128` |
| `POGOCACHE_REUSEPORT` | Reuseport for TCP (`yes`/`no`) | `no` |
| `POGOCACHE_TCPNODELAY` | Disable Nagle's algorithm (`yes`/`no`) | `yes` |
| `POGOCACHE_QUICKACK` | Use quickack - Linux (`yes`/`no`) | `no` |
| `POGOCACHE_URING` | Use io_uring - Linux (`yes`/`no`) | `yes` |
| `POGOCACHE_LOADFACTOR` | Hashmap load factor (percent) | `75` |
| `POGOCACHE_AUTOSWEEP` | Automatic eviction sweeps (`yes`/`no`) | `yes` |
| `POGOCACHE_KEYSIXPACK` | Sixpack compress keys (`yes`/`no`) | `yes` |
| `POGOCACHE_CAS` | Use compare and store (`yes`/`no`) | `no` |

## Available Images

Images are available in both Alpine and Debian variants:

Registry URL: [pogocache](https://github.com/franzramadhan/pogocache-docker/pkgs/container/pogocache)

```bash
# Alpine (smaller, recommended)
ghcr.io/franzramadhan/pogocache:alpine-latest
ghcr.io/franzramadhan/pogocache:alpine-<version>

# Debian (more compatibility)
ghcr.io/franzramadhan/pogocache:debian-latest
ghcr.io/franzramadhan/pogocache:debian-<version>
```

## Kubernetes Deployment

See the [kubernetes/](kubernetes/) directory for deployment examples:

```bash
kubectl create namespace pogocache
kubectl apply -f kubernetes/manifest.yaml -n pogocache --server-side
```

## Project Structure

```markdown
pogocache-docker/
├── Dockerfile.alpine       # Alpine-based image (musl)
├── Dockerfile.debian        # Debian-based image (glibc)
├── entrypoint              # Startup script with env var support
├── pogocache.help          # Command reference
├── kubernetes/             # Kubernetes deployment examples
│   ├── manifest.yaml
│   └── README.md
└── .github/workflows/      # Automated build pipeline
    └── build.yaml
```

## Building Images

Images are automatically built and published via GitHub Actions when tags are pushed. To build manually:

```bash
# Download pogocache binaries first
# Then build Alpine image
docker build -f Dockerfile.alpine -t pogocache:alpine .

# Or Debian image
docker build -f Dockerfile.debian -t pogocache:debian .
```

## License

This project is licensed under the Apache License - see the [LICENSE](LICENSE) file for details.

## Related Projects

- [pogocache](https://github.com/pogocache/pogocache) - The upstream pogocache project

## TODO

- [ ] Add workflow to deploy to cluster
- [ ] Automated workflow that subscribe to the new tag releases in upstream
- [ ] More flexible arch and platform based on the available artifacts in upstream

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
