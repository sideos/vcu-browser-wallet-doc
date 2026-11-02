---
title: Integration guide
nav_order: 4
has_children: true
permalink: /integration/
---

# Integration guide

For developers building the **issuer** or **verifier** side: how to hand a credential offer or a presentation request to this wallet, and what comes back.

| Page | Read it for |
|---|---|
| [Browser extension]({% link integration/browser.md %}) | The full issuance and verification flows, the exact platform calls, the trigger attributes, the Digital Credentials API path, and errors and edge cases. |
| [iPhone app]({% link integration/ios.md %}) | The same platform calls, reached through links, QR codes and Safari instead of page attributes. |
| [Live example]({{ '/integration/example.html' | relative_url }}) | A small page with the two trigger attributes wired up. Paste a URI from your backend, click, and watch the wallet react. The wallet extension must be installed. |

{: .note }
> The guide is written against one specific platform's API (the endpoints, scopes and result shapes below are that platform's). The wallet side, OpenID4VCI and OpenID4VP, is standard, but the `POST /api/...` calls are not. If you run a different issuer or verifier, you only need the wallet-facing parts: the trigger attributes, the Digital Credentials API section, and the notes on what the wallet shows.
