All in One SEO (AIOSEO) — Common Settings
=========================================

Title format
- {ページ名}｜TOKYO LABEL FUWALY
- In AIOSEO > Search Appearance > Global Settings > Title Format, set Site Title and per-post/page format accordingly.

Meta description (base)
- Use a concise brand-wide description (override per page later):
  清楚・未経験・素人を基調にした高級デリヘルのTOKYO LABEL FUWALY。
- Configure in AIOSEO > Search Appearance > Global Settings > Meta Description (fallback) and override on each page as needed.

Canonical
- Principle: canonical to / (認証ページ) to aggregate signals to root.
- Exception: 認証ページ自身のみ / 固定（= https://aoyama-fuwaly.com/）。
- If AIOSEO enforces page-specific canonicals, add a filter in theme to return home_url('/') for all pages under /top/* and other child routes.

OGP
- og:image (default): brand common sakura image — e.g. /assets/ogp/ogp-brand-sakura.jpg
- Auth (認証ページ) only: dedicated OGP — e.g. /assets/ogp/ogp-auth-sakura.jpg
- Set default image in AIOSEO > Social Networks > Facebook/Twitter; override the Auth page with page-level Social settings.

Notes
- This prototype also adds basic <meta> tags in HTML heads as a visual cue; AIOSEO will override them at runtime.
- Keep page slugs and internal routes consistent with canonical policy.

