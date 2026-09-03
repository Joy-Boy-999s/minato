# B. Neeraj Kumar — Portfolio

A single-page portfolio for a full stack developer, set inside a live WebGL harbour at dusk. The résumé is the
content; the port is the room it is read in.

**Sections:** About · Projects · Experience · Contact — driven by one continuous camera move down the harbour
as the page scrolls.

## Editing the content

Every word of the résumé lives in the HTML at the top of `index.html` — the hero, `#about` (summary, figures,
skills board), `#projects` (three cards and their notes), `#experience` (five plates), `#contact`, and the
footer. Nothing in the 3D scene needs touching to change any of it.

The three project cards are `[data-view="0…2"]`; the number points at a camera defined in `buildCards()`, which
is the corner of the harbour that card is a window onto.

## Deploying

It is a static folder — `index.html` plus `minato-assets/`. Any static host works. For GitHub Pages, push the
folder contents to the repo (`.nojekyll` is already there so the asset directory is served as-is); every path
is relative, so it also works from a repository subpath.

## What it does

- Moves a live WebGL camera down a harbour as the page scrolls, from the head of the wharf out through the mouth and past the gate.
- Builds the sea, the stone wharf and its two moles, the machiya street, the storehouses, the beacon tower, the jetties, the moored boats, the strung chōchin, the cherries, the offshore torii, the moon and its glitter path procedurally, at runtime.
- Displaces the water in the vertex shader by a sum of four travelling waves, and mirrors that same wave sum in JS so every moored hull heaves and rolls in phase with the surface under it.
- Stands a seventeen-metre **ōtorii** in the strait, shaded by a fragment shader that reads the tide off world
  height: submerged and glossy at the foot, a weed belt where the water spends its day, a chalky barnacle
  crust above that, and bleached lacquer at the top — with the bands wobbling on a wave read off world x and z,
  because a tide line drawn straight is a painted stripe.
- Builds each **wasen** from solved sections — beam falling off along the keel, gunwale lifting at the ends,
  a raked transom aft so she is not a canoe — then fits her out with thwarts, frames, a keelson, a woven toma
  canopy on its hoops, a sculling oar, a stern chōchin and a mooring line hanging in its own catenary. She
  floats on a real freeboard, and her planking carries a wet band and a tide stain solved in the shader.
- Draws every chōchin in the port as **one instanced mesh**, swayed and guttered in its own vertex shader off a
  per-instance phase, and lights the paper from the inside — brightest across the belly, dying into the lacquer
  collars, and dimmer at the silhouette than face on, which is what a diffuser lit from within actually does.
  Three draw calls and no per-frame CPU work for the whole street.
- Covers the quay in the things a working port is actually made of — crates and stacks, barrels, coils of rope,
  baskets, drying racks with nets that sag on their own rails, an andon and a signboard on every shopfront,
  onigawara on every ridge, glass floats moored in strings, gulls wheeling on their own circles, and the
  petals the cherries lose lying flat on the tide. All of it merged or instanced by material: four draw calls
  for the cargo, one each for the floats, the gulls and the drift, and nothing at all for the shop lanterns,
  which ride the runs already going out.
- Lights the whole harbour off a prefiltered radiance map generated at load, so stone, tile, lacquer and water
  all have a sky to reflect rather than being lit only where a lamp reaches — and lays a soft contact shadow
  into the water under every mass that stands in it.
- Renders the three **project cards as live viewports**: each frame is a hole punched in the layout, with the harbour drawn into it through its own camera and a scissored viewport, so the cards move when the harbour does.
- Layers editorial typography and alpha-preserving WebP foreground cut-outs over the 3D world, with section-specific fade and blur transitions.
- Includes chapter navigation, a responsive mobile layout, reduced-motion behaviour, a no-WebGL fallback that keeps the whole reading experience, and a custom cursor for precise pointer devices.

## How it is made

Minato is a deliberately small static site. `index.html` contains the document structure, CSS, procedural scene construction, scroll choreography, and interaction logic. A vendored Three.js r149 build provides WebGL rendering without a package manager or build step.

Every surface in the harbour is generated into a `<canvas>` at load and uploaded as a texture — granite, cedar boards, weathered vermilion lacquer, roof tile, harbour plaster, washi lantern paper, indigo noren, the moon's own disc, the dusk sky, and the near-plane cut-outs of grass, stone and blossom. The only bitmaps shipped are the foreground plates the page hangs over the reading.

## Run locally

From the repository root, run:

```bash
python3 -m http.server 4176 --bind 127.0.0.1
```

Then visit [http://127.0.0.1:4176/](http://127.0.0.1:4176/).

There is no build step, environment variable, analytics script, or runtime network dependency. Python is used only to serve the static files locally; any equivalent static server will work.

## Query parameters

Useful while working on the scene:

| Parameter | Effect |
| --- | --- |
| `?shot=0…5` | Park the camera on one chapter's waypoint, fully revealed, for review. |
| `?q=low` | Force the low-detail path (fewer particles, smaller water mesh, no shadows). |
| `?post=0` | Bypass the bloom/grade chain and render straight to the frame. |
| `?shadow=0` | Turn off the shadow map. |
| `?nogl=1` | Simulate a machine with no WebGL and exercise the fallback. |
| `?adapt=0` | Pin the resolution governor so it stops trading pixels for frame rate. |

`window.__minato` exposes the rig, world handles, camera and renderer once the harbour is up.

## Project structure

```text
minato/
├── index.html
├── PROMPT.md
├── README.md
└── minato-assets/
    ├── fonts.css
    ├── three.min.js
    └── foreground/png/
```

## Design and attribution

The harbour is an original design study, built as a companion piece to [Kage](../kage/) and inspired by Japanese port architecture and lantern-lit harbour towns. It is not affiliated with any real town, institution, tourism organisation or game.

The vendored Three.js r149 build retains its MIT license notice and copyright attribution. The subset fonts and the foreground cut-out plates in `minato-assets/` are carried over from the Kage project in this repository and remain under its terms; everything else in the harbour is generated at runtime by the code in `index.html`.
