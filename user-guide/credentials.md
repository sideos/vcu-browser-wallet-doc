---
title: Your credentials
parent: User guide
nav_order: 3
---

# Your credentials

## The list

{% include shot.html src="wallet-list.png" alt="The credential list: three credentials grouped under their issuer domains, with a search box and type filter chips" caption="Credentials are grouped by issuer. The number beside the + button is how many you hold." %}

- **Search** filters as you type. It matches the credential's *type* identifier (for example `EmploymentBadge`, written without spaces) and the issuer's address. It does not search the claims inside a credential, or a nickname you set.
- The **chips** under the search box filter by credential type. **All** shows everything.
- Each credential shows its name, how many claims it holds, and its format (`dc+sd-jwt` or `mso_mdoc`).
- A credential past its expiry date is marked **Expired**.

The wallet follows your system's light or dark setting by default. You can force one in [Settings]({% link user-guide/settings.md %}).

{% include shot.html src="wallet-list-dark.png" alt="The same credential list in dark mode" caption="The same list in dark mode." %}

## A credential's detail view

Click a credential to open it.

{% include shot.html src="credential-detail.png" alt="Detail view of an Employment Badge: an Issuer trusted pill, the format, the issuer and when it was received, then the claims" caption="The top of the detail view: who issued it, in what format, and its claims." %}

- **Issuer trusted** shows when you have trusted that issuer's domain.
- **Claims** lists the claims you can disclose. **Raw** switches to the underlying values.
- **Rename**: click the credential's name to give it a nickname, then press Enter to save or Esc to cancel. The nickname exists only in your wallet; clearing it brings the issuer's name back.
- **Present** opens the paste-a-link sheet, for when a verifier has sent you a request.
- The **bin** button deletes the credential.

Scroll down for the rest:

{% include shot.html src="credential-detail-history.png" alt="Lower half of the detail view: tags, the collapsed Metadata section, and the Shared with history" caption="Tags, protocol metadata, and where this credential has been shared." %}

- **Tags** are your own labels. Add one with **+ Add tag**; remove one with its **×**.
- **Metadata** holds the protocol fields that are part of the credential but aren't claims about you (issuer, type, issue and expiry times, the holder key binding, and the selective-disclosure plumbing). **Show** expands them.
- **Shared with** lists each requester this credential was shared with, and when.

## Deleting a credential

{% include shot.html src="delete-credential.png" alt="The Delete this credential confirmation sheet with Keep it and Delete buttons" caption="Deleting asks first." %}

Deleting removes the credential and its sharing history from this device. It does not tell the issuer, and it can't be undone: the issuer would have to send the credential again.

## Activity

The **Activity** tab lists every share and every refusal across all credentials, newest first, grouped by day.

{% include shot.html src="activity.png" alt="The Activity tab: four entries from different requesters, one of them marked Request declined" caption="Shared credentials and declined requests, by requester." %}

Each row shows who you shared with, which credential, and the time. A refused request is marked **Request declined**.

## Managing tags

**Settings → Manage tags** lists every tag and how many credentials use it. Deleting a tag here removes it from all of them. New tags are added from a credential's own detail view.

{% include shot.html src="manage-tags.png" alt="The Manage tags screen listing two tags with the number of credentials each is used on" caption="One place to see and delete tags." %}
