# dev.deedsats.com

Serves the association files that let the non-production Deedsats app builds
create and use passkeys with `dev.deedsats.com` as the rpId. Both platforms
fetch these to confirm the app is allowed to hold credentials for the domain;
without them, passkey create/get fails.

- `.well-known/apple-app-site-association`: `webcredentials` for
  `J88S62N4R4.com.deedsats.debug`.
- `.well-known/assetlinks.json`: digital asset links for `com.deedsats.debug`
  and `com.deedsats.profile`, both signed by the shared dev key.

It covers the development, staging-regtest and staging-testnet environments, and
is a trust-nothing domain: the signing key it delegates to is public. Never add
the production release fingerprint here; production lives on the apex
(`deedsats.com`).

Both URLs must return 200 over valid HTTPS with no redirect. Neither Apple nor
Google follows redirects for these paths.

## Verify

```bash
curl -sI https://dev.deedsats.com/.well-known/apple-app-site-association
curl -sI https://dev.deedsats.com/.well-known/assetlinks.json
curl -s "https://app-site-association.cdn-apple.com/a/v1/dev.deedsats.com"
curl -s "https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=https://dev.deedsats.com&relation=delegate_permission/common.get_login_creds"
```
