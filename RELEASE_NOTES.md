## chrony-nts 1.0.0-beta.8 (Pre-release)

- Fix startup reliability: ensure s6-overlay runs as PID 1 (`CMD ["/init"]`).
- Normalize init script shebangs to `#!/usr/bin/with-contenv bashio` and enforce executable bits.
- Alpine base with `chrony` and `ca-certificates`.
- No functional changes beyond startup reliability.

**Verify after install**
- Add-on logs start cleanly (no “s6-overlay-suexec: fatal: can only run as pid 1”).
- `chronyc -N sources` and `chronyc -N authdata` show NTS with non-zero KeyID.
