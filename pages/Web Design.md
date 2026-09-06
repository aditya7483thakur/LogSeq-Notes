- ## Web Design
	- Order every time: **category → brief → refs → pages → wireframe → hierarchy → type/color/space → hi-fi**.
	- One site, one job. If two things shout, neither works.
	- ### 5 major categories
		- **Ecommerce**
			- Goal: sell products
			- Design: products + price first, easy add-to-cart, trust (reviews, returns)
		- **Marketing / Business**
			- Goal: convert (lead, call, signup)
			- Design: hero + value + one clear CTA, social proof
		- **Content**
			- Goal: read / consume
			- Design: readable type, clear nav, featured + latest content
		- **Educational**
			- Goal: learn / progress
			- Design: courses first, path/progress visible, simple hierarchy
		- **Portfolio**
			- Goal: show work / get hired
			- Design: work first, short case studies, easy contact
	- ### Write a project brief
		- Before designing, write what the site is about. Design from this, not from vibes.
		- **Overview** — who they are, stage, why a site exists
		- **Description** — what the site must do (e.g. digital business card + credibility)
		- **Audience** — who it is for (investors, partners, buyers…)
		- **Scope** — how big (one-pager vs multi-page) + the one CTA
		- **Feel** — 2–3 words (professional, medical, tech…)
	- ### Steal the industry look
		- Search 3–5 sites in the **same domain** that look good and perform well
		- Note: layout, colors, type, how they build trust, where the CTA sits
		- Your design should feel like that industry — not far off from what people already expect
	- ### Pages first, then sections
		- List pages from the brief. Home + only what the audience needs. Cut the rest.
		- Common pages: Home, About, Work/Products, Pricing, Blog, Contact.
		- One-pager = same sections, no extra pages. Multi-page = each page has one job.
	- ### Page sections
		- Pick only sections that serve the brief. Order them: hook → prove → act.
		- **Navbar** — logo + few links + one CTA. Keep it thin.
		- **Hero** — first screen: headline, 1-line value, one CTA, supporting image. Job: say who + why in 5 seconds.
		- **Logos / proof bar** — "trusted by" row right under hero if credibility is the blocker.
		- **Features / Services** — 3–6 cards. Benefit first, not feature dump.
		- **How it works** — 3 steps. Use when the offer is new or confusing.
		- **About** — short story + credibility (team, numbers, logos). Skip if it doesn't help the goal.
		- **Testimonials** — name + role + photo + result. Proof after you make a claim.
		- **Pricing** — 2–3 plans, highlight one, CTA on each.
		- **CTA band** — repeat the one action before they leave (contact, book, buy).
		- **FAQ** — kill objections (price, risk, how it works).
		- **Footer** — links, legal, contact. Don't hide the CTA here only.
	- ### Wireframe = which button opens what
		- Grey boxes only. No colors, no fonts. Just boxes + labels + arrows.
		- List every page first (Home, About, Contact…).
		- On each page, label every clickable: **this button → that page / that section**.
		- Example
		  ```
		  Home
		    Logo        → Home
		    About       → About page
		    Work        → Work page
		    Contact CTA → Contact page
		    Hero CTA    → Contact page
		  ```
		- If two buttons do the same job, keep one. If a button has no page, don't draw it.
		- Do mobile wireframe too: hamburger → same links, CTA still visible.
	- ### Visual hierarchy
		- Rank 3 things per page: **1 headline  2 CTA  3 proof**. Rest stays quiet.
		- Bigger / bolder / higher contrast / more space around = more important.
		- Scan path: top-left logo → headline → CTA → then the rest.
		- One primary button per view. Secondary = outline or text link.
	- ### Layout / grid
		- Use a **12-column** grid. Desktop content width ~1100–1280px, centered.
		- Hero: text left + image right, or stacked centered. Don't invent a new layout.
		- Cards in 3s or 4s. Odd last row looks broken — keep counts clean.
		- Align to the grid. If it looks "off", it's usually alignment, not color.
	- ### Spacing
		- Use an **8px** scale: 8, 16, 24, 32, 48, 64, 96.
		- More space between sections than inside a section. Group related things.
		- Don't fill the page. Empty space makes the CTA feel expensive.
	- ### How to pick a font
		- Read the brief **feel** + the 3–5 ref sites. Match that world, don't pick a "cool" font.
		- **Sans-serif** (Inter, Geist, Plus Jakarta, Satoshi) — tech, SaaS, modern, clean
		- **Serif** (Playfair, Freight, Source Serif) — luxury, editorial, law, heritage
		- **One family, 2–3 weights** is safest. Or heading serif + body sans.
		- Never: 3+ fonts, script/display for body, comic / decorative on a business site.
		- Sizes that work
			- Body 16–18px, line-height 1.5
			- H1 ~40–64px, H2 ~28–36px, H3 ~20–24px
			- Button 16px medium/semibold
		- Short headlines. One idea. Subtext is one sentence.
		- Left-align long text. Center only short hero / CTA lines.
	- ### How to pick color
		- Principle: **visual hierarchy**. Don't pick attractive colors. Assign each color a **job**.
		- Color is not decoration. **Color controls attention.**
		- Example jobs
		  ```
		  Black       → background / space
		  White       → information
		  Gray        → lower-priority information
		  Neon green  → attention
		  ```
		- Don't make all primary text neon. Neon isn't "wrong" — you're spending your strongest signal everywhere, so nothing is first.
		- Ask: **what should the eye notice first, second, third?** Then work backwards.
		  ```
		  Notice first?     → strongest contrast / size / color
		  Notice second?    → slightly weaker
		  Supporting info?  → reduce contrast / size
		  ```
		- Don't start from a rainbow. Order: **neutrals → brand → CTA**.
		- **60 / 30 / 10**
			- 60% bg / surfaces (white, off-white, near-black)
			- 30% text + cards / secondary
			- 10% brand + CTA (the only loud color)
		- Feel → hue (then still check refs)
			- Trust / finance / SaaS / medical → blue
			- Health / eco / money → green
			- Luxury / fashion / editorial → black + gold or cream
			- Energy / food / consumer → orange / warm red
			- Creative / youth → one bold accent on neutrals
		- CTA = strongest color on the page. Use it on buttons, almost nowhere else.
		- Dark text on light bg (or reverse). Grey-on-grey is a fail.
		- If the company already has a logo color, that's the brand. Don't invent a second one.
	- ### Imagery
		- Real photos > stock smiles. Same crop, same lighting, or it looks messy.
		- Hero image supports the headline. Don't decorate with random art.
		- Icons: one style, same size, same stroke. 3–6 max in a row.
	- ### UI pieces
		- **Button** — one primary style. Label is a verb: Get started, Book a call, Buy.
		- **Card** — image/icon + title + 1 line + optional link. Equal height in a row.
		- **Form** — few fields. Label above input. One submit. Error under the field.
		- **Nav** — 4–6 links max. Current page slightly marked.
	- ### Design principles
		- These are how you judge a layout. If it "looks off", it's usually one of these.
		- **Hierarchy** — eye hits headline → CTA → proof. Size, weight, contrast, space do this.
		- **Contrast** — important thing is obviously different (dark/light, big/small, bold/regular).
		- **Alignment** — everything shares an edge. No "almost" lined up.
		- **Proximity** — related items sit together, unrelated get a gap.
		- **Repetition** — same button, same card, same type, same spacing on every page.
		- **White space** — room around the thing you want them to notice. Crammed = cheap.
		- **Balance** — left/right weight feels even (text vs image). Don't pile everything on one side.
		- **Consistency** — one nav, one CTA style, one card style. New page ≠ new look.
		- **Emphasis** — only one thing is the star per section. Secondary stays quieter.
	- ### Mobile
		- Design 375px and 1440px. Stack columns. Full-width CTA.
		- Hide extra nav behind a menu. Keep the main CTA on screen.
		- Tap targets ~44px. Don't put two small links on top of each other.
	- ### Hi-fi (make it look real)
		- Wireframe is locked first. Then apply type, color, real copy, real images.
		- Use the brief feel words as a filter: if it doesn't feel "medical / premium / bold", cut it.
		- Check: 5-second test — can a stranger say what this site is and what to click?
	- ### Brief for AI (when you generate the design)
		- Paste: category + brief + 3 ref sites + page list + button→page map + feel words.
		- Also say: 12-col grid, 8px spacing, 2 fonts, 1 accent CTA, mobile + desktop.
		- Ask for wireframe first, then hi-fi. Don't ask AI to invent extra pages.
