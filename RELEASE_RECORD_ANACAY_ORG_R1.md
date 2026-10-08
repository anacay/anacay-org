# Release record: anacay.org R1

- **Date:** 2026-10-08
- **What:** One static page at anacay.org listing the open, non-commercial work Anacay supports ("Built by people, for people"). Links to cpp.anacay.org and anacay.com.
- **Repo:** https://github.com/anacay/anacay-org (public, `main`)
- **Worker:** `anacay-org` (Cloudflare account that also holds anacay.com)
- **Deployed version:** `f969c248-d488-4d80-93fa-3cdfe1058823` at 100%
- **Custom domains:** anacay.org, www.anacay.org (attached with `wrangler triggers deploy`; no DNS conflicts)
- **Also on:** https://anacay-org.martinez-giol.workers.dev

## Steps

1. Confirmed anacay.org is an active zone in the same Cloudflare account as anacay.com.
2. `git init`, first commit, created the public repo anacay/anacay-org and pushed.
3. `wrangler versions upload` refused (Worker didn't exist yet), so ran `wrangler deploy` once with routes commented out (workers.dev only). Previews: `10b8c777` (first page), `13ad482a` (new title and lede); approved.
4. Committed the index.html change, uncommented the routes in `wrangler.jsonc`, `wrangler versions upload` (`f969c248`), `wrangler versions deploy` at 100%, `wrangler triggers deploy`.

## Verification (2026-10-08)

- `curl -I https://anacay.org` → `HTTP/2 200`, `content-security-policy: default-src 'none'; style-src 'self'; img-src 'self'; base-uri 'none'; form-action 'none'; frame-ancestors 'none'; upgrade-insecure-requests`
- `curl -I https://www.anacay.org` → `HTTP/2 200`, same CSP
- Page links to `https://cpp.anacay.org/` and `https://anacay.com/`.
- `dig MX anacay.org` → `5 mta-gw.infomaniak.ch.`

## Not touched

- anacay.com, its zone, the `anacay-site` Worker, the anacay/anacay-web repo.
- Existing anacay.org DNS records (MX, SPF, DKIM, DMARC, autoconfig, autodiscover). Cloudflare Email Routing not enabled; cpp@anacay.org stays on Infomaniak.

## Updating

Edit `public/`, then `npx wrangler versions upload`, check the preview URL, `npx wrangler versions deploy`.
