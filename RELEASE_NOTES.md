## chrony-nts 1.0.0-beta.6

- Fix s6 start error by relying on base image's default `/init` entrypoint.

## chrony-nts 1.0.0-beta.5

- Fix startup failure caused by outdated `/command/with-contenv` path.

## chrony-nts 1.0.0-beta.1

**Pre-release (beta)**

- Initial beta release of Chrony add-on with **NTS client** support (RFC 8915).
- Hardened config generation, conservative defaults, and minimal docs.
- LAN clients use classic NTP; upstream can be authenticated via NTS (e.g., `time.cloudflare.com`).
- Expect rapid iteration; interfaces may change before 1.0.0.

**Verification**
- In the add-on container: `chronyc sources` and `chronyc -N authdata` (look for non-zero KeyIDs).
- Ensure outbound **TCP/4460** and **UDP/123** from HA host.

**Security & risk**
- Pre-release; use at your own risk. Not for safety-critical systems. See README disclaimer.
