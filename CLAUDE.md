# manpearl-website

Personal website for manpearl.com.

## Stack

- Vanilla HTML5 + CSS3 (no frameworks, no build step, no JavaScript)
- Hosted on Railway as a static site
- Custom domain: manpearl.com via OpenSRS/Tucows DNS
- Email managed by Google Workspace (do not disturb MX records)

## Files

- `index.html` - single-page site, all markup
- `style.css` - dark theme with CSS variables, responsive
- `README.md` - deployment and setup instructions

## Local dev

```
python3 -m http.server 8080
```

Then open http://localhost:8080

## Code rules

- No JavaScript unless explicitly added later
- After HTML/CSS edits: `npx prettier --write index.html style.css`
- No em-dashes or en-dashes anywhere
- Conventional commits: `feat(scope): subject`
