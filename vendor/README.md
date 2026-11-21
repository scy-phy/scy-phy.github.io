Self-hosted third-party assets (so visitors' browsers never contact Google or a CDN).

- `cdnjs/ajax/libs/...`: the exact library versions the Academic theme expects (jQuery 3.4.1, Font Awesome 5.12, Fancybox 3.5.7, ...), taken from their npm packages. highlight.js is 10.7.3 (9.18.1 is not on npm). Paths mirror cdnjs; `data/assets.toml` (a modified copy of the theme's) points to them and carries the matching SRI hashes.
- `fonts/`: latin and latin-ext subsets from the Fontsource npm packages, referenced by `fonts/fonts.css` (included via `layouts/partials/site_head.html`).
