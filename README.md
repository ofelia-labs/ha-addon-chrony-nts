# Home Assistant Add-on: Chrony (with NTS client)

Secure NTP for your LAN; optionally authenticates upstream time using Network Time Security (NTS).

- Add this repository to the Home Assistant Add-on Store.
- Install **“Chrony NTP Server (with NTS client)”**.
- Set `use_nts: true` and choose NTS-capable upstreams (e.g., `time.cloudflare.com`).
- Ensure outbound **TCP/4460** (NTS-KE) and **UDP/123** are allowed.

See `chrony/DOCS.md` for details.

> **Status:** _Pre-release (beta)_. Expect rapid iteration and possible breaking changes before `1.0.0`.

---

## Legal & Risk Disclaimer

- **No warranty.** Provided “AS IS” under the MIT License (see `LICENSE`) without express or implied warranties, including merchantability, fitness for a particular purpose, or non-infringement.
- **Pre-release.** This is **beta** software. Interfaces, defaults, and behavior may change; bugs may exist. **Use at your own risk.**
- **Critical systems.** Do **not** deploy in safety-critical or high-integrity environments (e.g., medical, industrial control, aviation, financial settlement). Validate configuration and monitor logs.
- **Network exposure.** Serving NTP on UDP/123 and using NTS upstream (TCP/4460) expose network surfaces. Scope `allow` to your LAN; avoid public exposure unless you fully understand the implications and have DDoS controls.
- **Compliance.** NTS uses cryptography. You are responsible for applicable laws/regulations (import/export controls, sector rules, internal policies).
- **Third-party services.** Examples like `time.cloudflare.com`, `nts.time.nl`, `nts.netnod.se` are provided as references only; evaluate suitability and terms yourself.
- **Telemetry.** This add-on does **not** collect telemetry. Operational logs remain on your Home Assistant host.
- **Vulnerability reporting.** Use **GitHub → Security → “Report a vulnerability”** for private disclosures, or open an issue for non-sensitive bugs.
