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
- Renders the **defocused copy at the frame's own resolution** rather than at half of it, built from two separated blur iterations at widening steps. Blurring a half-res image and mixing three quarters of it back in does not read as a lens, it reads as a low-resolution frame. The circle of confusion falls off on a steeper curve to a lower ceiling and the in-focus zone is roughly twice as deep as a real lens of this length would give, because the frame is read for its detail as much as for its depth.
- Runs a six-level bloom, a **cylindrical-element flare** smeared across the brightest sources, **volumetric shafts** marched back toward the moon's live screen position so anything standing between the disc and the lens cuts its own shadow out of the light, and an **unsharp pass gated by the circle of confusion** — so tile, board grain and rope read as themselves at distance while the defocused background stays defocused.
- Generates every material canvas **supersampled**: the generators are written in 512-space and the canvas is allocated at twice that with the context pre-scaled, so the identical drawing lands on four times the pixels and the normal solved off it is read proportionally harder.
- Lights the whole harbour off a prefiltered radiance map generated at load, so stone, tile, lacquer and water
  all have a sky to reflect rather than being lit only where a lamp reaches — and lays a soft contact shadow
  into the water under every mass that stands in it.
- Renders the three **project cards as live viewports**: each frame is a hole punched in the layout, with the harbour drawn into it through its own camera and a scissored viewport, so the cards move when the harbour does.
- Layers editorial typography and alpha-preserving WebP foreground cut-outs over the 3D world, with section-specific fade and blur transitions. Once a bank has finished arriving it comes off its own transition and takes a live parallax, each plate travelling by its depth in the stage, so the near bank slides against the reading rather than with it.
- Stands **eleven scanned props** in the harbour — a timber derrick, a net rack, an ema board rack, a string of glass floats, a pair of stone lanterns at the head of the wharf, a machiya shopfront and a kawara roof run out along the east mole, an onigawara on its ridge, a smaller inner torii standing in the water short of the ōtorii, and two hulls moored on the tide. Each was generated at 300k polygons with an 8K PBR bake, then remeshed to a real height in metres with its origin on the ground, so a lantern is 2.4m because a lantern is 2.4m. The two hulls are handed the same wave sum as the procedural boats and heave in phase with the water under them.
- Grades **each chapter as its own shot** — exposure, saturation, bloom, vignette and flare are lerped along the same curve the camera rides, so the light arrives with the composition rather than snapping at a section boundary, and a half-stop lift is struck as the walk crosses into a chapter.
- Reads how fast the page is being scrolled and answers it the way a carried lens would: a degree of roll, a little more field, a wider anamorphic flare and more chromatic aberration at the edges, all of it gone within half a second of the scroll stopping. The focal plane chases the subject rather than being welded to it, so the harbour racks back into focus after a fast move.
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
| `?q=low` | Force the low-detail path (fewer particles, smaller water mesh, no shadows, no scanned props, single-rate textures). |
| `?post=0` | Bypass the bloom/grade chain and render straight to the frame. |
| `?shadow=0` | Turn off the shadow map. |
| `?nogl=1` | Simulate a machine with no WebGL and exercise the fallback. |
| `?adapt=0` | Pin the resolution governor so it stops trading pixels for frame rate. |
| `?dof=0` | Turn off the defocus, and with it the depth pass it needs. |
| `?props=0` / `?props=1` | Skip the scanned props, or force them onto the low path. |
| `?tex=1` | Generate the material canvases at single rate instead of supersampled. |

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
    ├── GLTFLoader.js
    ├── models/            # eleven scanned props, ~64 MB, fetched after first paint
    └── foreground/png/
```

Each prop carries a 4K base colour and a 2K normal. That is the ceiling worth
paying for at these viewing distances: the UV atlas covers the whole object, so
a 4K map already lands more texels on the nearest prop than the screen has
pixels to show them in. The 8K bake they came off is kept out of the shipped
files for a harder reason than download size — textures are not compressed in
GPU memory, so an 8192 base colour is 268 MB of VRAM before mipmaps, and eleven
props at 8K with their normals would ask for roughly 7.9 GB. At 4K the same set
asks for about 2 GB, which is why 4K is the number.

The props are the only substantial payload on the wire and they are deliberately
not on the critical path: the harbour is a complete composition without any of
them, they are requested 400ms after the first frame is on screen, and each one
fades up over its own second as it lands. They are skipped entirely on the
low-detail path, which is what a coarse pointer gets by default.

## Design and attribution

The harbour is an original design study, built as a companion piece to [Kage](../kage/) and inspired by Japanese port architecture and lantern-lit harbour towns. It is not affiliated with any real town, institution, tourism organisation or game.

The vendored Three.js r149 build and the r147 `GLTFLoader` beside it retain their MIT license notice and copyright attribution. The foreground cut-out plates and the scanned props in `minato-assets/` were generated with Meshy AI and processed for the web here — the plates keyed off a chroma background to alpha, the props remeshed, their maps resampled and a normal map solved back out of each base colour, since the remesh does not carry one through. The subset fonts and the foreground cut-out plates in `minato-assets/` are carried over from the Kage project in this repository and remain under its terms; everything else in the harbour is generated at runtime by the code in `index.html`.
