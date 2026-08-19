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

## The body atlas

A second interactive below the specimens: a rotatable écorché you click to open
the muscles of that part in 3D.

The figure is not a mannequin with hotspots painted on it. The superficial
muscle groups — pectoralis major, the four segments of rectus abdominis, the
obliques, trapezius, latissimus, erector spinae, sternocleidomastoid, deltoid,
biceps, triceps, the forearm masses, gluteus maximus, the quadriceps heads,
hamstrings, gastrocnemius, tibialis anterior — are modelled as real bellies laid
over a deep core, so hovering names the muscle you are actually pointing at.
It breathes: the thorax lifts and widens at about thirteen breaths a minute,
which is also the diaphragm's own entry in the chest region.

Eleven regions — head and neck, chest, shoulder and arm, forearm and hand,
back, abdominal wall, hip and thigh, leg and foot, plus the heart, the lungs
and the gut wall.
Each opens its own diagram with the bones in place, the muscles named, and what
each one actually does. Switch on **Viscera** and the anterior trunk
wall turns to glass, putting the heart, the lungs and the gut coil in reach.

The three visceral regions are the point of the section. The heart is branched
striated cardiac muscle wound as a helical band. The gut wall is two smooth
muscle coats at right angles. The lungs hold no muscle of their own at all —
spiral smooth muscle in the bronchial wall sets airway calibre, and skeletal
muscle in the diaphragm does the pumping, so both tissue types appear in one
model. Drag **Constrict** there to close the airways the way asthma does.
Each region links straight back to its tissue specimen at the top of the page.

Regions are also listed as buttons beside the figure, so nothing depends on
hitting a narrow limb with a mouse.

Below both viewers: three full atlas plates (cell structure, filaments, membrane
systems, connective tissue, control, fibre types), a seventeen-row comparison
table, and clinical correlations tied back to the histology.

## How the rendering works

A small painter's-algorithm renderer on canvas 2D — no WebGL, no three.js. One
engine drives all three canvases (specimen, figure, region diagram), aliased
onto whichever view is being built.

- Parametric mesh builders for tubes (arbitrary path, radius, angular sweep,
  twist and ovality) and ellipsoids. On top of those sit two anatomy
  primitives: a bone (shaft with epiphyseal flares) and a muscle belly
  (fusiform fascicle, pale tendon at each end). Broad muscles are drawn as a
  fan of fascicle strips, which is what they are.
- Clicking the figure hit-tests the real geometry: every face carries a region
  id, and a click runs point-in-polygon over the last frame's faces from
  nearest to farthest. No invisible hotspot map to keep in sync with the model.
- A two-light rig in view space, so illumination tracks the camera the way a
  microscope lamp does: a key light, a fill light from the opposite side and
  below to keep the shadow side off flat black, a crude sky-occlusion term so
  undersides sit back, a warm rim that reads as translucent tissue, and a
  specular highlight for wetness.
- Camera moves are eased rather than cut. Choosing a region turns the figure to
  face it and swings the diagram to its best angle; a drag cancels the tween.
- Views render only while on screen, so the breathing figure isn't competing
  for frames while you're three sections away looking at a specimen.
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
