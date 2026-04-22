# personal-site — Standing Instructions

## Deployment

**Vercel's GitHub integration does NOT reliably auto-deploy on push.**

After every code change + git push, always run:
```bash
vercel deploy --prod
```
from `personal-site/` to force a production deploy. Then verify with:
```bash
curl -s https://waldo.vanderlore.de/ | grep "<changed text>"
```

**Never claim the site is live based on a successful `git push` alone.** The git history and the live site can be out of sync. Always curl-verify the deployed URL before reporting success.

## Stack
- Astro 6, static output, deployed to Vercel
- Content Layer API: config at `src/content.config.ts` (NOT `src/content/config.ts`)
- Entries use `.id` not `.slug`
- Mermaid rendered client-side targeting `pre[data-language="mermaid"]`
- Dev: `npm run dev`
