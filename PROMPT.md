# Build prompt

Create a single-page, cinematic WebGL experience called **Minato**: a five-chapter dusk walk through a fictional island port town, of the kind you would find on a lantern-lit archipelago an hour after sundown. The result should feel like an editorial art book moving through a live 3D world, not a conventional product landing page.

## Experience

- Use a fixed full-viewport Three.js canvas as the environmental layer.
- Build the sea, the stone wharf, the two moles that hold the harbour, the machiya street, the storehouses, the beacon tower, the jetties, the moored boats, the strung paper lanterns, the cherries, the offshore torii, the moon, the mist and the falling petals procedurally.
- The sea gate is the subject of the whole composition: build it at ōtorii scale, standing in open water on
  raking legs, and shade it by tide height rather than as one surface — a gate that has stood in a strait is
  four materials stacked up its own columns.
- The water is the subject as much as the town: displace one plane by a sum of travelling waves in the vertex shader, solve its normal from the same function, and mirror that function in JS so every moored hull heaves and rolls in phase with the surface under it.
- Drive one continuous camera path from page scroll, and let it only ever move seaward. Each section should feel like a new composed shot rather than a hard scene replacement.
- Add restrained bloom, film grain, vignette, depth haze, warm paper-lantern light, cold moonlight over the strait, and a glitter path where the moon lands on the water.
- Keep the palette near-black violet, indigo, warm amber, bone white, sakura pink and one vermilion.

## Layout

- Structure the page as a hero, a sea-gate chapter, a lantern-street gallery, a shipwright's chapter atlas, an afterglow closing, and a manifesto footer.
- Use oversized left-aligned English headings, large vertical Japanese display type, small technical labels, chapter numbers, fine rules, and generous negative space.
- The gallery cards are not photographs: each frame is a hole punched in the page, with the harbour rendered into it through its own camera and a scissored viewport, so a card is a live window onto a different corner of the port.
- Layer alpha-preserving cut-outs of grass, cherry, maple, stones, walls, hills and lanterns at the bottom of the active viewport.
- Foreground layers should arrive at full visual opacity, remain pinned while their section is active, then fade and blur away during the handoff.
- Centre any play icon within the image frame itself, excluding the caption area.

## Motion

- Reveal headings word by word and supporting elements individually.
- Use slow, precise section transitions, subtle parallax, and eased camera interpolation.
- Let the navigation, chapter rail, cards, and foreground layers respond to the active section.
- Everything that burns should flicker on its own clock; nothing on one shared phase.
- Include reduced-motion behavior that preserves the complete reading experience.

## Interaction and quality

- Use a custom cursor only for fine pointer devices, with a drift of motes trailing it.
- Provide working anchor navigation, mobile navigation, responsive layouts, semantic landmarks, and accessible labels.
- Keep runtime assets local and use relative paths so the site works under a GitHub Pages repository subpath.
- Merge every repeated piece of geometry that never moves — a street drawn a shopfront at a time is several hundred draw calls of ornament.
- Avoid frameworks, build tooling, analytics, trackers, remote fonts, placeholder imagery, generic glassmorphism, excessive glow, and decorative motion without narrative purpose.
- Include no people. The port is read entirely through what they left lit.
- Verify at desktop and approximately 390 × 844, check all assets for 404s, parse every inline script, inspect the browser console, and test one complete scroll/navigation interaction before shipping.
