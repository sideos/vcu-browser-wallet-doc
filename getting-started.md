---
title: Getting started
nav_order: 2
---

# Getting started

## Install

The wallet is a Chrome (Manifest V3) extension that you load unpacked. Any Chromium-based browser that supports Manifest V3 extensions should work. It has been tested in Chrome.

You get the extension in one of two ways:

- **From a zip** that someone sent you. This is the usual way. No Node, no build step.
- **From source**, if you have access to the repository: `pnpm install`, then `pnpm build`, which writes the extension to `dist/`.

Then load it:

1. Open `chrome://extensions/`.
2. Switch on **Developer mode** (top right).
3. Click **Load unpacked** and choose the unzipped folder (or `dist/` if you built it yourself).
4. Pin the wallet from the puzzle-piece menu so its icon stays in the toolbar.

{: .note }
> After you **update or reload** the extension, **reload any page that was already open**. The scripts that let a page talk to the wallet run when a page starts loading, so they only reach pages opened afterwards. Rebuilding does not reload an installed extension either: use the refresh icon on its card in `chrome://extensions/`.

The wallet asks for access to all `http://` and `https://` pages. That is deliberate: it has to reach whichever issuer or verifier a link points to, and it can't know the host in advance.

## First launch

On first launch the wallet creates a **holder key** on your device. That key is what credentials are bound to. It never leaves the device, and there is no export. The first screen says so, and offers a PIN.

{% include shot.html src="onboarding.png" alt="The welcome screen: Your wallet is ready, with Set a PIN and Not now" caption="First launch. You can set a PIN now or skip it and set one later in Settings." %}

Choosing **Set a PIN** takes you to a 4 to 8 digit PIN entry. With a PIN set, the wallet locks itself after 5 minutes without use.

{% include shot.html src="pin-setup.png" alt="The Set a PIN screen with two PIN fields" caption="Choose a PIN and confirm it." %}

If you pick **Not now**, you land on an empty wallet. Nothing is stored yet.

{% include shot.html src="empty.png" alt="The empty wallet: No credentials yet, with a Paste a link button" caption="An empty wallet. Paste a link, or click an offer on a page that supports the wallet." %}

## Next

- Put a credential in the wallet: [Receiving a credential]({% link user-guide/receiving.md %}).
- Show one to a verifier: [Sharing a credential]({% link user-guide/sharing.md %}).
