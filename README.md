# pcap-analysis
# pcap-analysis

Network traffic analysis writeups: packet captures investigated in Wireshark, with findings documented the way a SOC analyst would report them.

Each writeup walks through a capture from first look to conclusion: what happened, how I found it, and which indicators matter.

## Writeups

| # | Writeup | Scenario | Key skills | Date |
|---|---------|----------|------------|------|
| 01 | [Tech Support Scam](writeups/) | Malvertising redirect chain leading to a fake Microsoft / Windows Defender alert page | DNS & HTTP analysis, redirect tracing, object export, sandboxing, IOC enrichment | Sep 2026 |

<!-- Add one row per writeup. Keep the newest at the bottom. -->

### Featured: Tech Support Scam

A Windows 7 host running Internet Explorer 11 loads a malicious script from a flagged IP, gets bounced through an ad click tracker via a 302 redirect, and lands on a scam page. The page fingerprints the browser, then serves a fake Windows Defender alert built from images plus an alarm sound. A final `security.php` endpoint loops 401 responses with a "suspicious activity" message to keep the victim panicked.

**IOCs**

| Type | Indicator | Role |
|------|-----------|------|
| IP | `134.249.116.78` | Served malicious `jquery.js` (11/89 on VirusTotal) |
| Domain | `clk.verblife-3.co` | Ad click tracker / redirect (302) |
| Domain | `sd5doozry8.com` | Flagged redirector (3/89 on VirusTotal) |
| Domain / IP | `site.topwebsite4.xyz` / `68.183.175.204` | Scam page server (`/pc-error-0xxxfrxx88/`) |
| Victim | `10.1.9.101` | Infected host (Windows 7, IE11) |

## Methodology

Every investigation follows the same workflow:

1. **Triage.** Capture properties, protocol hierarchy, conversations and endpoints to get the shape of the traffic.
2. **Hunt.** Filter for suspicious activity: unusual ports, DNS anomalies, cleartext credentials, beaconing intervals, file transfers.
3. **Dig in.** Follow TCP/HTTP streams, export objects, reconstruct what was sent and received.
4. **Extract IOCs.** IPs, domains, hashes, user agents, URLs.
5. **Report.** Timeline of events, findings, and recommended response actions.

## Tools

- **Wireshark** for capture analysis, stream following and object export
- **VirusTotal** for domain and IP reputation checks
- **ANY.RUN** for detonating exported files in a sandbox

## Writeup structure

Each writeup in `writeups/` follows the same sections: **Scenario → Investigation → Classification → Key Takeaways**, with Wireshark, VirusTotal and sandbox screenshots as evidence.

## A note on the captures

Some captures contain live malware traffic, so they are **not stored in this repo**. Each writeup links to the original source of the pcap instead. If you download one, open it in an isolated VM and never extract or run recovered files on your host machine.

## About me

Third-year cybersecurity student in Toronto, working toward a SOC analyst role.
<!-- Optional: link your LinkedIn or portfolio here -->
