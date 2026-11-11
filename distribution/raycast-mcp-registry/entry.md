# Raycast MCP Registry entry

Entry for `OFFICIAL_ENTRIES` in `extensions/model-context-protocol-registry/src/registries/builtin/entries.ts` (raycast/extensions).

```ts
  {
    name: "httpfy",
    title: "HTTPfy",
    description:
      "Free DNS, email-authentication (SPF, DKIM, DMARC, BIMI, MTA-STS), TLS certificate, WHOIS, IP/ASN, blocklist, ping, traceroute and port checks run from httpfy.io servers. 24 read-only tools, every result links to a full report. Hosted remote Streamable HTTP server; no sign-in and no API key.",
    icon: "https://httpfy.io/static/android-chrome-512x512.png",
    homepage: "https://httpfy.io/agents",
    configuration: {
      command: "npx",
      args: ["-y", "mcp-remote", "https://httpfy.io/mcp"],
    },
    remoteUrl: "https://httpfy.io/mcp",
  },
```
