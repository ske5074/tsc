# Transport Speed Calculator

Estimates how long it takes to move data over a given transport and distance, from a punch card by Pony Express to an exabyte over 400 Gigabit Ethernet.

Live: https://www.kenops.io/tsc

Single static `index.html` with no server and no dependencies. Open it in a browser or host it anywhere.

## Features
- Data units from nibbles to exabytes (binary and decimal), plus media like floppies, DVDs and the Apollo Flight Computer
- Ethernet (10 Mb to 400 Gb), Fibre Channel (1G to 64G) and historical transports
- Distance in miles or kilometers
- Latency effects: TCP window, packet loss (Mathis formula) and slow start for Ethernet; buffer credits for Fibre Channel
- Human-readable results, e.g. "3 days, 4 hours, 12 minutes"

## Configuration
All data is in the `<script>` block of `index.html`. Values use mebibytes (MiB, 2^20 bytes) as the reference and are written as formulas.

Add a data unit:
```js
{ name: "LTO-9", desc: "Tape (18TB)", mebibytes: 18e12 / MiB }
```

Add a transport:
```js
{ name: "800 Gigabit Ethernet", mebibytesPerSecond: 800e9 / 8 / MiB, ...TCP }
```

Set `default: true` on an entry to preselect it.

Tuning:
| Setting | Default | Effect |
|---|---|---|
| `net.lossRate` | `1e-7` | Higher packet loss slows long-distance TCP |
| `net.streams` | `1` | Parallel streams multiply throughput limits |
| `net.mssBytes` | `1460` | TCP segment size |
| `TCP.windowMiB` | `16` | Max TCP window |
| `FC.windowMiB` | 500 credits × 2112 B | Fibre Channel buffer credits |
| `FIBER` | `8.2e-6` | Propagation delay in seconds per mile |
