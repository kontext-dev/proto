# Kontext Protocol Buffers

Shared contract between the [Kontext CLI](https://github.com/kontext-dev/kontext-cli) and the Kontext API.

## Services

### `kontext.agent.v1.AgentService`

The core governance service. Handles session lifecycle, hook event streaming, credential exchange, and policy sync.

| RPC | Type | Purpose |
|---|---|---|
| `CreateSession` | Unary | Start a governed agent session |
| `ProcessHookEvent` | Unary | Ingest a tool call event |
| `Heartbeat` | Unary | Keep session alive |
| `EndSession` | Unary | Terminate session |
| `ExchangeCredential` | Unary | Resolve provider credentials |
| `SyncPolicy` | Server stream | Push policy updates for local evaluation |

## Usage

### Go (CLI)

```yaml
# buf.gen.yaml
version: v2
deps:
  - buf.build/kontext/proto
plugins:
  - local: protoc-gen-go
    out: gen
    opt: paths=source_relative
  - local: protoc-gen-connect-go
    out: gen
    opt: paths=source_relative
```

### TypeScript (API)

```yaml
# buf.gen.yaml
version: v2
deps:
  - buf.build/kontext/proto
plugins:
  - local: protoc-gen-es
    out: gen
    opt: target=ts
  - local: protoc-gen-connect-es
    out: gen
    opt: target=ts
```

## CI

Every PR runs:
- `buf lint` — proto style enforcement
- `buf breaking --against .git#branch=main` — backward compatibility check

Consumer repos (CLI, API) run `buf breaking` against this repo's `main` branch to catch incompatible changes before merge.

## Development

```bash
# Lint
buf lint

# Check breaking changes
buf breaking --against .git#branch=main

# Format
buf format -w
```
