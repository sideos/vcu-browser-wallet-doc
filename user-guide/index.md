---
title: User guide
nav_order: 3
has_children: true
permalink: /user-guide/
---

# User guide

The wallet is one small popup with three tabs along the bottom:

| Tab | What it is |
|---|---|
| **Credentials** | Everything you hold, grouped by issuer. The **+** button at the top starts adding or presenting something. |
| **Activity** | A log of every time you shared a credential or declined a request. |
| **Settings** | Appearance, language, security, the list of requesters you trust or block, and reset. |

## Three ways to hand the wallet a link

Receiving a credential and sharing one both start with a link: an offer from an issuer, or a request from a verifier. There are three ways it reaches the wallet.

1. **Paste it.** Click **+** and paste the link. The wallet works out whether it is an offer, a request or an issuer address.
2. **Click it on a page.** A page built for this wallet puts the link on a button. Clicking the button opens the wallet with the link already loaded.
3. **A page asks through the browser.** A site can use the browser's Digital Credentials API. With this extension installed, the wallet answers those requests itself and the page gets your answer.

If the wallet can't open its popup on its own after a click, the **toolbar icon changes** to show that something is waiting. Click the icon to open it.

## In this guide

- [Receiving a credential]({% link user-guide/receiving.md %})
- [Sharing a credential]({% link user-guide/sharing.md %})
- [Your credentials]({% link user-guide/credentials.md %}): the list, the detail view, tags, history and deleting
- [Trust and security]({% link user-guide/trust-and-security.md %}): requesters, the PIN, and resetting
- [Settings reference]({% link user-guide/settings.md %})
