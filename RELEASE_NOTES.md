## chrony-nts (Pre-release)

- Ensure s6-overlay is PID 1 (CMD ["/init"]) to fix suexec error.
- Normalize init script shebangs and executable bits.
- No functional changes beyond startup reliability hardening.

Verify after install:
- Logs start cleanly (no "s6-overlay-suexec: fatal: can only run as pid 1").
- `chronyc -N sources` and `chronyc -N authdata` show NTS with non-zero KeyID.
