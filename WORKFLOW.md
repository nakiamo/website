# Workflow (Hugo + Netlify)

This repo tracks the generated site output in `public/`. Use this workflow whenever you update content (e.g. `content/**/*.md`).

## Preview while editing (optional)

```bash
hugo server -D
```

Stop the preview server with `Ctrl+C` when you’re done.

## Build for publishing (writes to `public/`)

```bash
HUGO_ENV=production hugo --gc --minify
```

Quick sanity check (optional):

```bash
rg "localhost:1313" public/ || echo "OK: no localhost refs in public/"
```

## Commit + push

```bash
git add -A
git commit -m "Update content"
git push origin master
```

