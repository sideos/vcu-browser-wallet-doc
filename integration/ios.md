---
title: iPhone app
parent: Integration guide
nav_order: 2
---

# Integrating with the VCu Wallet iPhone app

The iOS counterpart of [browser extension page]({% link integration/browser.md %}), for the same issuer and
verifier developers. The platform side is identical: the same API key,
`POST /api/credentials/offers` and `POST /api/presentations/requests`
calls, and the same verification result. What changes is how the wallet is
reached: through links and QR codes instead of `data-sideos-*` attributes,
and through Safari's Digital Credentials API for mdoc only.

The app is distributed through TestFlight (iOS 26 and later). It holds both
formats, `dc+sd-jwt` and `mso_mdoc`.

## At a glance

| You want to | Browser extension | iPhone app |
|---|---|---|
| Issue, same device | `data-sideos-offer` on a button | a link whose `href` is the `credentialOffer` |
| Issue, other device | — | the `credentialOffer` as a QR code |
| Issue after a login at your issuer | `chrome.identity` flow | user enters your issuer URL in the wallet |
| Request, same device | `data-sideos-request` on a button | a link whose `href` is the `authorizationRequest` |
| Request, other device | — | the `authorizationRequest` as a QR code |
| Request from a web page, standard API | DC API, `openid4vp-v1-*` | DC API in Safari, `org-iso-mdoc`, mdoc only |

## Issuing a credential

Create the offer exactly as in [Flow 1]({% link integration/browser.md %}#flow-1-issuing-a-credential),
then hand the `credentialOffer` URI to the phone:

```html
<!-- same device: a plain link; iOS opens VCu Wallet for this scheme -->
<a href="openid-credential-offer://?credential_offer=…">Add to VCu Wallet</a>
```

- **Other device:** render the same URI as a QR code. The user scans it in
  the wallet (Add, then Scan QR code).
- **Transaction code:** if the offer requires a `tx_code`, the wallet asks
  for it and shows your `tx_code.description` as the hint. Send the code by
  another channel (SMS, email).
- **Expiry and single use:** offers are single use and expire (10 minutes on
  the sandbox). Create the offer when the user is ready, not when the page
  loads.
- **Sign-in issuance (authorization code):** give the user your issuer
  identifier, `https://<platform>/oid4vci/<orgId>`. They enter it in the
  wallet (Add, paste), pick the credential type, and sign in on your login
  page in a private browser sheet. Nothing to render on your side. End users
  must exist in your org.
- **Key attestation:** if your template sets `keyAttestationRequired`, the
  iOS wallet accepts it only for `key_storage: ["none"]`, backed by Apple
  App Attest. It declines anything that names an ISO 18045 level or
  `user_authentication`, before using the offer up.

The result on your side is the same as in the browser flow: the credential
is issued, and the notification endpoint reports `credential_accepted`,
`credential_deleted` (declined) or `credential_failure`.

## Requesting and verifying a presentation

### By link or QR (OpenID4VP)

Create the request as in [Flow 2]({% link integration/browser.md %}#flow-2-requesting-and-verifying-a-presentation),
then hand the `authorizationRequest` to the phone the same way as an offer:

```html
<a href="openid4vp://?client_id=…&request_uri=…">Share from VCu Wallet</a>
```

or as a QR code for another device. Then poll
`GET /api/presentations/requests/{id}` from your backend, exactly as in the
browser flow.

- **Signed requests show who you are.** With `trustModel: "rpac"`, the wallet
  verifies your access certificate. It shows your organisation as a verified
  relying party, including your `organizationIdentifier`, and your
  registration certificate as verified when you have one. Unsigned requests
  work, but the user sees a warning. Users can turn on a setting that refuses
  every request not signed with an access certificate.
- **Encryption:** `jarm: true` (`direct_post.jwt`) is supported and shown to
  the user as "Response is encrypted to the verifier".
- **Both formats:** DCQL queries for `dc+sd-jwt` and `mso_mdoc` work, as do
  `credential_sets` and `transaction_data` (TS12 payment, login, account
  access) on `dc+sd-jwt`.
- **Returning to your page** (wallet build 20 and later). If the response
  endpoint answers the wallet's post with `{"redirect_uri": "https://…"}`
  (OpenID4VP 1.0 §8.2), the wallet opens it after Share or Decline. That URL
  can also use your own app's scheme. Without one, the wallet tells the user
  the data was sent and to go back by hand. The VCu platform doesn't return
  a `redirect_uri` yet (requested), so for now your page notices the result
  by polling.

### From a web page in Safari (Digital Credentials API)

Safari offers third-party wallets the `org-iso-mdoc` protocol only (ISO/IEC
18013-7 Annex C). OpenID4VP over the DC API is not available to them on iOS,
so SD-JWT credentials can't be requested this way: use a link or QR for
those.

1. **Create the request** with your page's exact origin and the mdoc
   protocol:

   ```json
   POST /api/presentations/requests
   {
     "templateId": "…",
     "format": "mso_mdoc",
     "requestedClaims": ["family_name", "age_over_18"],
     "dcApiOrigin": "https://shop.example.com",
     "dcApiProtocols": ["org-iso-mdoc"],
     "trustModel": "rpac"
   }
   ```

   `trustModel: "rpac"` signs the request (reader authentication); leave it
   out for an unsigned request. The response carries
   `digitalCredentialRequests`, an array of `{ protocol, data }`.

2. **Call the browser API** from a click handler:

   ```js
   const credential = await navigator.credentials.get({
     mediation: 'required',
     digital: { requests: digitalCredentialRequests },
   });
   ```

   iOS shows its own wallet picker, then the VCu Wallet sheet.

3. **Forward the answer** from your backend:
   `POST /api/presentations/requests/{id}/response` with
   `{ "protocol": credential.protocol, "response": credential.data.response }`.
   The reply is the final status. The response is HPKE-encrypted to the
   request's key, so your page only ever holds ciphertext.

What to expect:

- **The wallet is only offered when it can answer.** iOS lists VCu Wallet
  only if the wallet holds a document of the requested docType, and only if
  the user allowed websites to see which documents it holds.
- **Bound to your origin**, as reported by Safari. A response forwarded from
  another origin fails verification.
- **Decline** rejects the promise. Handle a rejection as "not shared".

## Current limitations

- **No wallet-presence signal.** Your page can't tell whether the app is
  installed. If it isn't, an `openid-credential-offer://` or `openid4vp://`
  link does nothing, or opens another wallet that claims the same scheme.
  iOS doesn't define which app wins when several claim one scheme.
- **No universal links yet.** Only the two custom schemes are registered.
- **No redirect from the VCu platform yet** (see above).
- **One flow at a time.** A second link while the wallet is busy is
  ignored.
- **Sandbox trust only.** The app trusts the VCu sandbox's issuer root,
  access CA root and registrar root. Requests and credentials under other
  roots are marked unverified.
