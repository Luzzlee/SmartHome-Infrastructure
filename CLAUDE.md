# SmartHome-Infrastructure

Docker Compose setup for the MQTT broker (`eclipse-mosquitto:2`) shared by `SmartHome-Backend` and the physical devices in `SmartHome-Sketchbook`.

See also: [top-level `D:\Git\CLAUDE.md`](../CLAUDE.md) for how this fits with the other SmartHome repos.

## Commands

```bash
docker compose up -d
```

Broker listens on port `1883` (plain TCP, no TLS/websockets). Config: `mosquitto/mosquitto.conf` (`allow_anonymous false`, auth via `mosquitto/pwfile`). Known username: `smarthome`.

## Rotating the broker password

The password lives in `mosquitto/pwfile` as a hash, managed with `mosquitto_passwd`:

```bash
mosquitto_passwd -c mosquitto/pwfile smarthome
```

If you rotate it, two things need updating to match, both **manual, outside of what any automated cleanup can do**:
1. `SmartHome-Backend`'s `Mqtt:Password` (set via `dotnet user-secrets` or the `Mqtt__Password` env var — see that repo's CLAUDE.md).
2. Any physical ESP32 devices from `SmartHome-Sketchbook` — their `arduino_secrets.h` (gitignored, local to each dev machine) needs the new password and the device needs reflashing.

## CI

`.github/workflows/ci.yml` runs on every push and pull request: `docker compose config -q` validates the syntax and schema of `docker-compose.yaml`. This does not start the container or test connectivity to the broker, and does not check the contents of `mosquitto.conf`.

## Known issues / not yet done

- Only the MQTT broker is containerized here — the Backend and its SQLite DB run outside Docker.
