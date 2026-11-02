---
title: Trust and security
parent: User guide
nav_order: 4
---

# Trust and security

{: .warning }
> This is a test and reference wallet. Read the limits below before you put anything sensitive in it.

## Your holder key

On first launch the wallet creates a key pair on your device. Credentials are bound to that key, and every presentation is signed with it. The private key stays on the device. There is **no export and no backup**, by design. If you reset the wallet or lose the browser profile, the key is gone, and so are the credentials: the issuer has to issue them again.

## Requesters: who the wallet will talk to

The wallet keeps a list of the **issuers and verifiers** (it calls them requesters) it has seen, by domain. A domain is in one of three states:

| State | What the wallet does |
|---|---|
| **Trusted** | Goes straight ahead, with a *Trusted* marker on the consent screens. |
| **Unknown** (never seen) | Stops on a *You have not met this …* screen and asks. |
| **Blocked** | Refuses automatically. |

You build this list by using the wallet: accepting a first-time issuer or sharing with a first-time verifier (**Add anyway** / **Share anyway**) trusts that domain. **Deny and close** blocks it. It is trust on first use, so it protects against surprises, not against an issuer or verifier that you chose to trust.

Review the list in **Settings → Requesters**.

{% include shot.html src="requesters.png" alt="The Requesters screen: a Trusted tab listing four domains with their share counts and Untrust buttons, and a Blocked tab" caption="Settings → Requesters. Untrust sends a domain back to Unknown; the Blocked tab lists what you refused." %}

- On the **Trusted** tab, **Untrust** returns a domain to Unknown, so it asks you again next time.
- On the **Blocked** tab, **Trust** lets a blocked domain through again.
- A request from a requester you blocked is stopped on a **Blocked** screen, where you can **Keep blocked** or **Trust & continue**.

### "Allow unknown requesters"

This switch in Settings skips the *You have not met this …* screen for domains the wallet has never seen. Two things stay true when it is on:

- A **blocked** domain stays blocked. The switch only waives the warning for unknown ones.
- Letting an unknown domain through this way does **not** make it trusted. Only an explicit **Add anyway** or **Share anyway** does that.

Leave it off unless you are testing against many throwaway hosts.

## The PIN

Switch on **Settings → Require PIN to unlock** (or set one at first launch) to lock the wallet behind a 4 to 8 digit PIN. Once the wallet has been idle for 5 minutes, the next time you open it, it asks for the PIN. **Lock wallet** in Settings locks it right away.

{% include shot.html src="pin-unlock.png" alt="The locked wallet showing a numeric keypad" caption="The locked wallet." %}

{: .warning }
> **The PIN is a screen lock, not encryption.** The wallet stores a salted hash of the PIN to check what you type. The credentials themselves are stored on disk by the browser as they are, unencrypted. The PIN stops someone from opening the wallet's screens on an unattended, unlocked browser. It does not protect against someone with access to your browser profile or your computer's files. Rely on your operating system's disk encryption and account lock for that.

**Forgot the PIN?** The only way back in is **Forgot PIN — reset wallet**, which wipes the holder key, every credential and the PIN. The PIN can't be recovered.

## Resetting the wallet

**Settings → Reset wallet** asks you to confirm, then deletes the holder identity, every stored credential, and the PIN, returning the wallet to a fresh install. This can't be undone, and there is no backup to restore from.

## What the wallet can reach

The extension has permission to contact any `http://` or `https://` host, because it has to reach whichever issuer or verifier a link names. It also uses the browser's sign-in window for issuers that require a login. Network requests happen as part of something you started: collecting an offer, answering a request, or (during an offer) fetching the credential type's metadata document for a display name. The wallet's background code has no timers or scheduled jobs.
