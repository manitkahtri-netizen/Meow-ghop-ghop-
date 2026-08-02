# Muscle Tissue in Three Dimensions

An interactive histology page covering the three types of muscle tissue — skeletal,
cardiac and smooth — plus the sarcomere that drives the two striated ones.

`muscle-tissue-3d.html` is a single self-contained file. No build step, no
dependencies, no network requests, no image assets. Open it in a browser.

## What's in it

A darkfield "specimen stage" holding four rotatable 3D models, each generated as
geometry every frame so the contraction is real deformation rather than a canned
animation. Drag to rotate, scroll or pinch to zoom, click any annotation to jump to
its explanation. Arrow keys rotate when the canvas has focus.

| Specimen | Shows |
| --- | --- |
| 01 Skeletal | Cut-away through epimysium → perimysium → fascicle → fibre; A/I banding, endomysial sleeve, peripheral nuclei. The twitch shortens the fibre to 66% of rest and the belly fattens with it. |
| 02 Cardiac | Branching network, stepped intercalated discs, one central nucleus per cell, and a conduction wave that fires the cells in sequence. Rate is adjustable 40–190 bpm. |
| 03 Smooth | Staggered spindles under a peristaltic wave, shortening up to 68%, with the nucleus twisting into its corkscrew. |
| 04 Sarcomere | Sliding filaments at 1 unit = 1 µm. Contracting from 0 to 100% takes the sarcomere 2.56 → 1.72 µm and the I band 0.86 → 0.02 µm while the A band holds at 1.70 µm. |

Below the viewer: three full atlas plates (cell structure, filaments, membrane
systems, connective tissue, control, fibre types), a seventeen-row comparison
table, and clinical correlations tied back to the histology.

## How the rendering works

A small painter's-algorithm renderer on canvas 2D — no WebGL, no three.js.

- Parametric mesh builders for tubes (arbitrary path, radius, angular sweep,
  twist and ovality) and ellipsoids.
- Per-face flat shading: Lambert key light in view space, so the illumination
  tracks the camera the way a microscope lamp does, plus a cool rim term and a
  specular highlight for wet tissue.
- Back-face culling, then a single depth sort across all faces per frame.
- Central nuclei render in a second overlay pass so they read *through* the
  sarcoplasm without making the whole cell translucent.
- Polygons are expanded by a third of a pixel from their centroid so adjacent
  quads overlap instead of leaving antialiased hairline seams. That replaced a
  same-colour stroke on every face and roughly halved the per-frame cost.
- Static scenes only redraw on interaction; animated ones run on rAF.

Annotations are 3D-anchored, projected each frame, and hit-tested for clicks.

## Themes and layout

The page chrome follows the viewer's light or dark theme through a token layer.
The viewport itself stays dark in both — it is a darkfield scope view, and that
is a deliberate choice rather than a missing theme. Reduced-motion preferences
suppress the automatic animation; every control remains usable by hand.

## Note on the file

The file omits `<!doctype>`, `<html>`, `<head>` and `<body>` because it is also
published as a hosted artifact, where that skeleton is supplied by the host.
Browsers fill it in when the file is opened directly, so it works either way.
