# Portfolio Website

This is a single, self contained website. All the markup, styling and
behaviour live in one file, `index.html`, so there is nothing to install
and nothing to build before it can go live.

## Publishing it with GitHub Pages

1. Create a new repository on GitHub. If you name it exactly
   `duaasghar.github.io`, GitHub will publish it at the root of that
   address automatically. Any other repository name works too, it will
   simply publish at `https://duaasghar.github.io/repository-name`.

2. Copy `index.html` into the root of that repository.

3. Commit and push it.

   ```
   git init
   git add index.html
   git commit -m "Add portfolio website"
   git branch -M main
   git remote add origin https://github.com/duaasghar/YOUR-REPO-NAME.git
   git push -u origin main
   ```

4. On GitHub, open the repository, go to Settings, then Pages, and under
   Build and deployment choose the main branch and the root folder, then
   save. GitHub publishes the site within a minute or two and shows the
   live URL on that same settings page.

## About the animated particle background

The hero and the rest of the page both run a small canvas based particle
network, nodes drifting continuously and connecting with faint lines
when close together, colored using the same four category colors used
throughout the site. In the hero it also reacts to the cursor, gently
pushing nearby particles away. This is plain JavaScript and the Canvas
API, no external library, so it costs nothing to load and works the
moment the page opens.

Two things worth knowing if you want to adjust it, both near the bottom
of the file in the `createParticleNetwork` calls:

- The hero network uses more particles, faster movement, and cursor
  reaction. The background network behind the rest of the page is
  deliberately sparser, slower and lower opacity, with no connecting
  lines, so it adds life without ever fighting with the text sitting on
  top of it. If it ever feels too busy or too quiet on your screen,
  the `count`, `maxSpeed`, `linkOpacity` and `particleOpacity` values in
  each call are the ones to change.
- Anyone with reduced motion turned on in their operating system gets a
  single still frame instead of continuous animation, which is standard
  accessibility practice and required no extra work on your part to
  keep working after this change.

## About the four interactive demos

The "Try it yourself" section runs four real machine learning models
directly in the visitor's browser, using Hugging Face's Transformers.js,
loaded from a public CDN. There is no backend server, no API key, and no
ongoing cost to you, and nothing a visitor types or uploads ever leaves
their device.

A few things worth knowing:

- **The first use of each demo downloads a small model**, roughly 14MB
  for the image classifier and 25MB for the search and semantic tools,
  each language pair in the translator downloads its own model too.
  This only happens once per visit, the browser keeps it cached after
  that.
- **This will not work if you open `index.html` directly from your
  computer's file system.** Browsers block cross origin script loading
  under a `file://` address as a security measure. It works correctly
  once the site is actually hosted over `https://`, which GitHub Pages
  gives you automatically, so please test the live, deployed link rather
  than the local file.
- **The English to Urdu translation model is likely available but was
  not something I could fully confirm before shipping this**, since the
  page I would need to check is not reachable from the environment I
  built this in. If it fails to load when you test it live, the
  translator already shows a friendly message pointing to French and
  Spanish instead, both of which I am confident are available. If Urdu
  does not work, let me know and I will swap in an alternative, or you
  can quietly remove that option from the dropdown in the translator
  panel.
- **The "Ask about my work" chat is deliberately not a free form
  chatbot.** It searches a small hardcoded corpus of your own portfolio
  content, described in the `KNOWLEDGE_BASE` array near the bottom of
  the file, by meaning rather than by keyword, and shows you the closest
  matching passage. This avoids the small in browser language models
  that exist today inventing wrong or strange answers on a page meant to
  represent you professionally. If you add new projects or experience
  later, add a sentence or two describing them to that same array so the
  search can find them.
- **The similarity threshold in the chat** (currently 0.2, in the
  `searchKnowledgeBase` result handling) decides when it says "I do not
  have a direct answer to that" instead of showing a match. Once the
  site is live, try asking it a range of real questions, and adjust that
  number up or down if it feels too willing or too reluctant to answer.

## Adding your photo

The hero section currently shows a placeholder monogram. To replace it,
add your image file to the repository, for example `photo.jpg`, then
find this block near the top of the body in `index.html`:

```html
<div class="photo-frame" id="photo-frame">
  <span class="photo-placeholder">DM</span>
  <div class="waveform-strip">
```

Replace the placeholder line with an image tag, keeping the waveform
strip underneath it exactly as it is:

```html
<div class="photo-frame" id="photo-frame">
  <img src="photo.jpg" alt="Dua Asghar Mughal">
  <div class="waveform-strip">
```

A square, well lit photo works best given the frame is a perfect square.

## Keeping the project links correct

The featured research section and the project reference grid link out to
two repositories:

- `github.com/duaasghar/non-invasive-haemoglobin-detection`
- `github.com/duaasghar/ai-projects`

If your actual repositories are named differently, search `index.html`
for each address and update the `href` values, or let me know the real
names.

## Making future edits

Everything lives in one HTML file with an embedded style sheet and
script, so most changes are a matter of finding the relevant section
using the labelled comments in the file, for example
`<!-- ============================ SKILLS ============================ -->`,
and editing the text directly. No build tools are required, open the
file in any text editor and refresh a browser to see the change, though
remember that the four demos specifically need to be tested on the live
deployed link rather than opened locally, for the reason explained above.
