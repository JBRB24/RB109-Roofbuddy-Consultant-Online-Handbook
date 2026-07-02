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
- Core tokens: navy #161823, green #40BB6A, Poppins (body/headings), Bebas Neue (ghost numerals), hard corners (border-radius 0), no shadows.
- Per-chapter colour via one CSS var (--c) so components inherit automatically.
  Ch2 #009444, Ch3 #1a6e77, Ch4 #33586b, Ch5 #2aa8e0, Ch6 #3d3f63, Ch7 #6f446d, Ch8 #ed1456, Ch9 #822c3b, Ch10 #d15c31, Ch11 #d5966d, Ch12 #d49a2c. (Ch1 is the lighter green, exact hex not recorded.)
- Hero: image at brightness ~0.6, solid chapter-colour layer (mix-blend-mode: color, opacity 1), no navy bottom gradient, Bebas ghost numeral (opacity raised), Poppins title ending in a full-stop, depth deepened ~20%.
- Numbered section lozenges. Caption style: Poppins 400 italic, 12px/22px, rgb(136,146,164).

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
- Section sub-nav: solid chapter-colour tabs, white text, hover darker shade scoped to @media (hover: hover), active holds darker shade. Mobile fills width, wraps edge-to-edge, no horizontal scroll, no scrollbar.
- Lightbox: navy ~92% backdrop, image-only and detail modes, close X/Esc/backdrop.
- Chapter index overlay: 4x3 grid, tiles enlarged ~60% via wider container, bold titles ending in full-stop, active nav underline tracks current view, no X, backdrop click closes, Esc closes, tile/nav clicks shielded.
- Site-wide search: index all text by walking the DOM at load; results list with context + location label; click switches view, opens accordion/lightbox if needed, reveal-then-scroll, subtle chapter-colour highlight that fades. Centred modal mobile + desktop, white field on mobile, no keyboard hints.
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

## 9. Seed checklist for a new project
- Confirm the three roles and the copy-box loop.
- Lock the guardrails in section 2.
- Establish brand tokens.
- Set per-section colour scheme and the var(--c) approach.
- Agree the content markup tag vocabulary.
- Decide the reusable components before building page one.
- Make searchability a build-time requirement.
- Agree the per-section workflow.
