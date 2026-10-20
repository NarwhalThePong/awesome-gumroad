# Awesome Gumroad for Developers [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of open-source tools, API clients, integrations and hard-won gotchas for developers who **build on** or **sell software through** [Gumroad](https://gumroad.com).

Contributions welcome: open a PR (see [Contributing](#contributing)).

## Contents
- [Official](#official)
- [API clients](#api-clients)
- [License key verification](#license-key-verification)
- [Access control (Discord, GitHub)](#access-control-discord-github)
- [Webhooks and sale alerts](#webhooks-and-sale-alerts)
- [Analytics and dashboards](#analytics-and-dashboards)
- [Pricing and discounts](#pricing-and-discounts)
- [Embeds, themes and storefronts](#embeds-themes-and-storefronts)
- [Auth](#auth)
- [Testing and debugging](#testing-and-debugging)
- [Free web tools](#free-web-tools)
- [Gotchas every Gumroad developer hits](#gotchas-every-gumroad-developer-hits)
- [Paid kits](#paid-kits)

## Official
- [Gumroad API docs](https://gumroad.com/api) - REST API v2: products, sales, subscribers, licenses, resource subscriptions (Ping).
- [antiwork/gumroad](https://github.com/antiwork/gumroad) - The Gumroad app itself, open source. The best reference for how the API really behaves.
- [antiwork/gumroad-cli](https://github.com/antiwork/gumroad-cli) - Official CLI for the Gumroad API.
- [antiwork/gumroad-mobile](https://github.com/antiwork/gumroad-mobile) - The Gumroad mobile app, built with Expo.
- [antiwork/wp-gumroad](https://github.com/antiwork/wp-gumroad) - WordPress plugin to embed or overlay Gumroad products with shortcodes.

## API clients
- [SamyPesse/gumroad-api](https://github.com/SamyPesse/gumroad-api) - Node.js client.
- [noahbuscher/gumnode](https://github.com/noahbuscher/gumnode) - Node.js client.
- [opsdisk/pygumroad](https://github.com/opsdisk/pygumroad) - Python client.
- [tj/gumroad](https://github.com/tj/gumroad) - Go client.
- [maxstoller/gumroad](https://github.com/maxstoller/gumroad) - Ruby wrapper.

## License key verification
- [gumroad-license-check](https://github.com/NarwhalThePong/gumroad-license-check) - Zero-dependency Node and Python check that also rejects refunded, charged-back and disputed purchases and ended subscriptions.
- [caarlos0/go-gumroad](https://github.com/caarlos0/go-gumroad) - Check licenses against Gumroad's API from Go.
- [dkasaj/GumroadLicenseValidator](https://github.com/dkasaj/GumroadLicenseValidator) - License key validation for macOS Swift apps.

## Access control (Discord, GitHub)
- [benaclejames/GumCord](https://github.com/benaclejames/GumCord) - Discord bot that verifies Gumroad license keys and assigns roles.
- [ErfanEbrahimnia/unlockrepo](https://github.com/ErfanEbrahimnia/unlockrepo) - Automate GitHub repository access for buyers.

## Webhooks and sale alerts
- [alexellis/gumroad-custom-workflow](https://github.com/alexellis/gumroad-custom-workflow) - Get a Slack message every time you sell a product.

## Analytics and dashboards
- [guivr/ohmydashboard](https://github.com/guivr/ohmydashboard) - Dashboard that unifies Stripe, Gumroad and RevenueCat metrics.
- [sameerasw/GumroadStats](https://github.com/sameerasw/GumroadStats) - Android payouts tracker with a home-screen widget.

## Pricing and discounts
- [peterj/ppp-gumroad-discounts](https://github.com/peterj/ppp-gumroad-discounts) - Create purchasing-power-parity discount codes.

## Embeds, themes and storefronts
- [peterw/Gumroad-Landing-Page-Generator](https://github.com/peterw/Gumroad-Landing-Page-Generator) - Generate a landing page from a Gumroad profile.
- [michael-andreuzza/microstore](https://github.com/michael-andreuzza/microstore) - Free Astro theme with Gumroad embeds.
- [YJPL/Supply](https://github.com/YJPL/Supply) - Jekyll e-commerce theme with Gumroad integration.
- [mddanishyusuf/gumroad-coffee-template](https://github.com/mddanishyusuf/gumroad-coffee-template) - "Buy me a coffee" page paid through Gumroad.
- [isqua/hugo-shortcodes](https://github.com/isqua/hugo-shortcodes) - Hugo shortcodes, including a Gumroad button.
- [Moyf/obsidian-gumroad-theme](https://github.com/Moyf/obsidian-gumroad-theme) - Obsidian theme in Gumroad's style.

## Auth
- [antiwork/omniauth-gumroad](https://github.com/antiwork/omniauth-gumroad) - OmniAuth strategy for "Sign in with Gumroad".

## Testing and debugging
- [justinmahar/gumroad-api-tester](https://github.com/justinmahar/gumroad-api-tester) - Web tool for poking at the Gumroad API.

## Free web tools
Disclosure: these are made by the maintainer of this list. They run in the browser and don't upload anything.
- [Gumroad license key checker](https://narwhalthepong.github.io/gumroad-license-check/gumroad-license-key-checker) - Check a key's validity, refund, chargeback, dispute and subscription status without incrementing its uses.
- [Gumroad fee calculator](https://narwhalthepong.github.io/gumroad-license-check/gumroad-fee-calculator) - Payout on direct vs Discover sales and the price needed for a target payout.
- [Gumroad sales CSV analyzer](https://narwhalthepong.github.io/gumroad-license-check/gumroad-sales-csv-analyzer) - Gross, refunds, fees and net by product, month, referrer and country from a sales export.
- [Gumroad bundle and discount calculator](https://narwhalthepong.github.io/gumroad-license-check/gumroad-bundle-discount-calculator) - Bundle and offer-code pricing with fees and buyer savings.

## Gotchas every Gumroad developer hits
1. **`/v2/licenses/verify` returns `success: true` after a refund.** The key still exists. Check `purchase.refunded`, `purchase.chargebacked`, `purchase.disputed`, and for memberships `subscription_ended_at`, `subscription_cancelled_at` and `subscription_failed_at`, or refunded buyers keep access.
2. **Verify calls count activations by default.** Pass `increment_uses_count=false` on routine re-checks, or your app burns through a buyer's activations every time it starts.
3. **Ping (the sale webhook) is not signed.** Anyone who learns your Ping URL can post a fake sale. Put a secret in the URL and re-fetch the sale from `GET /v2/sales/:id` with your token before acting on it.
4. **Ping is form-encoded, not JSON.** Pointing it straight at a Discord or Slack incoming webhook does not work; you need a small relay in between.
5. **Pings can arrive more than once.** Deduplicate on sale ID plus event type.
6. **A missed Ping is a missed refund.** If your server is down or errors when a refund or chargeback Ping arrives, you can miss it. Run a periodic reconcile against the Sales API so access is removed anyway.
7. **Fees differ by channel.** Direct sales and Discover marketplace sales are charged differently, so compute net per sale instead of applying one flat rate. Check Gumroad's current pricing page for the rates.

Walkthroughs with code for these: [Gumroad developer guides](https://narwhalthepong.github.io/gumroad-license-check/) (license checks, private repo access, sale alerts, Discord roles, fees, 1099-K taxes, and a [fee comparison with Lemon Squeezy, Polar and Paddle](https://narwhalthepong.github.io/gumroad-license-check/gumroad-vs-lemon-squeezy-polar-paddle-fees)).

## Paid kits
Disclosure: these are made by the maintainer of this list.
- [Gumroad License Kit](https://paudrudis.gumroad.com/l/dufcs) - License verification with offline cache, activation limits and a proxy; Node and Python.
- [Gumroad to GitHub Access Kit](https://paudrudis.gumroad.com/l/znsmji) - Sell a private repo: auto-invite on sale, auto-remove on refund or ended membership.
- [Gumroad Sales Dashboard Kit](https://paudrudis.gumroad.com/l/btvjm) - Pull sales into CSV and fee-aware reports.
- [Gumroad Seller Finance Tracker](https://paudrudis.gumroad.com/l/sudmq) - Spreadsheet: real net after fees, monthly P&L, refunds, tax set-aside.
- [Gumroad Ping Relay](https://paudrudis.gumroad.com/l/tdohx) - Verified sale and refund alerts to Discord, Slack and Telegram.
- [Gumroad to Discord Access Kit](https://paudrudis.gumroad.com/l/gskxf) - Sell a paid Discord role or community; roles removed on refund, chargeback or ended membership.

## Contributing
Add a project by opening a pull request. Keep entries to one line: `- [name](link) - What it does.` Projects should be maintained, open source or clearly documented, and actually about Gumroad.

## License
[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)
To the extent possible under law, the maintainer has waived all copyright to this list. Not affiliated with Gumroad.
