# Topology3D — Website

Single-page landing site + printable measurement report. Serves a live
3D foot viewer showing a real iPhone-LiDAR scan fitted to the FIND
parametric foot model.

Hosted on GitHub Pages. No build step, no framework — pure HTML, CSS
and a single Three.js module loaded from the unpkg CDN.

> **Testing locally?** Do **not** double-click `index.html`. Browsers
> block `fetch()` from `file://` (CORS), so the 3D viewer will show
> "LOAD FAILED". Run a one-line static server instead:
>
> ```bash
> cd topology3d-web
> python3 -m http.server 8000
> # open http://localhost:8000
> ```

## Structure

```
topology3d-web/
├── index.html                       Landing page + Three.js viewer
├── report.html                      Printable measurement report
├── assets/
│   ├── fitted_foot.glb              Fitted FIND mesh (warm tone)
│   ├── fitted_foot_visibility.glb   Same mesh, per-vertex green/red coverage
│   └── logo.svg                     Text-based wordmark
├── CNAME                            Custom domain for GitHub Pages
└── README.md                        This file
```

## Running locally

Any static file server works. The GLBs must be served over HTTP
(browsers block `file://` fetches):

```bash
cd topology3d-web
python3 -m http.server 8000
open http://localhost:8000
```

## Deploying to GitHub Pages

1. Create a new public GitHub repository (e.g. `topology3d-web`).
2. Push the contents of this folder to the `main` branch.
3. In the repo: **Settings → Pages → Source → Deploy from branch → main / root**.
4. Wait ~1 minute. The site will be live at
   `https://<username>.github.io/topology3d-web/`.

### Custom domain (`topology3d.com`)

The `CNAME` file already contains `topology3d.com`. Once the repo is
live on GitHub Pages:

1. At your DNS provider, add an `A` record for `@` pointing to:
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```
2. Add a `CNAME` record for `www` pointing to `<username>.github.io`.
3. In GitHub **Settings → Pages**, enter `topology3d.com` as the custom
   domain and tick **Enforce HTTPS**.

## Updating the 3D model

Replace the two files in `assets/`:

| File | Source in pipeline |
| --- | --- |
| `fitted_foot.glb` | `FIND/results_scan1/fitted_foot.glb` |
| `fitted_foot_visibility.glb` | `FIND/results_scan1/fitted_foot_visibility.glb` |

To regenerate from OBJ/PLY output:

```bash
cd FIND
source venv/bin/activate
python results_scan1/convert_to_glb.py
cp results_scan1/fitted_foot*.glb ../topology3d-web/assets/
```

To update the measurement numbers, edit four places in `index.html`
(length / ball / heel / EU) and four in `report.html`, and the
three numbers in the accuracy table if relevant.

## Dependencies

- Three.js r160 — loaded from unpkg via `<script type="importmap">`
- No bundler, no transpiler, no framework

## Browser support

- Desktop: any modern Chromium, Firefox, Safari
- Mobile: iOS Safari 16+, Chrome Android
- The 3D viewer uses WebGL2 (available everywhere WebGL is, since 2022)

## License

Content and measurements &copy; 2026 Topology3D. Three.js is MIT-licensed.
The FIND parametric foot model is used under the terms of its original
license (University of Cambridge).
