---
title: Sharing a credential
parent: User guide
nav_order: 2
---

# Sharing a credential

A verifier asks for some claims from a credential. You decide whether to answer and exactly which claims go.

## The request arrives

A request reaches the wallet the same three ways an offer does: you paste it, you click a button on the verifier's page, or the page asks through the browser's Digital Credentials API. See [three ways to hand the wallet a link]({% link user-guide/index.md %}#three-ways-to-hand-the-wallet-a-link).

## Requesters you have not met

As with issuers, a verifier the wallet has never seen gets a warning first.

{% include shot.html src="requester-warning-verifier.png" alt="Warning screen: You have not met this requester, showing the requester domain verifier.example.com and the status Never seen before" caption="A first-time requester. Nothing is sent unless you continue." %}

- **Deny and close** blocks the requester. If the request came from a page through the browser API, the page is told you refused.
- **Share anyway** continues to the share screen. If you then share, the requester is remembered as trusted.

## Choose what to share

{% include shot.html src="share-request.png" alt="Share screen: Requested by verifier.example.com, with four claims from the Employment Badge, two ticked and marked Asked, two unticked" caption="The share screen. The ticked claims are what the verifier asked for." %}

- **Requested by** is the verifier's domain. A **Trusted** badge appears next to a requester you have trusted before.
- Under **Disclose from …** you see the claims of the credential that match the request. The claims the verifier asked for are pre-ticked and marked **Asked**.
- **Untick any claim to keep it back.** The verifier only learns what you send.
- If more than one stored credential matches the request, you choose which one to use.
- If none matches, the wallet says so (*No stored credential matches …, nothing to present*) and offers **Close**.

Some requests carry **transaction data**, for example a payment or login the verifier wants you to confirm. The wallet shows it under *Transaction data* as plain structured data. It is not (yet) laid out as a formatted payment summary, so read it carefully.

Press **Share N claims** to send, or **Deny** to refuse. A refusal is recorded in [Activity]({% link user-guide/credentials.md %}#activity).

## Confirmation

{% include shot.html src="presentation-sent.png" alt="The Shared confirmation: 2 claims shared with verifier.example.com, with Department and Employee id withheld and Employee name and Employer disclosed" caption="After sharing, the wallet lists what was disclosed and what was withheld." %}

The confirmation lists each claim as **disclosed** or **withheld**, and the share is recorded in your activity history.

## How this differs between flows

| | Pasted link or clicked button | Browser API (Digital Credentials) |
|---|---|---|
| The verifier learns the answer | Your wallet posts it to the verifier's endpoint | The page's own request returns your answer |
| If you deny | The verifier's session simply stays unanswered until it expires | The page's request is rejected, so the page knows you said no |
| Who the answer is bound to | The verifier named in the request | The page's own origin, as reported by the browser |
