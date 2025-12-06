# Deploying torrence.biz to Cloudflare Pages — step-by-step

This guide assumes your domain torrence.biz is already added to Cloudflare and managed there.

1) Prepare your repository
- Create a Git repository (GitHub, GitLab, etc.) and push the site files to the branch you will use (e.g., main).
- Files to include in the repo root:
  - index.html
  - sitemap.xml
  - robots.txt
  - favicon.svg
  - (optional) CNAME — not required for Cloudflare Pages but harmless.

2) Create the Pages project
- In Cloudflare Dashboard → Pages → Create a project.
- Connect your Git provider and select the repository and the branch (e.g., main).
- Build settings:
  - If your site is plain HTML with no build step: leave "Build command" empty and set "Build output directory" to the repository root (often `.` or `/` as the UI suggests).
  - If using a static site generator, provide the build command and publish directory.

3) Deploy once (optional)
- Let Pages do an initial deploy. This gives you a pages.dev URL (e.g., `yourproject.pages.dev`) and confirms deploy works.

4) Add custom domain torrence.biz
- In the Pages project, go to Custom domains → Add a custom domain → enter `torrence.biz`.
- Cloudflare Pages will check ownership. You will usually see one of the following:
  - Pages will automatically create the required DNS record for you (if your domain is on Cloudflare).
  - Or Pages will show a single DNS record to add: typically a CNAME from the apex (flattened) or instructions to set a CNAME for `www` and/or a verification record.
- Follow Pages instructions to add/verify the DNS record(s). Because torrence.biz is on Cloudflare, Pages often configures this automatically.

5) Add the www domain (recommended)
- Also add `www.torrence.biz` to Pages as an alternate domain.
- In the Pages domain settings, set your preferred (primary) domain — either `torrence.biz` or `www.torrence.biz`.
- If you want `www.torrence.biz` to redirect to `torrence.biz` (or vice versa), set the primary domain in Pages and ensure both domains are added — Pages will serve a redirect to the primary domain.

6) DNS & proxy settings
- For Cloudflare Pages, the DNS records Cloudflare creates will typically be proxied (orange cloud), and Cloudflare will manage TLS automatically. You do not need to turn proxy off.
- If Pages gives instructions to add CNAMEs, use CNAME flattening for the apex if needed (Cloudflare handles this).

7) SSL / HTTPS
- Cloudflare will provision TLS certificates for Pages automatically — wait a few minutes after configuring the custom domain for the certificate to be issued.
- In Cloudflare Dashboard → SSL/TLS:
  - Set "Always Use HTTPS" to ON.
  - Turn on "Automatic HTTPS Rewrites".
  - Minimum TLS version: 1.2 or 1.3.

8) Redirects, caching, and cache purging
- If you change files, either wait for automatic cache updates or purge Cloudflare cache for updated files.
- For redirects between www and apex, set primary domain in Pages or create a Page Rule to redirect (if you prefer using Page Rules).

9) Email and mailto
- mailto: links will continue to work.
- MX records for email remain DNS-only (Cloudflare does not proxy MX). Leave any MX records as DNS only.

Troubleshooting
- Certificate mismatch or "Not secure": ensure the DNS records for the Pages-managed domain are proxied through Cloudflare and that you added the correct custom domain in Pages.
- 404 after deploy: check pages build settings and that files exist in the repo root (and that output directory matches).
- Changes not appearing: purge Cloudflare cache.

If you want, I can:
- Provide the exact DNS record values Pages requests during verification (after you start the domain add flow, copy the values here and I'll validate them).
- Prepare a GitHub repo push script for you to run locally to push these files quickly.
