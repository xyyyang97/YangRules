# YangRules

YangRules is a production-oriented personal proxy rule repository for long-term maintenance across Apple and Clash ecosystem clients.

It is intentionally not an airport subscription and does not ship any proxy nodes. The repository only maintains:

- policy groups
- DNS settings
- rule order
- rule-provider definitions
- documentation

All large rule datasets stay upstream and are referenced remotely.

## Supported Platforms

- Shadowrocket on iPhone and iPad
- Clash Verge Rev on macOS
- Mihomo / Clash Meta on Android
- Clash Meta on Windows and Linux

## Repository Architecture

```text
YangRules/
├── Shadowrocket/
│   └── Yang.conf
├── Clash/
│   ├── Yang.yaml
│   └── providers/
├── README.md
├── LICENSE
└── .gitignore
```

### Design Choices

- No upstream repository is forked.
- No bulk domain database is committed into this repository.
- Upstream rule lists are referenced directly from BlackMatrix7.
- The config is provider-independent by design.
- The final routing policy is `MATCH -> Proxy`.

## Policy Groups

Both configs define these core groups:

- `🚀 Proxy`
- `🤖 AI`
- `🍎 Apple`
- `💻 Microsoft`
- `🧑‍💻 Dev`
- `🎬 Media`
- `🎯 Direct`
- `🛑 Reject`

### AI Routing Scope

The repository keeps separate rule-provider definitions for these services so they are easy to maintain and extend:

- Apple Intelligence
- Siri
- OpenAI
- Anthropic
- Gemini
- Microsoft
- Bing
- GitHub
- GitHub Copilot

Current default routing logic:

- Apple Intelligence, Siri, OpenAI, Anthropic, Gemini, Bing, GitHub Copilot -> `🤖 AI`
- Apple ecosystem traffic -> `🍎 Apple`
- Microsoft ecosystem traffic -> `💻 Microsoft`
- GitHub developer traffic -> `🧑‍💻 Dev`

To add a future AI service, add one new provider entry and one new rule line near the existing AI block.

### Media Routing Scope

Media traffic is routed with dedicated upstream providers for:

- YouTube
- Netflix
- Disney+
- Telegram

## Rule Strategy

The rule order is intentionally simple:

1. Match high-priority AI services first.
2. Match Apple, Microsoft, developer, and media services next.
3. Send China traffic to `DIRECT`.
4. Fall back to `MATCH -> Proxy`.

Domestic traffic policy:

- China traffic -> `DIRECT`
- International traffic -> `🚀 Proxy`

## Upstream Rule Source

Primary rule source:

- BlackMatrix7 `ios_rule_script`
- Repository: `https://github.com/blackmatrix7/ios_rule_script`

Platform mapping:

- Shadowrocket uses remote `.list` rule sets from the `Shadowrocket` directory.
- Clash / Mihomo uses remote `.yaml` rule-providers from the `Clash` directory.

## Installation Guide

### Shadowrocket

1. Open the raw `Shadowrocket/Yang.conf` URL in Safari.
2. Use the system share sheet and choose Shadowrocket, or copy the URL and import it in Shadowrocket.
3. After import, bind your own proxies or proxy subscription inside Shadowrocket.
4. Set `🚀 Proxy` as the main outbound selector for international traffic.

Important:

- This repository does not provide nodes.
- If you do not add your own proxies, `🚀 Proxy` only has `DIRECT` available by default.

### Clash Verge Rev / Mihomo / Clash Meta

1. Import the raw `Clash/Yang.yaml` URL into your client.
2. Add your own `proxies` or `proxy-providers` according to your environment.
3. Keep `🚀 Proxy` as the main international policy group.
4. Use the other groups to override AI, Apple, Microsoft, developer, or media traffic as needed.

Important:

- `Yang.yaml` is designed to stay independent from any provider.
- The top-level `🚀 Proxy` group uses `include-all: true`, so appended proxies can be surfaced without hardcoding vendor names.

## How To Update

- The repository itself changes rarely.
- Upstream rule datasets refresh remotely from BlackMatrix7.
- Rule providers are configured with a 24-hour refresh interval in Clash.
- To update your local policy structure, pull the latest repository version or re-import the raw file URL.

## How To Customize Policy Groups

### Shadowrocket

- Edit `Shadowrocket/Yang.conf`.
- Change rule targets if you want a service to go to `DIRECT`, `REJECT`, or another group.
- Add new `RULE-SET` lines above the China and final rules for any new service.

### Clash / Mihomo

- Edit `Clash/Yang.yaml`.
- Add or remove `rule-providers` under the provider section.
- Add matching `RULE-SET` entries in the `rules` block.
- Reorder `proxy-groups` if you want different defaults.

Recommended customization patterns:

- Route work services to `🧑‍💻 Dev`
- Route region-sensitive streaming to `🎬 Media`
- Keep Apple China traffic on `🎯 Direct`
- Send privacy-sensitive or unavailable services to `🛑 Reject`

## Why This Repository Does Not Contain Proxy Subscriptions

This repository is deliberately separated from providers and credentials.

Reasons:

- proxy subscriptions change frequently
- subscriptions are personal and often sensitive
- public repositories should not expose node URLs or secrets
- rule structure should remain stable even when providers change
- one clean rule repository can be reused across multiple clients and providers

## Security Notes

This repository must never contain:

- airport subscription URLs
- proxy nodes
- API keys
- passwords
- tokens
- personal information

If you need to use private providers, add them only inside your local client configuration and do not commit them here.

## Credits

- Rule source credits: BlackMatrix7 `ios_rule_script`
- This repository only maintains reusable policy structure on top of those upstream rules
