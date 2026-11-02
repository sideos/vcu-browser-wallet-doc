---
title: Receiving a credential
parent: User guide
nav_order: 1
---

# Receiving a credential

An issuer gives you a credential in four steps: you hand the wallet a link, the wallet checks who the issuer is, you review what is being offered, and you accept or decline.

## 1. Give the wallet the link

Click **+** (or **Paste a link** on an empty wallet) and paste the link. As you type, the wallet says what it recognised:

{% include shot.html src="paste-link.png" alt="The Paste a link sheet with an issuer address entered and the line 'Get a credential from issuer.example.com'" caption="The wallet reads what kind of link you pasted." %}

| You paste | The wallet says | What happens next |
|---|---|---|
| A credential offer (`openid-credential-offer://…`) | *Credential offer from …* | The wallet collects the credential from that issuer. |
| An issuer address (`https://…`) | *Get a credential from …* | The wallet starts a sign-in with that issuer, in a browser window, and then collects the credential. |
| A presentation request (`openid4vp://…`) | *Presentation request from …* | See [Sharing a credential]({% link user-guide/sharing.md %}). |

If the issuer's page has a button for this wallet, you can skip pasting: click the button and the wallet opens with the offer loaded.

## 2. Issuers you have not met

The first time an issuer's domain shows up, the wallet stops and asks before it does anything.

{% include shot.html src="requester-warning.png" alt="Warning screen: You have not met this issuer, showing the issuer domain and the status Never seen before" caption="A first-time issuer. Nothing has been fetched or stored yet." %}

- **Deny and close** blocks that domain. Future offers from it are refused automatically.
- **Add anyway** continues. If you then accept the credential, the issuer is remembered as trusted.

The wallet checks before it uses up the offer, so stopping here does not waste a single-use offer. You can review and change these decisions at any time in **Settings → Requesters** ([Trust and security]({% link user-guide/trust-and-security.md %})).

## 3. A code, if the issuer wants one

Some issuers protect an offer with a short code that they send you separately, by SMS or email, for example. The wallet then shows an **Enter code** screen with the issuer's hint. Enter the code and continue.

## 4. Review and accept

{% include shot.html src="offer-consent.png" alt="Consent screen for an Employment Badge from a trusted issuer, listing the claims that will be stored, with Decline and Add to wallet buttons" caption="The consent screen: what will be stored, and from whom." %}

The consent screen shows the credential's name, who issued it, and the claims you would be storing, plus its format. **Nothing is written to your device until you choose Add to wallet.**

- **Add to wallet** stores the credential.
- **Decline** discards it.

If the issuer supports it, the wallet tells the issuer which you chose (accepted or declined). That is how an issuer finds out whether you actually have the credential.

{: .note }
> **If the popup closes before you decide**, nothing is lost. An offer can only be collected once, so as soon as the wallet has the credential it keeps a copy aside, and the consent screen comes back the next time you open the wallet. That copy lasts for the browser session: it survives closing the popup, but not quitting the browser.
