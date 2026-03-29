# seanplusplus.github.io

Personal blog. Jekyll, GitHub Pages.

## Structure

- `_posts/` — blog posts in markdown (front matter: `layout: post`, `title`, `date`)
- `_layouts/` — page templates
- `_includes/` — partials (head, footer)
- `css/main.css` — all styles, no preprocessor

## Writing Posts

- First paragraph becomes the preview on the index page — keep it short
- Conversational tone, not corporate
- Code snippets should be lean — just enough to illustrate the point
- This is a public blog — no internal project names, ticket IDs, or employer-specific details

## Formatting

- Code blocks: keep lines short. Objects and arrays that render in `<pre>` will not wrap gracefully on mobile — shorten property values to avoid line breaks
- Tables: styled in `css/main.css` under `.post-content table` — light `#e8e8e8` borders, `8px 12px` cell padding, subtle `#f6f6f6` header background. No additional markup needed, standard markdown tables render styled automatically. Tables use `display: block; overflow-x: auto` so they scroll horizontally on mobile instead of blowing out the page width
- Jekyll uses kramdown with GFM input and rouge syntax highlighting (configured in `_config.yml`)
