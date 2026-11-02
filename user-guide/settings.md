---
title: Settings reference
parent: User guide
nav_order: 5
---

# Settings reference

{% include shot.html src="settings.png" alt="The Settings tab: Wallet identity, Appearance, Language, Branding, and the start of Security" caption="The top of Settings. Scroll for Security, Data, Developer and the lock and reset buttons." %}

| Section | Setting | What it does |
|---|---|---|
| **Wallet identity** | (fingerprint) | The start of your holder key's public value. Useful to tell wallets apart. It identifies the key, not you. |
| **Appearance** | System / Light / Dark | Follow the operating system, or force a theme. |
| **Language** | English / Deutsch | The wallet's language. A branding config can set the default; your choice here wins. |
| **Branding** | Import branding | Paste a JSON config to change the name, colours, font or language, with no rebuild. See [Branding]({% link branding.md %}). |
| | Reset to default branding | Back to the branding shipped in the build. |
| **Security** | Require PIN to unlock | Lock the wallet behind a PIN. See [Trust and security]({% link user-guide/trust-and-security.md %}#the-pin). |
| | Allow unknown requesters | Skip the first-contact warning for domains never seen. See [Trust and security]({% link user-guide/trust-and-security.md %}#allow-unknown-requesters). |
| | Requesters | The trusted and blocked lists. |
| **Data** | Activity history | Everything you shared or declined (same as the Activity tab). |
| | Manage tags | See and delete tags. |
| **Developer** | Run library smoke test | A self-test of the wallet's bundled libraries, with the result shown on screen. For developers; you don't need it to use the wallet. |
| *(bottom of the screen)* | **Lock wallet** | Lock now. Only shown while *Require PIN to unlock* is on. |
| | **Reset wallet** | Delete the holder key, every credential and the PIN, and start over. |
