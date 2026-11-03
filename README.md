# vgs-proxy-demo

An interactive, single-page walkthrough of how the [Very Good Security](https://www.verygoodsecurity.com/) (VGS) proxy works. You type a test card, send it through the inbound route, charge it through the outbound route, and see what every hop received: the browser, your backend, your DB and logs, the VGS vault, and the payment processor.

**Live page:** https://deuber.github.io/vgs-proxy-demo/

## What it shows

- **Inbound (reverse proxy, REDACT):** the form posts to the vault URL. VGS swaps the card number and CVC for aliases before the request reaches your API, so your database and logs only ever hold aliases.
- **Outbound (forward proxy, REVEAL):** your backend sends the charge through VGS. VGS swaps the aliases back for real values on the way to the processor.
- **Through VGS vs. direct:** flip the toggle to see the same checkout with no VGS, and which systems end up holding raw card data.
- **The Content-Type trap:** post the form as `x-www-form-urlencoded`. The JSON-only filter doesn't match, and the raw card passes straight through with no error.
- **Alias formats:** 19-digit fixed, `FPE_SIX_T_FOUR`, `FPE_T_FOUR`, `NUM_LENGTH_PRESERVING`, `UUID`.
- **Generated config:** an illustrative route YAML and the code changes (curl to the vault URL, Python `requests` through the forward proxy), both kept in sync with your filter choices.
- **Real setup steps**, following VGS's own [HTTP Proxy walkthrough](https://www.youtube.com/watch?v=2Aex2VpcvCE).

## Honest scope

This is a **simulation**. It runs entirely in the browser, makes no network calls and has no VGS account behind it. Aliases are generated locally to look like VGS alias formats, and the route YAML has the same shape as a VGS export but is trimmed down. Use test card numbers only. Not affiliated with VGS.

## Run locally

It's one static file with no build step:

```sh
open docs/index.html
# or
python3 -m http.server -d docs 8000
```

## Deploy

GitHub Pages → Settings → Pages → Source: `main` branch, `/docs` folder.
