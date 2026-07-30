# Master Playbook, Building a Brand Document as a Single-File Web App

A reusable operating manual distilled from the Roofbuddy Consultant Handbook build. Hand this to a new project to reproduce the same working model, guardrails, and design standards.

## 1. The working model, three roles
- Creative Director (Jonathan): owns vision, brand, every final call. Sets direction, reviews live output, decides.
- Tango (design consultant, a Claude chat): translates direction into precise buildable instructions. Reads source, maps structure, recommends with a point of view, hands every task to the builder as one copy-paste prompt. Does not touch the repo. Flags trade-offs.
- Cash (builder, Claude Code in the local repo): executes prompts, commits, opens the page. Confirms real filenames, reports back.
- The loop: Tango produces a one-click copy box per change. Director copies it to Cash. Cash builds, commits, opens. Director reviews, feeds the next instruction to Tango.

## 2. Guardrails held throughout
- Delivery: every build instruction is one copyable prompt block. Long ones scroll. Every prompt ends: "Commit the change, then open the page so I can see it live."
- Content is verbatim: no paraphrasing or re-punctuating. Retain all formatting (bold, headings, labels, tables, lozenges) as per source.
- Content must physically reach the builder: inline it in the prompt between ===CONTENT START=== / ===CONTENT END=== markers. A workspace file is not in the repo.
- Flag, do not silently fix: reproduce source quirks verbatim, surface them for a decision.
- Technical hygiene: check inlined content for & and < before handing over. Confirm real asset filenames.
- Style: no em dashes. Concise. Genuine recommendations. Direct critique.

## 3. Brand / design system
- Core tokens: navy #161823, green #40BB6A, Poppins (body/headings), Bebas Neue (ghost numerals), hard corners (border-radius 0), no shadows. One intentional exception, on record: the global search (the pill input and its Spotlight results surface) uses rounded corners and subtle depth; everything else stays square and shadowless.
- Per-chapter colour via one CSS var (--c) so components inherit automatically.
  Ch2 #009444, Ch3 #1a6e77, Ch4 #33586b, Ch5 #2aa8e0, Ch6 #3d3f63, Ch7 #6f446d, Ch8 #ed1456, Ch9 #822c3b, Ch10 #d15c31, Ch11 #d5966d, Ch12 #d49a2c. (Ch1 #40BB6A.)
- Hero: image at brightness ~0.6, solid chapter-colour layer (mix-blend-mode: color, opacity 1), no navy bottom gradient, Bebas ghost numeral (opacity raised), Poppins title ending in a full-stop, depth deepened ~20%.
- Numbered section lozenges. Caption style: Poppins 400 italic, 12px/22px, rgb(136,146,164).
- Type floor: nothing below 12px anywhere. Body copy and list items (bullets and numbered steps) 14px so bullets match the body exactly; top nav 15px; 12px italic captions are the floor; headings, sub-nav pills, section numbers and Bebas numerals keep their own sizes.

## 4. Content markup convention (role tags map to existing classes)
- {SECTION n.n | Heading.} numbered lozenge + chapter-coloured heading
- {LEAD} navy bold lead paragraph
- {SUB} navy bold standalone subheading
- {SUBLINE: Label: Descriptor.} label to first colon in chapter colour, remainder navy bold
- {LABEL: x:} chapter-coloured bold inline run-in, continues regular
- {BULLETS} regular; {BULLETS-BOLD} navy bold
- {GLOSSARY-ROW: Term. | img:} a tile in the image grid
- {IMAGE-PLACED} / {IMAGE-FULL} placed images, lightbox
- {CAPTION} caption style; {COMPARE-TABLE} typed 2-col; {PULLQUOTE} large quote
- Two heading colours: chapter-colour for section headings, sub-titles, category labels; navy bold for leads, descriptors, standalone subheads.

## 5. Component library (all themed by var(--c))
- Image / glossary grid: uniform 4:3 tiles (object-fit cover), repeat(auto-fit, minmax(200px,1fr)) gap 16px, 3-up desktop down to 1. Tile shows term + one-line identifier; full definition lives in the lightbox. Tap/click/keyboard opens detail. Hover (desktop only): subtle scale(1.02) lift + top-right chapter-colour expand icon, no outline, cursor zoom-in. Rationale: field guide, scan to identify.
- Sequence vs category: ordered sequences (rust Stages 1 to 4) keep order; not everything with images is a category.
- Accordions: hairline dividers, hard corners, chapter accent, rotating chevron, visible one-line teaser when collapsed, independently openable. Searchable hard rule: all text stays in DOM (collapse via max-height, never display:none, no lazy render); a hit opens the row then scrolls then highlights.
- Section sub-nav: solid chapter-colour tabs, white text, hover darker shade scoped to @media (hover: hover), active holds darker shade, square, no shadow, keyed to var(--c). Desktop: an even flex row of number-only pills with a hover tooltip. Mobile: a single-row horizontally scrollable strip (swipe); tabs never wrap and the strip scrolls internally so the page never scrolls sideways; scrollbar hidden cross-browser; the active tab auto-centres when its section becomes active; mask-based edge fades (toggled from scrollLeft, right by default, left once scrolled, both in the middle) signal off-screen tabs; mobile tabs show number plus section title. All tab text stays in the DOM (scroll and collapse via layout, never display:none) so search still sees every tab.
- Full-width chapter layout (opt-in .chapter.full-width): full-bleed hero, sub-nav and grids with a contained, centred reading measure. The content-area widens (~1440px, margin auto); prose children cap at ~760px and centre via margin-left/right auto so alignment matches every other chapter; structural blocks (image grids, compare tables, accordions) set max-width:none to fill the width, and grids raise the tile min (~240px) for a clean 4-up to 5-up. Scoped to .full-width so other chapters keep the narrow centred column; the caps only bite on wide viewports, so mobile is unaffected.
- Lightbox: navy ~92% backdrop, image-only and detail modes, close X/Esc/backdrop. Prev/next arrows step the set the image belongs to (its grid, or the section's figures) in DOM order, wrap at the ends, keyboard left/right, hidden when the set holds one image; arrows square, no shadow, at the backdrop edges so they never cover the image.
- Responsive data table to cards (criteria/reason tables): real 3-col table on desktop, square, hairline row dividers, sticky header filled var(--c) with white text, confined to the reading column. On mobile reflow the same table with display:block into stacked cards, reason plus badge as a filled var(--c) card header, per-field labels via data-label ::before. Never display:none the content; move the thead sr-only so it stays indexable. Status badges are small square uppercase tags, filled var(--c) or a muted outline for an unsettled state; a short left-aligned legend explains them. Merged source cells become a rowspan, shown once with an "applies to both" note; nested sub-lists render lighter-weight and regular.
- Content locked inside an image: mirror it as DOM text (an org chart becomes a grouped name/title roster) and wire it into the search index, so every value is searchable. Per-chapter var(--c), square, no shadow.
- Global fixed controls (back-to-top): a single fixed button outside the chapters cannot inherit a chapter's --c by cascade. activate() mirrors the active chapter colour onto :root as --active; key the hover to var(--c) with an --active fallback, scoped to @media (hover: hover), resting state unchanged.
- Chapter index overlay: fluid/responsive grid that scales to the viewport (4-up stepping down to 3, 2, then 1) so the bottom row never crops, tiles enlarged ~60% via wider container, bold titles ending in full-stop, active nav underline tracks current view, no X, backdrop click closes, Esc closes, tile/nav clicks shielded.
- Site-wide search: the entry is a pill input (radius-0 exception, on record), no placeholder text, green magnifying-glass icon, floating directly on the backdrop with no container/panel around it. Results are a unified macOS/iOS-26 Spotlight-style surface (rounded, subtle depth, the exception above) that expands smoothly: grouped rows of green icon + bold title + muted location label, with top-hit emphasis, driven by keyboard up/down/Enter/Esc. Index all text by walking the DOM at load; click or Enter switches view, opens accordion/lightbox if needed, reveal-then-scroll, subtle chapter-colour word highlight that glows then fades. The highlight must survive the programmatic scroll-into-view (no scroll-based clear; it fades on a timer then unwraps so the DOM text is unchanged and MiniSearch stays intact); scope it to the landed section and scroll the first match into view, so a hit that sits deep in a section still lands on screen. Centred modal mobile + desktop, white field on mobile, no keyboard hints.
- Mobile menu: full-screen navy overlay, logo top (enlarged), large-type centred list, generous spacing, active green, search icon at foot, burger retained across all views, scroll locked.

## 6. Design principles, the throughlines
1. Information architecture before styling.
2. Detail on demand.
3. Touch has no hover; scope hover with @media (hover: hover).
4. Subtlety wins (1.02 lift not outline; soft fading highlight not marker-yellow).
5. Searchability is a build-time requirement.
6. One reusable component, themed by a variable.
7. Verbatim content, flagged quirks.
8. Whitespace and colour do the structural work.

## 7. Per-chapter build workflow
1. Map source pages. 2. Extract verbatim. 3. Render pages to confirm bold/colour/labels/tables/images. 4. Write marked-up content (tags + sub-nav + hero). 5. Check for & / <; confirm filenames. 6. Deliver one inlined copy-box prompt (content between markers) with colour, lozenges, styling map, sub-nav, hero. 7. Director pastes to Cash, commit, open, review, iterate.
Per chapter the director supplies: colour hex, hero and any in-content images.

## 8. Lessons and gotchas
- Inline content in the prompt; a workspace file does not reach the repo.
- Build shared things as components keyed to var(--c).
- Keep indexable text in the DOM; design search and collapse together.
- Confirm styling by rendering the source, not reading extracted text.
- Distinguish sequence from category before grid vs ordered list.
- Decide the active-state cue whenever everything looks selected.
- Do a dedicated mobile pass; grids, accordions, sub-navs, menus differ at phone width.
- Full-width is a bleed exception, not a re-alignment: keep the reading measure centred (margin auto), let only structural blocks bleed. A capped-but-not-centred column drifts to the left edge.
- Full-width grids can be pulled back to the reading column and capped 2-up per chapter with an id-scoped rule, without touching the shared grid component elsewhere. Flag before rolling sitewide.
- Sticky table header: position:sticky on th needs border-collapse:separate (collapse breaks positioning and stickiness in Chrome) and no scroll container around the table (overflow-x:auto computes overflow-y to auto and traps the sticky). Offset top to clear the topnav plus the sub-nav.
- Responsive table to cards: reflow one table with display:block; keep the header in the DOM by moving the thead sr-only, never display:none; label the mobile fields with data-label ::before.
- A global fixed control tints to the active chapter via :root --active, not var(--c), which will not cascade to an element sitting outside the chapters.
- Search word-highlight fades on a timer, never on scroll: the programmatic scroll-into-view fires scroll events that would wipe it before it is seen.

## 9. Seed checklist for a new project
- Confirm the three roles and the copy-box loop.
- Lock the guardrails in section 2.
- Establish brand tokens.
- Set per-section colour scheme and the var(--c) approach.
- Agree the content markup tag vocabulary.
- Decide the reusable components before building page one.
- Make searchability a build-time requirement.
- Agree the per-section workflow.
