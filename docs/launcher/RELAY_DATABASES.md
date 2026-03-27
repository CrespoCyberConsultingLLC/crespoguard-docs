
# Relay Database Files

> The relay uses CSV database files for GeoIP and ASN filtering. These files are not included with the relay — you must download or generate them yourself.

## GeoIP Database (`geoip.csv`)

Used when `GeoIPEnabled` is `true`. Maps IP ranges to 2-letter country codes for geographic filtering.

### Format

```csv
# start_ip_decimal,end_ip_decimal,country_code
16777216,16777471,AU
16777472,16777727,CN
16842752,16843007,JP
17039360,17039615,US
```

- `start_ip_decimal` — First IP in the range, converted to a 32-bit unsigned integer
- `end_ip_decimal` — Last IP in the range, converted to a 32-bit unsigned integer
- `country_code` — 2-letter ISO 3166-1 alpha-2 code (uppercase)
- Lines starting with `#` are comments
- The file must be sorted by `start_ip_decimal` (ascending)

### IP Decimal Conversion

To convert an IP address to its decimal form: `A.B.C.D → (A × 16777216) + (B × 65536) + (C × 256) + D`

Example: `1.0.0.0` → `(1 × 16777216) + (0 × 65536) + (0 × 256) + 0` = `16777216`

### Where to Get It

**MaxMind GeoLite2** (free, requires account):

1. Create a free account at [maxmind.com](https://www.maxmind.com/en/geolite2/signup)
2. Download `GeoLite2-Country-CSV` from your account dashboard
3. Use the `GeoLite2-Country-Blocks-IPv4.csv` file
4. Convert the `network` CIDR column to start/end decimal ranges
5. Join with `GeoLite2-Country-Locations-en.csv` to get country codes

**Alternative: DB-IP Lite** (free, no account):

1. Download from [db-ip.com/db/lite](https://db-ip.com/db/download/ip-to-country-lite)
2. Extract and convert to the CSV format above

### Example Config

```json
{
    "GeoIPEnabled": true,
    "GeoIPFile": "geoip.csv",
    "GeoIPAllowedCountries": ["US", "BR", "PH", "ID", "MY", "TH", "VN"]
}
```

Only players from the listed countries can connect. All other countries are blocked. Private/loopback IPs (127.x, 10.x, 192.168.x, 172.16-31.x) always bypass GeoIP filtering.

---

## ASN Database (`asn.csv`)

Used when `ASNBlockEnabled` is `true`. Maps IP ranges to Autonomous System Numbers (ASNs) for blocking datacenter, VPN, and proxy traffic.

### Format

```csv
# start_ip_decimal,end_ip_decimal,asn_number,organization
16777216,16777471,13335,Cloudflare Inc
16777472,16777727,15169,Google LLC
16842752,16843007,14061,DigitalOcean LLC
17039360,17039615,16276,OVH SAS
17301504,17301759,63949,Akamai Connected Cloud
17367040,17367295,396982,Google Cloud
```

- `start_ip_decimal` / `end_ip_decimal` — Same format as GeoIP
- `asn_number` — The Autonomous System Number (integer)
- `organization` — Organization name (optional, for reference)
- Lines starting with `#` are comments
- The file must be sorted by `start_ip_decimal` (ascending)

### Where to Get It

**IPtoASN** (free, no account):

1. Download from [iptoasn.com](https://iptoasn.com/) — click "IPv4 TSV"
2. Convert the TSV to CSV format (fields: start, end, ASN, country, org)
3. IP ranges are already in decimal format

**RIPE NCC / BGP data:**

1. Download from [stat.ripe.net](https://stat.ripe.net/data/announced-prefixes/data.json)
2. Or use [bgp.he.net](https://bgp.he.net/) for ASN lookups

### Common ASNs to Block

| ASN | Provider | Why Block |
|-----|----------|-----------|
| 14061 | DigitalOcean | VPS/cloud — not residential |
| 16276 | OVH | VPS/hosting — not residential |
| 63949 | Akamai/Linode | Cloud hosting |
| 396982 | Google Cloud | Cloud hosting |
| 8075 | Microsoft Azure | Cloud hosting |
| 16509 | Amazon AWS | Cloud hosting |
| 13335 | Cloudflare | CDN/proxy |
| 20473 | Vultr | VPS hosting |
| 46606 | Unified Layer | Hosting |
| 14618 | Amazon | AWS US-East |

### Example Config

```json
{
    "ASNBlockEnabled": true,
    "ASNBlockFile": "asn.csv",
    "ASNBlockList": [14061, 16276, 63949, 396982, 8075, 16509, 20473],
    "ASNAllowList": [12345]
}
```

- `ASNBlockList` — Block connections from these ASNs
- `ASNAllowList` — Always allow these ASNs (overrides block list)

Players connecting from residential ISPs are not affected. Only IPs belonging to the listed datacenter/VPN ASNs are blocked.

---

## Threat Intelligence

Used when `ThreatIntelEnabled` is `true`. Downloads IP blocklists from remote URLs on startup and refreshes periodically.

### Supported Formats

**Format 1: IP + Score (tab-separated)**
```
# IPsum threat intelligence feed
1.2.3.4	5
5.6.7.8	3
9.10.11.12	2
```

**Format 2: Plain IP (one per line)**
```
# blocklist.de / Tor exit nodes
1.2.3.4
5.6.7.8
9.10.11.12
```

- Lines starting with `#` or empty lines are ignored
- Score-based lists use the `ThreatIntelMinScore` threshold (default: 2)

### Default Sources

The relay ships with three default threat intel sources:

| Source | Content | Update Frequency |
|--------|---------|-----------------|
| [IPsum](https://raw.githubusercontent.com/stamparm/ipsum/master/ipsum.txt) | Aggregated threat feeds, scored 1-10 | Daily |
| [blocklist.de](https://lists.blocklist.de/lists/all.txt) | Active attack IPs | Hourly |
| [Tor Exit Nodes](https://check.torproject.org/torbulkexitlist) | Known Tor exit nodes | Hourly |

### Example Config

```json
{
    "ThreatIntelEnabled": true,
    "ThreatIntelURLs": [
        "https://raw.githubusercontent.com/stamparm/ipsum/master/ipsum.txt",
        "https://lists.blocklist.de/lists/all.txt",
        "https://check.torproject.org/torbulkexitlist"
    ],
    "ThreatIntelRefreshHours": 24,
    "ThreatIntelMinScore": 2
}
```

- `ThreatIntelRefreshHours` — How often to re-download lists (default: 24 hours)
- `ThreatIntelMinScore` — Minimum score for IPsum-style lists (IPs scoring below this are ignored)

Add your own blocklist URLs to `ThreatIntelURLs`. Any URL serving one-IP-per-line text works.

### How It Works

1. On startup, the relay downloads all configured URLs
2. IPs are loaded into memory for O(1) lookup
3. Every `ThreatIntelRefreshHours`, the relay re-downloads all lists
4. If a download fails, the relay keeps the previous data (fails open on first run)
5. Private/loopback IPs always bypass threat intel checks

---

## File Locations

All database files should be placed in the relay's working directory (same folder as `server.json`):

```
CrespoGuard/
├── CrespoGuardRelay.exe
├── server.json
├── geoip.csv          ← GeoIP database (you provide)
├── asn.csv            ← ASN database (you provide)
├── bans.json          ← Auto-created by dashboard
├── ipbans.json        ← Auto-created by dashboard
└── routes.json        ← Auto-created for edge relay
```

`bans.json`, `ipbans.json`, and `routes.json` are created automatically by the relay. You only need to provide `geoip.csv` and `asn.csv` if you enable those features.
