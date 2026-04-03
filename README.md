# AI Progress Tracker — TurboNet

Shared memory for all AI agents working on this project. Every session reads this before making changes.

## Current System State (as of 2026-04-03)

| Component | Status | Notes |
|---|---|---|
| Backend (PM2) | ✅ Online | 54m+ uptime, no crash loop |
| MySQL `turbonet` user | ✅ Fixed | Password reset, all tables accessible |
| FreeRADIUS | ✅ Active | Listening on 0.0.0.0:1812/1813 |
| WireGuard tunnel | ❌ DOWN | Handshake fails continuously on MikroTik |
| FRP tunnel | ❌ DEAD | No `frps` process running on VPS |
| MikroTik RADIUS entries | ⚠️ BROKEN | 2 of 3 entries point to dead WireGuard (10.10.10.1) |
| VPS Public IP | `136.109.224.75` | Also `136.117.23.173` in MikroTik config |
| MikroTik RADIUS IP | `136.117.23.173` | Working entry (index 0) |
| DuckDNS | `turbowifi.duckdns.org` | Dynamic DNS for callback URL |
| M-Pesa | Sandbox mode | `MPESA_SHORTCODE=174379` |
| Plans | 6 loaded | 6hrs(KES20) to 1Month(KES1000) |

## Active Issues

### Issue #1: MikroTik RADIUS Dead Entries
- **Problem:** Entries at index 1 and 2 point to `10.10.10.1` (dead WireGuard)
- **Fix:** Run on MikroTik: `/radius remove 1` twice
- **Status:** ⏳ Pending — needs to be run in WinBox/SSH terminal
- **Impact:** Phones connect to WiFi but never see captive portal (RADIUS auth fails)

### Issue #2: WireGuard Tunnel Broken
- **Problem:** MikroTik peer handshake fails every 2 minutes since boot
- **VPS side:** `wg-mikrotik` interface exists, port 51820 listening, peer key `dblysq...`
- **MikroTik side:** Peer key `p+DET4...`, endpoint may be wrong or key mismatch
- **Status:** 🔴 Not working
- **Impact:** No private tunnel between MikroTik and VPS

### Issue #3: FRP Server Not Running
- **Problem:** No `frps` process on VPS, port 7000 open in UFW but nothing listening
- **Config:** `/home/vin/frp/frps.toml` exists with `bindPort = 7000`
- **Status:** 🔴 Not running
- **Impact:** No reverse tunnel for exposing internal services

## Next Steps (Priority Order)
1. [ ] Fix MikroTik RADIUS entries (run `/radius remove 1` twice on MikroTik)
2. [ ] Test captive portal flow — phone should see payment page after WiFi connect
3. [ ] Harden UFW — restrict port 1812/udp to MikroTik public IP only
4. [ ] Decide: rebuild WireGuard tunnel or keep public IP RADIUS
5. [ ] Start FRP server if still needed

---

## AI Session Log

| Date | Agent | Action | Commit | Notes |
|---|---|---|---|---|
| 2026-04-03 | Qwen | DB fix + logx | `8ec9718` | Reset turbonet MySQL password, created logx logging script |
| 2026-04-03 | Qwen | ai_tracker setup | TBD | Created this progress tracker, pushed to GitHub |
