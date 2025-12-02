Snow Monkey mapping guide (for TOKYO LABEL FUWALY)
=================================================

Goal
- Use this static prototype as a visual/structural reference.
- Rebuild the same layout using Gutenberg blocks with Snow Monkey.

Global
- Header: Snow Monkey "Header" + Global Navigation (Appearance > Menus)
- Footer: Footer widget areas + Navigation menus

Sections → Block mapping
- Hero: Section block (full width) → Columns (2col) → Heading, Paragraph, Buttons, Image
- Features: Section (background color) → Columns (3col) → Each column uses Media & Text or Image + Heading + Paragraph
- About: Section → Media & Text (image left, text right) → List + Button
- Lineup: Section → Columns (3col) or Snow Monkey "Items" block → Image + Heading + Text
- News: Section (background color) → Posts list (Snow Monkey Blocks: Recent Posts) or Query Loop
- CTA: Section (accent background) → Heading + Paragraph + Button (centered)

Design tokens (connect to Customizer)
- Colors: Map to theme colors (Primary, Background, Text, Muted). Replace CSS variables with brand values.
- Typography: Set base font size/line-height; choose Japanese font family from guide.
- Radius/Shadow: Adjust via Additional CSS or utility classes.

Implementation tips
- Use Snow Monkey "Section" block spacing presets to match vertical rhythm.
- For card look: use Group block with border + radius + shadow (Additional CSS) or Snow Monkey card-like blocks.
- Keep headings hierarchy: H1 (Hero), H2 (section titles), H3 (card/product titles).
- Ensure contrast ratio (≥ 4.5:1) for body text and buttons.

Next steps
- Replace images with brand art direction assets.
- Set exact color palette, spacing scale, and font per design guide.
- Build Gutenberg patterns for: Hero, Feature 3-up, Media & Text, Products grid, Posts list, CTA.

