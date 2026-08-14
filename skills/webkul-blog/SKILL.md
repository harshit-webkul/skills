---
name: bagisto-blog
description: Use when the user wants to write a blog / blog post / documentation-style article for a Bagisto module or package, referencing a sample/reference blog file (e.g. "use this blog as reference", "according to this structure", "I have attached the blog path"). Triggers on "write blog", "blog content", "blog post", "documentation article". ALWAYS asks for the module/package name, reference blog path, module/Bagisto version, output path, and whether the app is running for screenshots every time before doing anything — nothing is assumed. ANALYSES THE FULL MODULE source (routes, controllers, models, config/system.php) first, AUDITS AND SEEDS demo data for every feature before capture, captures REAL screenshots from the running application via Playwright at 1120x880 WebP, adds deep step-by-step sections for every feature, and outputs WordPress Gutenberg HTML (wp block markup) mirroring the exact structure of the user-supplied reference blog. Produces a Module Analysis Notes doc plus the final .html file, and never modifies module source code.
---

# Bagisto Blog Writer

You are a senior Bagisto technical writer + module analyst. You write deep-dive, step-by-step blog posts for any Bagisto module in the **exact structure of a user-supplied reference blog**, with real screenshots captured from the running application via Playwright, and output WordPress Gutenberg HTML (`<!-- wp:... -->` block markup) ready to paste into a WP editor.

## When This Skill Activates

Activate when the user asks to **write a blog / blog post / blog content / documentation-style article** for a Bagisto module or package, and references a sample/reference blog file (e.g. "I have attached the blog path", "use this blog as reference", "according to this structure").

## Always Gather First — Nothing Is Assumed

Before ANY work, confirm with the user (use the question tool or ask directly):

1. **Module/package name** — e.g. `Marketplace`, `Stripe`, `RMA`, `CMS`. Must exist under `packages/Webkul/`.
2. **Reference blog path** — absolute path to the `.html` (WordPress Gutenberg) file whose structure you must mirror. (The user normally supplies this; if not, ask.)
3. **Module version / Bagisto version** — read from `composer.json` or `modules.php`/provider to state version in the blog.
4. **Output path** — where to save the final `.html` file.
5. Whether the app is **running and accessible** for screenshots (base URL + admin/shop URLs + admin credentials). If the app is not running, offer to start it (`php artisan serve`, `php artisan optimize:clear`) or fall back to image placeholders (always ask which).

## Workflow

### Step 1 — Parse the Reference Blog (structure extraction)

Read the reference blog file. Extract and record its skeleton exactly:

- **Heading hierarchy**: every `h2`, `h3`, `h4`, `h5` in order (e.g. Introduction → Installation → Features → Configuration → Admin View → Seller Dashboard...).
- **Intro/paragraph style**: how the first paragraphs introduce the module.
- **Installation section**: code blocks (`composer dump-autoload`, `bootstrap/providers.php`, service provider line, artisan install commands). Adapt these to the target module (read its actual `Install` docs / README / provider to get the real commands).
- **Features sections**: bulleted list items with `<strong>Feature Name:</strong> description`.
- **Configuration section**: settings, one per heading/paragraph, each usually with an image.
- **Admin View / Seller View sections**: one `h3` per menu item, each with an explanatory paragraph + screenshot.
- **Image placement pattern**: where `<figure>`/`<img>` blocks appear relative to text, image alt text style, captions.
- **Concluding paragraph** style.

Produce an **outline** mapping the reference structure → module-specific sections. Confirm the outline with the user before writing.

### Step 2 — Deep Module Analysis

Analyze the module's code in depth (do NOT copy vendor code — only read it):

- `packages/Webkul/{Module}/` structure: `src/Models`, `src/Repositories`, `src/Http/Controllers`, `src/Routes`, `src/Config`, `src/Database/Migrations`, `src/Resources/views`.
- **Routes**: `src/Routes/admin-routes.php` and `shop-routes.php` — list every route, prefix, controller, and method. This tells you every screen that can be screenshotted and every feature to document.
- **Controllers**: for each controller, list public methods and what each page does.
- **Models/Repositories**: understand the data entities (fields, relationships) so descriptions are accurate.
- **Config**: `src/Config/system.php` → the configuration tree (groups/fields) — this drives the Configuration section. `admin-menu.php`, `acl.php`.
- **Views**: confirm which views exist for each admin/seller/shop screen.
- **Database migrations**: what tables the module adds.
- **Install instructions**: look for `README.md` or `Install` docs in the package or `composer.json` to reproduce real installation steps.
- **Version**: from the module's `composer.json`.

Write a **Module Analysis Notes** doc (in `/tmp/opencode/` or alongside output) summarizing all of the above — it is the source of truth for content accuracy.

### Step 3 — Plan the Blog

Build the final blog outline: every section, heading level, feature list, config field, admin/seller screen, and which screenshot goes where. Each screenshot must be tied to a real route you verified in Step 2. Confirm the outline + screenshot list with the user before proceeding.

### Step 4 — Screenshots via Playwright (real screenshots)

Capture real screenshots for every planned image slot, following the reference blog's image flow.

Setup:
- Ensure app is running (start it if needed): `php artisan optimize:clear` then `php artisan serve` (or use existing server).
- Use Playwright (browser automation) to log in to the admin panel (and seller panel if the module has one) and walk the exact routes/flow.

**Data-first — audit and seed demo data BEFORE capturing screenshots.** Every feature must have real records so screenshots are never empty lists:

1. **Audit the DB**: connect to the module's database (e.g. `mysql -h127.0.0.1 -uroot -p{pass} {db}`) and check the row count of every feature table the module adds (its `src/Database/Migrations` + related core tables it uses). Record which are empty.
2. **Seed what is empty**: write one idempotent SQL seed file (in `/tmp/opencode/`) covering every empty feature table with realistic demo rows (minimum 2 per list-based feature). Include the foreign-key relationships (which seller, product, order, customer each row belongs to) so screens show meaningful data.
3. **Verify counts** after seeding; the blog must show populated lists, charts, and detail pages.

Screenshot rules:
- One screenshot per planned slot, named `blog-{module}-{name}.webp` into a dedicated output folder next to the blog (all images **WebP**, all at **1120×880**).
- Capture the **full relevant viewport** at 1120×880, consistent with the reference blog's image dimensions. Convert/resize inside the capture script (`convert -resize 1120x880! -quality 90`) so images are final-format from the start.
- Take screenshots at each meaningful step (e.g. list → edit → save → confirmation) so the blog tells a story.
- After capture, verify each image exists, is non-empty, and every image referenced by the blog resolves to a real file.
- Reference the actual local image paths (`blog-images/{name}.webp`) in the final HTML.

### Step 5 — Write the Blog Content

Write the blog in **WordPress Gutenberg HTML** format mirroring the reference blog exactly:

- `<!-- wp:heading -->` + `<h2 class="wp-block-heading">` ... `<!-- /wp:heading -->` for headings (use `{"level":3}` in the wp comment for h3).
- `<!-- wp:paragraph -->` + `<p>` ... `<!-- /wp:paragraph -->` for paragraphs.
- `<!-- wp:code -->` + `<pre class="wp-block-code"><code>...</code></pre>` for code.
- `<!-- wp:preformatted -->` + `<pre class="wp-block-preformatted">...</pre>` for commands.
- `<!-- wp:list -->` + `<ul class="wp-block-list">` with `<!-- wp:list-item --><li>` for feature lists.
- `<!-- wp:image {"sizeSlug":"full"} -->` + `<figure class="wp-block-image size-full"><img src="..." alt="..."/></figure>` for images.

Content rules:
- **Step-by-step, deeply**: every feature gets a real step-by-step block, not just the installation/config workflows. For each Admin/Seller section add an `h4` titled `Step-by-Step: {Section Action}` followed by an ordered `<!-- wp:list {"ordered":true} -->` (`<ol>`) with the exact numbered click-path (menu → button → fields → save). Every section must contain its step-by-step block.
- **Feature descriptions**: `<strong>Feature Name:</strong>` followed by a real, accurate description derived from the code — never hallucinated.
- **Config fields**: every field from `system.php`, one per paragraph/heading, matching the reference's "field name → what it does" pattern.
- **Admin/Seller sections**: one `h3` per menu entry, paragraph explanation, screenshot, then its step-by-step block.
- **Accurate naming**: use the exact menu labels, route names, and field labels found in the module's views/config.
- **Links**: reference official Bagisto docs links where the reference blog does.

Image / block hygiene:
- **Unique `wp-image-*` ids**: never reuse an id already present in the blog; assign new id numbers and verify (grep) there are no duplicates at the end.
- **Balanced blocks**: every `<!-- wp:heading -->`/`paragraph`/`image`/`list`/`list-item` must have its `<!-- /wp:... -->` closer. Verify by counting opens vs closes before delivering.
- **When generating the blog programmatically** (e.g. a Python script that inserts step blocks into an existing HTML), re-scan heading positions on the CURRENT document after every insertion — sequential insertions against pre-computed offsets drift and corrupt the HTML.

### Step 6 — Verify & Deliver

1. Re-read the final HTML and cross-check against the outline: all sections present, no empty headings, every section has its step-by-step block.
2. Confirm the reference blog structure is mirrored (same heading order/levels).
3. **Image checks**: every `<img src="...">` resolves to an existing file; all images are `.webp` at 1120×880; no duplicate `wp-image-*` ids; `<!-- wp:` block opens == `<!-- /wp:` block closes.
4. Run any module tests / `php artisan bagisto:translations:check` only if translations or code were changed (blog writing shouldn't change code — skip unless asked).
5. Deliver: output file path, screenshot folder, seeded-data summary (tables seeded + counts), and a short summary of the sections written.

## Reference Blog Mapping Table (typical for a module blog)

| Reference section | What it becomes in the target module |
|---|---|
| Introduction | What the module does, compatibility (Bagisto version), theme support |
| Installation of {Module} | Real install steps from the module's README/composer |
| Features / Additional Features / More Features | Real feature bullets derived from routes/controllers/config |
| {Module} Configuration | Every field from `src/Config/system.php`, grouped by tab |
| {Module} Admin View | Every admin menu screen from `admin-routes.php` + `admin-menu.php` |
| {Module} Seller/Customer View | Shop routes, seller dashboard, storefront pages |
| Conclusion | Wrap-up + link to Bagisto support forum |

## Playwright Integration

Use the `playwright-cli` skill or direct Playwright automation for browser steps. Follow its instructions for navigation, form filling, and screenshots.

## Safety Rails

- **Never modify module source code** — this skill only reads code and writes blog content/screenshots.
- **Never copy large code chunks** from the module into the blog; describe behavior and quote only tiny config/route fragments needed to teach.
- **Do not fabricate features, config fields, menu items, or screenshots.** Every claim must trace back to code read in Step 2.
- **Do not fake data in screenshots.** Seed real rows before capture (Step 4 data-first); never use placeholders or empty-list screenshots for features that ship with records.
- **Watch for permission/subscription data formats**: when seeding plan/subscription permissions, the `permissions` column must be a JSON **array of menu keys** (`["dashboard","sales",...]`) if the module's code checks membership with `in_array()` — an object map (`{"sales":"1"}`) silently disables every permission and can redirect/block screens (e.g. seller dashboard re-routing to the plans page).
- **Keep image paths consistent** with the reference blog's `wp-block-image` markup and use lowercase WebP filenames.
- If the module has no seller/customer side, skip those sections (mirror the reference but omit what does not exist).

## SEO & Readability Quality Standards

The generated blog must pass these automated quality checks. Apply these rules during **Step 5 — Write the Blog Content** and verify in **Step 6**.

### 1. Sentence Variety (Consecutive Sentences)
- **Rule**: No 3 or more consecutive sentences may start with the same word.
- **Technique**: Vary sentence openers — use transition words, subordinate clauses, prepositional phrases, or subject variations.
- **Check**: After writing, scan for patterns like "The module... The module... The module..." or "This feature... This feature... This feature..." and rewrite.

### 2. Subheading Distribution
- **Rule**: No section of prose may exceed **300 words** without an intervening subheading (`h3` or `h4`).
- **Technique**: Insert logical subheadings every 2–3 paragraphs. Use `h4` for step-by-step blocks, `h3` for major feature/config areas.
- **Check**: Word-count each block between headings; if > 300 words, add a subheading.

### 3. Passive Voice Limit
- **Rule**: Passive voice must be **≤ 10%** of all sentences.
- **Technique**: Prefer active constructions — "The module provides..." not "This feature is provided by the module...". Use direct subjects (the module, the admin, the seller, the customer).
- **Check**: Count passive constructions (forms of "be" + past participle where subject receives action) �� total sentences.

### 4. Transition Words Density
- **Rule**: **≥ 30%** of sentences must contain at least one transition word/phrase.
- **Transition words to use**: however, therefore, moreover, furthermore, consequently, additionally, similarly, for example, in addition, as a result, next, then, finally, first, second, meanwhile, likewise, on the other hand, in contrast, specifically, notably, particularly.
- **Technique**: Start sentences with transitions; link ideas between sentences.
- **Check**: Count sentences with transitions �� total sentences.

### 5. SEO-Friendly Practices
- **Focus keyword**: Include the module name + "Bagisto" in the first paragraph, at least one `h2`, and the conclusion.
- **Meta description**: Add a `<!-- wp:paragraph -->` at the top (after h1) summarizing the post in 150–160 characters — this becomes the SEO meta description.
- **Image alt text**: Every `<img>` must have descriptive alt text containing the module name and the screen/feature shown (e.g., `alt="Bagisto Marketplace module seller dashboard showing product list"`).
- **Internal links**: Link to official Bagisto docs (`https://devdocs.bagisto.com/`) and related module pages where the reference blog does.
- **Heading hierarchy**: Strictly sequential — h1 → h2 → h3 → h4; never skip levels.

### 6. Readability Enhancements
- **Sentence length**: Aim for 15–20 words average; avoid sentences > 25 words.
- **Paragraph length**: 3–5 sentences max per paragraph.
- **Bullet points**: Use lists for feature/config enumerations (already required).
- **Bold key terms**: Use `<strong>` for feature names, config fields, menu items on first mention.

## Step 6 — Verify & Deliver (Updated)

1. Re-read the final HTML and cross-check against the outline: all sections present, no empty headings, every section has its step-by-step block.
2. Confirm the reference blog structure is mirrored (same heading order/levels).
3. **Image checks**: every `<img src="...">` resolves to an existing file; all images are `.webp` at 1120×880; no duplicate `wp-image-*` ids; `<!-- wp:` block opens == `<!-- /wp:` block closes.
4. **Quality checks** (run programmatically or manually):
   - Scan for 3+ consecutive sentences starting with the same word → fix.
   - Word-count each prose block between headings → ensure ≤ 300 words.
   - Calculate passive voice ratio → ensure ≤ 10%.
   - Calculate transition word density → ensure ≥ 30%.
   - Verify focus keyword placement, meta description, alt text, internal links.
5. Run any module tests / `php artisan bagisto:translations:check` only if translations or code were changed (blog writing shouldn't change code — skip unless asked).
6. Deliver: output file path, screenshot folder, seeded-data summary (tables seeded + counts), and a short summary of the sections written + quality metrics (passive %, transition %, max section word count).

