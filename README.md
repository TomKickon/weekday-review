# Weekday Offer Review

Internal tool for Kickon Group marketing managers. Walks through Lightspeed data analysis and produces an executive-ready Word report on weekday food specials.

## What it does

1. Marketing manager picks their venue and uploads Lightspeed exports
2. Enters the specials they want to review (item name, special price, full price)
3. Tool analyses day-of-week trading data and shows a dashboard
4. Auto-recommends one of: Keep, Tweak, Replace, Kill
5. AI does competitor and supply chain research (live web search)
6. MM verifies the research, optionally adds context notes
7. For "Replace" decisions, AI proposes alternative specials
8. MM tweaks financial projection assumptions via sliders
9. Tool generates a downloadable Word document

## Lightspeed exports needed

- **Sales Summary** report (Reports → Sales Summary, click Download → CSV) - downloads as a ZIP containing day-by-day sales data
- **Product Mix** report (Reports → All reports → Product Mix Report, click Export CSV) - tells the tool which accounting groups are food vs beverage

Both files get uploaded into the tool. Eight weeks of data minimum for reliable averages.

## Hosting

Hosted on Netlify, deployed from this GitHub repo automatically on every push to main.

The Anthropic API key lives as a Netlify environment variable named `ANTHROPIC_API_KEY`. The `netlify.toml` build config replaces the `__ANTHROPIC_API_KEY__` placeholder in `index.html` with the real key before publishing.

### Updating the tool

1. Edit `index.html` (on GitHub directly, or locally then push)
2. Netlify auto-detects the change and redeploys in ~30 seconds
3. Live site updates

### Rotating the API key

If the key ever needs rotating (e.g. it leaks):

1. Anthropic Console → revoke old key, generate new one
2. Netlify → Site configuration → Environment variables → edit `ANTHROPIC_API_KEY` with the new value
3. Netlify → Deploys → "Trigger deploy" → "Deploy site"
4. Live site has the new key within ~30 seconds

### Local development

When `index.html` is opened directly (without Netlify build), the `__ANTHROPIC_API_KEY__` placeholder stays in the file. The tool falls back to prompting the user for an API key, which gets stored in the browser's localStorage.

## Costs

- Each report: ~10 cents (Sonnet 4.6 tokens + web search)
- All 13 MMs running 2 reports/month: ~$3/month total
- Netlify free tier hosting: $0/month
