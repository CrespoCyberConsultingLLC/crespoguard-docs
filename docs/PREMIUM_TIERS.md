# CrespoGuard Tier Reference

> Tiering controls player caps and paid server-side capabilities. The neutral Community launcher package stays server-neutral and uses manual package updates by default.

## Tier Overview

| Tier          | Price     | Max Players | Key Additions                                                      |
| ------------- | --------- | ----------- | ------------------------------------------------------------------ |
| **Community** | Free      | 50          | Launcher, self-hosted relay, IP bans/rate limiting, manual updates |
| **Guard**     | $15/mo    | 50          | Dashboard, HWID bans, combat features, server IP masking           |
| **Shield**    | $30/mo    | 200         | Multi-zone proxy, file logging                                     |
| **Fortress**  | $50-75/mo | 500         | Edge relay mode, PROXY protocol, health check endpoint             |

## Community

Community is the neutral base lane for server owners starting with CrespoGuard:

- Launcher/operator kit with templates.
- `modules.json` is created by the operator and encrypted into `System\Launcher\Config\config.bin`.
- Manual package updates by default.
- Self-hosted relay path for encrypted login, IP bans, and rate limiting.
- No server-specific config is included in the base operator kit.

## Guard

Guard adds paid management and enforcement capabilities:

- Relay dashboard.
- HWID ban enforcement.
- Announcements and admin actions.
- Paid client-side combat features where configured.
- Server IP masking through the configured relay path.

## Shield

Shield is for larger deployments that need more network and logging controls:

- Higher player cap.
- Multi-zone proxy support.
- File logging.

## Fortress

Fortress is for larger or geographically distributed deployments:

- Highest published player cap.
- Edge relay mode.
- PROXY protocol v1.
- Health check endpoint.

## Choosing Your Tier

| Your Situation                                               | Recommended Tier |
| ------------------------------------------------------------ | ---------------- |
| Small community, neutral launcher package, manual updates    | **Community**    |
| Need HWID bans, dashboard, or combat features                | **Guard**        |
| Need 50-200 players or multi-zone proxy/logging              | **Shield**       |
| Need 200+ players, edge relays, or load-balancer integration | **Fortress**     |

## Activation

1. Purchase a subscription from CrespoGuard if you need a paid tier.
2. Receive an activation code.
3. Run activation on the relevant server component.
4. Restart the server component so the tier is applied.

No player-facing archive should include plaintext `modules.json`; distribute the generated `config.bin` only.
