This folder is no longer required for the site to show real photography.

As of this round of updates, every photo spot on the site (hero, about,
services, products, news, and the blog articles) points directly at a
real, licensed stock photograph hosted on Pexels (a free-to-use stock
photo CDN — see https://www.pexels.com/license/), instead of a local
/assets/images/*.jpg file. Nothing needs to be dropped into this folder
for photos to appear.

WHY: hosted preview/sandbox environments used earlier in this project
blocked remote image URLs entirely, which is why the site originally
shipped with local-file placeholders only. The current build embeds
real hotlinked photo URLs directly, which display normally in any
regular browser with internet access (the constraint was specific to
one preview sandbox, not to browsers in general).

If a photo ever fails to load (the visitor is offline, a Pexels URL is
retired, etc.), the page automatically falls back to the same inline
brand-colored SVG illustration used before — nothing breaks either way.

TO SWAP IN THE CLIENT'S OWN PHOTOGRAPHY LATER:
Search each file's <script> for the `photo:` field (or, for the hero
and about images, the <img src="..."> attribute) and replace the
Pexels URL with a URL to the client's own image — either a link to
wherever they host it, or a local path like assets/images/hero.jpg if
this folder is populated and the site is served from a real web server
(not opened via file://, since local file paths inside a script-built
<img src> won't resolve reliably from file:// in every browser). Exact
spots to edit, all in index.html unless noted:

  Hero photo               <img class="hero-photo" ...> near the top of <body>
  About photo              <img class="about-photo" ...> in the About section
  Service cards (6)        SERVICES array, `photo:` field, near the bottom of <script>
  Product cards (3)        PRODUCTS array, `photo:` field
  News cards (3)           NEWS array, `photo:` field
  Blog article cards (8)   ARTICLES array in blog.html, `photo:` field

Team member photos are still not wired to files — the Team section on
About renders colored initials avatars. Ask if you'd like real
headshots wired in with the same pattern.
