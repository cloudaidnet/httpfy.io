# HTTPfy MCP server

Free, read-only, no-authentication remote [MCP](https://modelcontextprotocol.io) server for DNS, email authentication, TLS certificates, WHOIS, IP/ASN and network diagnostics, run from [httpfy.io](https://httpfy.io) servers.

- **Endpoint:** `https://httpfy.io/mcp` (Streamable HTTP, protocol 2026-07-28 and 2025-03-26 to 2025-11-25)
- **Docs and one-prompt setup:** https://httpfy.io/agents
- **Server card:** https://httpfy.io/.well-known/mcp/server-card.json
- **No account, no API key.** Limits per client per hour: 100 lookups, 30 active checks, 10 probes ([details](https://httpfy.io/agents#limits)).
- Results are licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/); every result includes a `report_url` to the full report.

This repository holds the manifests that let agent clients and directories install the server. The server itself runs at httpfy.io.

## Install

| Client | How |
|---|---|
| Claude Code | `claude mcp add --transport http --scope user httpfy https://httpfy.io/mcp` or `/plugin install httpfy@cloudaidnet/httpfy.io` |
| Claude.ai / Desktop | [Add custom connector](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=HTTPfy&connectorUrl=https%3A%2F%2Fhttpfy.io%2Fmcp) |
| Cursor | [![Install in Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=httpfy&config=eyJ1cmwiOiJodHRwczovL2h0dHBmeS5pby9tY3AifQ==) |
| VS Code | [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_httpfy_MCP-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect/mcp/install?name=httpfy&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fhttpfy.io%2Fmcp%22%7D) or `code --add-mcp '{"name":"httpfy","type":"http","url":"https://httpfy.io/mcp"}'` |
| Codex | `codex mcp add httpfy --url https://httpfy.io/mcp` |
| Gemini CLI | `gemini extensions install https://github.com/cloudaidnet/httpfy.io` or `gemini mcp add --scope user --transport http httpfy https://httpfy.io/mcp` |
| Kiro | [Add to Kiro](https://kiro.dev/launch/mcp/add?name=httpfy&config=%7B%22url%22%3A%22https%3A%2F%2Fhttpfy.io%2Fmcp%22%7D) |
| Goose | `goose://extension?url=https://httpfy.io/mcp&type=streamable_http` |
| Any other MCP client | Add an HTTP server named `httpfy` with URL `https://httpfy.io/mcp`, no auth. Config samples: [`mcp.json`](mcp.json), [`.mcp.json`](.mcp.json) |

## Tools

- `dns_lookup` (domain, record_type): Look up DNS records of one type (A, AAAA, CNAME, MX, NS, SOA, TXT, SRV) for a domain using public resolvers.
- `dns_propagation_check` (domain_name, query_type): Query a domain on about 40 public resolvers in different regions and report which ones return which answers, to see whether a DNS change has propagated.
- `dnssec_check` (domain): Validate the DNSSEC chain of trust for a domain from the root down (DS, DNSKEY, RRSIG) and report each zone as secure, insecure or broken.
- `mx_lookup` (domain): List the mail exchanger (MX) records of a domain with their preferences.
- `spf_lookup` (domain): Fetch the SPF record of a domain from its authoritative name server and list its mechanisms (ip4, ip6, include, redirect, all).
- `spf_validate` (domain): Validate the SPF record of a domain: syntax, single-record rule, DNS lookup count (limit 10), void lookups, includes and the all mechanism.
- `domain_whois` (domain): WHOIS registration data for a domain: registrar, dates, status and name servers (personal contact data is redacted by registries).
- `domain_expiry` (domain_name): Registration and expiry dates of a domain, with the time remaining before it expires.
- `ssl_certificate_check` (domain): Connect to a domain on port 443 and report the certificate chain: subject, issuer, validity dates, days remaining, SANs and chain details.
- `ping` (host): Ping a public host from httpfy servers (4 ICMP echo requests) and report packet loss and round-trip times. Useful to test reachability from outside your own network.
- `traceroute` (host): Traceroute to a public host from httpfy servers (up to 10 hops). Internal hops of httpfy's own network are masked.
- `ip_whois` (ip_address): WHOIS / registry data for a public IPv4 address: ASN, network range, owner organisation and registry.
- `ip_blacklist_check` (ip_address): Check whether a public IPv4 address is listed on common DNS-based blocklists such as Spamhaus, Barracuda and SpamCop.
- `mac_vendor_lookup` (mac_address): Identify the manufacturer registered for a MAC address prefix (OUI) using the IEEE registry.
- `asn_lookup` (query): Look up the autonomous system (ASN) for a public IP address, or details for an ASN such as AS15169: name, country, announced prefixes and address counts.
- `bimi_check` (domain): Check the BIMI record of a domain, whether DMARC meets BIMI requirements, and whether the SVG logo and certificate URLs are reachable and well formed.
- `caa_check` (domain): Find the CAA records that apply to a domain (climbing to parent domains per RFC 8659) and list which certificate authorities may issue certificates.
- `certificate_transparency_search` (domain): List certificates logged in Certificate Transparency for a domain (via crt.sh): issuer, validity window and names, with a summary by issuer.
- `dkim_check` (domain, selector): Check DKIM public keys for a domain. With a selector it checks that selector; without one it probes common selectors. Reports key type and RSA key length and flags weak or revoked keys.
- `dmarc_check` (domain): Find and explain the DMARC record of a domain (policy, pct, rua/ruf, alignment) and flag weaknesses such as p=none or missing external report authorization.
- `dns_report` (domain): Full DNS report for a domain: delegation from the root, every authoritative name server checked individually (reachability, recursion, TCP, consistency), SOA values, MX records and their reverse DNS, SPF, DMARC, DKIM and DNSSEC.
- `domain_health_report` (domain): Grade a domain A to F across 11 checks (name servers, SOA, DNSSEC, MX, SPF, DMARC, CAA, HTTPS certificate, HTTP to HTTPS redirect, HSTS, IPv6), with a reason and fix for each failed check.
- `mta_sts_check` (domain): Check MTA-STS (TXT record and the HTTPS policy file, mode and MX patterns) and the TLS-RPT reporting record for a domain.
- `port_check` (host, port): Check whether one TCP port on a public host is open, closed or filtered when tested from httpfy servers on the internet.

The [Agent Skill](skills/httpfy-network-checks/SKILL.md) explains which tool answers which question. Download it as a zip from https://httpfy.io/agents/httpfy-network-checks.zip.

## Repository layout

| Path | Used by |
|---|---|
| `server.json` | Official MCP Registry entry (`io.httpfy/httpfy`) |
| `.claude-plugin/plugin.json`, `.mcp.json`, `skills/` | Claude Code / Claude.ai plugin |
| `.cursor-plugin/plugin.json`, `mcp.json` | Cursor Marketplace plugin |
| `gemini-extension.json`, `GEMINI.md` | Gemini CLI extension |
| `.devin-plugin/plugin.json` | Devin marketplace |
| `llms-install.md` | Install instructions for agents (Cline and others) |
| `distribution/` | Manifests for third-party catalogs (Docker, Devin, Raycast) |
| `assets/` | Logos |

## Try it

```bash
curl -s https://httpfy.io/mcp -H 'content-type: application/json' -H 'accept: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"dmarc_check","arguments":{"domain":"paypal.com"}}}'
```

## Privacy and terms

[Privacy policy](https://httpfy.io/privacy-policy) · [Terms of service](https://httpfy.io/terms-of-service)

## License

Manifests and documentation in this repository: [MIT](LICENSE). Check results: CC BY 4.0, credit httpfy.io.
