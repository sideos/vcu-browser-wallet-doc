# Browser Wallet docs

Source of the documentation site for the Browser Wallet:
<https://sideos.github.io/vcu-browser-wallet-doc/>

A [Jekyll](https://jekyllrb.com/) site using the
[Just the Docs](https://just-the-docs.com/) theme, built by GitHub Pages from
the `main` branch. There is no build step to run: push Markdown, the site
updates.

## Layout

| Path | What |
|---|---|
| `index.md`, `getting-started.md` | Home and install |
| `user-guide/` | Receiving, sharing, managing credentials, trust and security, settings |
| `integration/` | Issuer and verifier guide (browser extension, iPhone app) and a live example page |
| `branding.md` | Name, colours, font and language |
| `assets/img/` | Screenshots (generated, see below) |
| `_includes/shot.html` | Embeds a screenshot at the wallet's real popup size |

## Screenshots are generated, not hand-made

`assets/img/*.png` come from the wallet's own repo, not from this one. In the
wallet repo:

```sh
pnpm build        # the extension, into dist/
pnpm screenshots  # writes docs/assets/img/*.png
```

The script loads the built extension in Chromium, seeds it with made-up
credentials, and drives the real screens, including a share through the
Digital Credentials API. Nothing in the images is real holder data. After
regenerating, update `wallet_version` in `_config.yml` to the version that was
built.

## Preview locally

GitHub builds the site, so this is optional. With Ruby and Bundler:

```sh
cat > Gemfile <<'EOF'
source "https://rubygems.org"
gem "jekyll", "~> 4.3"
gem "just-the-docs", "0.12.0"
gem "webrick"
EOF
printf 'remote_theme: null\ntheme: just-the-docs\nplugins: []\n' > _config.local.yml
bundle install
bundle exec jekyll serve --config _config.yml,_config.local.yml
```

`Gemfile` and `_config.local.yml` are for preview only; the first is ignored by
the site build, and neither is needed on GitHub.

## What is deliberately not here

The wallet's internal planning documents, change requests to the platform, and
changelog stay in the (private) wallet repository.
