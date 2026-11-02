---
title: Home
layout: home
nav_order: 1
description: A Chrome extension wallet that holds OpenID4VCI credentials and answers OpenID4VP requests.
---

# Browser Wallet

A Chrome extension that plays the **holder** in the OpenID4VCI and OpenID4VP protocols: it receives credentials from an issuer, stores them on your device, and shows them to a verifier when you agree.

{: .warning }
> **This is a test and reference wallet, not a production wallet.** It exists so issuer and verifier developers have a real holder to test against. It isn't published on the Chrome Web Store, it has no key backup or export, and its PIN is a screen lock, not encryption (see [Trust and security]({% link user-guide/trust-and-security.md %})).

These pages describe **wallet version {{ site.wallet_version }}**. Every screenshot was captured from that build, with made-up data.

{% include shot.html src="wallet-list.png" alt="The wallet's credential list with three credentials grouped by issuer" caption="The wallet popup: credentials grouped by issuer." %}

## Where to start

| I want to… | Go to |
|---|---|
| Install the wallet and open it for the first time | [Getting started]({% link getting-started.md %}) |
| Receive, view and share credentials | [User guide]({% link user-guide/index.md %}) |
| Make my issuer or verifier work with this wallet | [Integration guide]({% link integration/index.md %}) |
| Change the wallet's name, colours, font or language | [Branding]({% link branding.md %}) |

## What it supports

| | |
|---|---|
| **Issuance** | [OpenID4VCI 1.0](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html): pre-authorized code (with an optional transaction code) and the authorization code flow with a sign-in step. |
| **Presentation** | [OpenID4VP 1.0](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) with DCQL queries, by link or through the browser's Digital Credentials API. |
| **Credential formats** | SD-JWT VC (`dc+sd-jwt`, built on [RFC 9901](https://www.rfc-editor.org/rfc/rfc9901.html) and the [SD-JWT VC draft](https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/)) and ISO mdoc (`mso_mdoc`). |
| **Web pages** | A page can hand the wallet a link with a `data-sideos-offer` / `data-sideos-request` attribute, or call the [W3C Digital Credentials API](https://www.w3.org/TR/digital-credentials/) (`openid4vp-v1-unsigned` and `openid4vp-v1-signed`). |
| **Languages** | English and German. |

{: .note }
> Specification versions and status change. The links above point to the current published text; check them before relying on a detail. The SD-JWT VC specification in particular was still an IETF working-group draft when this page was written.

## About the name

The wallet ships as "sideos Wallet", but its name, colours, font and language can be changed without rebuilding. Screenshots here show the default branding. The integration guide's examples use the data attribute prefix `data-sideos-`, which is the wallet's own convention and does not change with the brand.
