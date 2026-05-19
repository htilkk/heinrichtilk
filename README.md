# heinrichtilk.com

minimal videographer portfolio. plain html/css. no framework, no build step.
edit any file in a text editor, refresh browser, done.

## file structure

```
heinrichtilk/
├── index.html              ← home (hero reel)
├── projects.html           ← list of all projects
├── about.html              ← about page
├── contact.html            ← contact page
├── style.css               ← all styling
├── README.md
└── projects/
    └── example.html        ← project page template
```

## first things to change

1. **the videos.** see "adding videos" below.
2. **your email + socials** in `contact.html`.
3. **the about text** in `about.html`.
4. **your photos** for the about page (drop into `media/about/`).
5. **rename the projects** in `projects.html` and fill in real project pages.

---

## adding a new project (the important bit)

every time you finish a new piece and want it on the site:

1. **copy** `projects/example.html` to `projects/your-project-slug.html`
   (use lowercase, hyphens, no spaces — e.g. `dunes-2026.html`)
2. **open the new file** and edit:
   - the `<title>` tag (browser tab name)
   - the project name in `<h1>`
   - the year
   - the vimeo/youtube embed URL (see "embedding video" below)
   - the credits in the `<dl>` block — add or remove `<div>` blocks as needed
   - the description paragraphs
3. **open `projects.html`** and add a new `<a class="project-card">` block
   at the top of `.project-grid`:
   ```html
   <a href="projects/your-project-slug.html" class="project-card">
     <div class="project-thumb">
       <img src="media/your-thumb.jpg" alt="" />
     </div>
     <span class="project-name">your project name</span>
   </a>
   ```
   for a video thumbnail instead of a static image, swap the `<img>` for:
   ```html
   <video autoplay muted loop playsinline>
     <source src="media/your-clip.mp4" type="video/mp4" />
   </video>
   ```
   keep thumbnails roughly 16:10 ratio (e.g. 1200x750px) for consistency.

that's it. save the files, refresh your browser, the new project is live.

---

## adding videos

### homepage hero reel

option a (host the file yourself):
1. export a 10–30 second silent loop, 1080p, h.264 mp4, under ~15mb
2. save it as `media/reel.mp4` (create the `media/` folder if it doesn't exist)
3. update the `poster="..."` attribute in `index.html` to a still frame url

option b (embed from vimeo — recommended for full-length reels):
- see the commented snippet at the bottom of `index.html`
- the magic parameter is `background=1` — it hides all vimeo player chrome
  and autoplays/loops/mutes automatically

### project page videos

each project page embeds from vimeo or youtube via iframe:

```html
<!-- vimeo -->
<iframe src="https://player.vimeo.com/video/123456789?title=0&byline=0&portrait=0">

<!-- youtube -->
<iframe src="https://www.youtube.com/embed/abc123xyz">
```

find your video id in the vimeo/youtube url. the query string parameters
hide the title, uploader byline, and avatar — making the player feel
native to your site instead of like an external embed.

---

## adding the real neue haas grotesk

right now the site uses helvetica neue + helvetica + inter as fallbacks.
to use the actual licensed font:

1. buy a webfont license from monotype.com or fonts.com for `neue haas grotesk display` (or the version you want — there's also `text`, optimized for body copy)
2. they'll send you webfont files (`.woff2` and `.woff`)
3. drop them in a `fonts/` folder
4. add this at the top of `style.css` (above the inter `@import`):

   ```css
   @font-face {
     font-family: 'Neue Haas Grotesk';
     src: url('fonts/NeueHaasGrotesk-Bold.woff2') format('woff2'),
          url('fonts/NeueHaasGrotesk-Bold.woff') format('woff');
     font-weight: 700;
     font-display: swap;
   }
   @font-face {
     font-family: 'Neue Haas Grotesk';
     src: url('fonts/NeueHaasGrotesk-Regular.woff2') format('woff2'),
          url('fonts/NeueHaasGrotesk-Regular.woff') format('woff');
     font-weight: 400;
     font-display: swap;
   }
   ```

the rest of the css already references `'Neue Haas Grotesk'` first in the
font stack — once you add the `@font-face`, it'll just work.

---

## tweaking the gradient

the gradient lives in `style.css`, in the `body` selector:

```css
background:
  radial-gradient(ellipse 90% 80% at 100% 100%,
                  rgba(0, 0, 255, 0.10) 0%,    ← blue strength
                  rgba(0, 0, 255, 0.04) 35%,   ← mid-gradient
                  var(--off-white) 70%);       ← where it fades to off-white
```

- want more blue? bump `0.10` to `0.15` or `0.20`
- want it from a different corner? change `at 100% 100%` (bottom-right) to
  `at 0% 0%` (top-left), `at 50% 100%` (bottom-center), etc.
- want it linear instead of radial? swap `radial-gradient(...)` for
  `linear-gradient(135deg, rgba(0,0,255,0.10), var(--off-white))`

---

## tweaking the spacing

two custom properties at the top of `style.css` control everything:

```css
--gutter: clamp(16px, 2.4vw, 32px);   /* gap between elements */
--frame:  clamp(20px, 3.2vw, 44px);   /* outer page padding */
```

bigger numbers = more whitespace.

---

## deploying for free

**easiest path: netlify drop**
1. zip this whole folder (or just drag the folder, not the zip)
2. go to https://app.netlify.com/drop
3. drag the folder onto the page
4. you'll get a live url in ~30 seconds (e.g. `dazzling-llama-123.netlify.app`)

**connecting heinrichtilk.com:**
1. buy the domain from porkbun or namecheap (~$12/year)
2. in netlify: site settings → domain management → add custom domain
3. follow their instructions to update your domain's dns
4. ssl certificate is automatic and free

other free hosts that work identically: cloudflare pages, vercel,
github pages. all are fine.

---

## quick reference: the design tokens

```css
--blue:       #0000ff;    /* the accent color */
--off-white:  #f4f3ef;    /* the page background */
--ink:        #0a0a0a;    /* main text */
--ink-muted:  #6a6a68;    /* secondary text */
--hairline:   rgba(10, 10, 10, 0.12);  /* dividers */
```

change any of these in `:root` at the top of `style.css` and they'll
update everywhere the site uses them.
