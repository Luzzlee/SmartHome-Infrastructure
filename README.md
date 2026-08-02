# SmartHome-Infrastructure

Docker Compose setup for the MQTT broker (`eclipse-mosquitto:2`) shared by [SmartHome-Backend](https://github.com/Luzzlee/SmartHome-Backend) and the physical devices in [SmartHome-Sketchbook](https://github.com/Luzzlee/SmartHome-Sketchbook).

Part of the multi-repo [SmartHome](https://github.com/Luzzlee/SmartHome-Backend) home-automation project — see also [SmartHome-Svelte-Frontend](https://github.com/Luzzlee/SmartHome-Svelte-Frontend) (dashboard UI).

## Getting started

```bash
docker compose up -d
```

Broker listens on port `1883` (plain TCP, no TLS/websockets). Auth is required (`allow_anonymous false`); the known username is `smarthome` and its password lives as a hash in `mosquitto/pwfile`, rotated with `mosquitto_passwd`.

Rotating the password requires updating it in two other places too — the Backend's `Mqtt:Password` and any physical device's `arduino_secrets.h` — see [`CLAUDE.md`](./CLAUDE.md) for the full procedure and current known gaps (e.g. only the broker is containerized here; the Backend runs outside Docker).
