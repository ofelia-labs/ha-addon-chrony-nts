# Chrony NTP Server (with NTS client)

This add-on serves **NTP** to your LAN and can **authenticate upstream time** using **Network Time Security (NTS)**.

- LAN clients talk plain NTP (UDP/123) to this add-on.
- The add-on, in turn, can fetch time securely from NTS-capable providers (e.g., `time.cloudflare.com`) over **TCP/4460** (for NTS-KE) and UDP/123.

---

## Installation (quick)

1. Add this repository to **Home Assistant → Settings → Add-ons → Add-on store → (⋮) → Repositories**:
   ```
   https://github.com/ofelia-labs/ha-addon-chrony-nts
   ```
2. Install **Chrony NTP Server (with NTS client)**.
3. Open the add-on → **Configuration** → set your options → **Start**.

---

## Configuration (reference)

**Example:**
```yaml
mode: server
ntp_pool: pool.ntp.org
ntp_server:
  - time.cloudflare.com
  - nts.time.nl
  - nts.netnod.se
allow: "192.168.0.0/16,10.0.0.0/8,fd00::/8"
set_system_clock: true
use_nts: true
local_fallback: false
extra_config: ""
```

**Options:**

- `mode`:
  - `server` → use `ntp_server` entries (explicit hosts)
  - `pool` → use `ntp_pool` only
- `ntp_server` / `ntp_pool`: Hostnames of upstream time sources. `iburst` is added automatically.  
  **Note:** Not all `pool.ntp.org` servers support NTS. Prefer specific NTS providers like `time.cloudflare.com`, `nts.time.nl`, `nts.netnod.se`.
- `use_nts`: `true` to authenticate upstream time (NTS client).  
  Requires **outbound TCP/4460** and **UDP/123** from the HA host.
- `allow`: Comma or space-separated networks the add-on will serve via NTP.  
  Examples: `"192.168.1.0/24,fd00::/64"` or `"all"` (serves everyone — **not recommended**).
- `set_system_clock`: `true` lets chronyd steer the host clock; `false` runs chronyd with `-x` (serve NTP only).
- `local_fallback`: `true` adds `local stratum 10` so the service will still answer if no upstream is available. Leave **false** unless you know you need it.
- `extra_config`: Appended **verbatim** to `chrony.conf` for advanced tuning.  
  Example (custom CA pinning for private NTS):
  ```
  ntstrustedcerts /ssl/my-ca.pem
  nosystemcert
  ```

---

## Verification

After enabling `use_nts: true`, give it ~30 seconds for the first NTS handshake.

**From HA’s SSH & Web Terminal:**
```bash
# Find the container name (varies by system)
docker ps --format '{{.Names}}' | grep chrony

# Show sources and which one is selected (*)
docker exec -it <addon_container_name> chronyc sources

# Show auth details; NTS sources should have non-zero KeyID/Type/KLen
docker exec -it <addon_container_name> chronyc -N authdata
```

**From a LAN client (optional):**
- Linux: `ntpdate -q <HA_IP>`  or  `sntp -d <HA_IP>`
- macOS: `sntp -sS <HA_IP>`
- Windows (admin): `w32tm /stripchart /computer:<HA_IP> /dataonly /samples:5`

---

## Troubleshooting

- **`authdata` shows zeros / NTS not active**
  - Confirm `use_nts: true`.
  - Ensure outbound **TCP/4460** (NTS-KE) and **UDP/123** are allowed from the HA host.
  - Try an explicit NTS provider (e.g., `time.cloudflare.com`) in `ntp_server`.
  - Check logs in the add-on panel for TLS or DNS errors.

- **Clients aren’t syncing**
  - Make sure `allow` includes your subnets (e.g., `192.168.0.0/16`).
  - Point clients (or your router/DHCP option 42) to the **HA IP** as NTP server.
  - Verify nothing else on your network is blocking inbound UDP/123 to HA.

- **Using IPv6 literals**
  - Use bracketed form in `ntp_server` if you include a port elsewhere (e.g., `[2001:db8::1]`).

---

## Security notes

- NTS protects **upstream** time with TLS-established cookies; your LAN still uses classic NTP (widely supported).
- `local_fallback` is **off** by default to avoid serving possibly-wrong time if cut off from upstream.
- Keep `allow` scoped to your LAN subnets unless you explicitly want to serve broader networks.

---

## Updating

- Update via the Add-on Store UI as usual.
- After updates, give chronyd ~30 seconds to re-handshake NTS and reselect sources.

---

## Credits & license

- Built on top of chrony’s native NTS capabilities.
- License: MIT (see `LICENSE`).
