# cs5800-visualizations

Interactive problem visualizations for **CS 5800 — Algorithms** recitations at Northeastern.

One problem per week, each with a model students can poke at to build intuition
before writing anything down. The models deliberately **do not** run the
algorithm or solve the problem — they only report the consequences of the
student's own choices.

## Structure

```
index.html                        problem index (add a week here)
shared/site.css                   shared styles and the full color palette
shared/favicon.svg
problems/week-01-butterflies.html Week 1 — butterfly species judgments
vercel.json
```

No build step, no dependencies, no framework. Every page is plain HTML, CSS,
and vanilla JS, with all graphics drawn as inline SVG. Opening any `.html` file
directly in a browser works exactly like the deployed site.

## Local preview

Any static server works:

```sh
npx serve .
# or
python -m http.server 8000
```

## Adding a week

1. Copy `problems/week-01-butterflies.html` as a starting point. It keeps its
   own page-specific CSS and JS inline and pulls only `../shared/site.css`.
2. Append one entry to the `PROBLEMS` array near the top of the `<script>` in
   `index.html`:

   ```js
   {
     week: 2,
     title: 'Problem title',
     blurb: 'One or two sentences, plain language.',
     tags: ['greedy', 'proofs'],
     href: 'problems/week-02-slug.html'
   }
   ```

   The index sorts newest week first and builds the topic filter chips from the
   union of all `tags`, so nothing else needs touching.

## Conventions worth keeping

- **Colors come from the CSS custom properties** in `shared/site.css`
  (`--moss` for species A / satisfied, `--rust` for species B, `--alert` for a
  violated constraint, `--unknown` for undecided). Reuse them so a student
  reads the same meaning from the same color every week.
- **Everything is reachable by keyboard.** Interactive SVG elements carry
  `tabindex`, `role="button"`, an `aria-label`, and Enter/Space handlers. Since
  the graph is re-rendered from scratch on each interaction, focus is
  re-anchored afterwards via the `data-focus-key` attribute.
- **Respect `prefers-reduced-motion`.** Intro animations are skipped and
  layout transitions become instant snaps.
- **Live status text** sits in a `role="status" aria-live="polite"` region.

## Deployment

Deployed on Vercel as a static site — no build command, repo root as the
output directory. Pushes to the default branch ship to production; other
branches get preview URLs.
