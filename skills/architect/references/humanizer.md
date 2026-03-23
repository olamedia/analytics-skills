# Humanizer

Rewrite text to eliminate identifiable LLM patterns. Use when writing compiled issues (Phase 4), testing notes, goal descriptions, or any output meant for humans in issue trackers.

## Kill List — words and patterns to eliminate or replace

### Overused AI vocabulary

Never use these unless the context genuinely demands them:

additionally, align with, boasts, bolstered, crucial, delve, emphasizing, enduring,
enhance, fostering, garner, highlight (verb), interplay, intricate/intricacies,
key (adjective), landscape (abstract), meticulous/meticulously, pivotal, showcase,
tapestry (abstract), testament, underscore (verb), valuable, vibrant, nestled,
in the heart of, groundbreaking, renowned, diverse array, rich (as filler adjective),
profound, commitment to, natural beauty, serves as, stands as, marks a shift,
setting the stage, evolving landscape, focal point, indelible mark, deeply rooted

### Structural tells

- **Neat parallel headers**: `## The Problem` / `## The Solution` / `## The Result` — real people don't outline their rants
- **Rule of three**: "X, Y, and Z" lists appearing repeatedly — vary list lengths, use 2 or 4 items sometimes
- **Balanced parallelism**: "It does X when it should do Y" — break the symmetry
- **Not just X, but also Y**: drop the construction entirely
- **Despite challenges + optimistic ending**: the "despite its..., it continues to..." formula
- **Superficial -ing analyses**: "highlighting its importance", "underscoring the significance", "reflecting broader trends"
- **Elegant variation**: constantly swapping synonyms for the same thing to avoid repeating a word — just repeat the word
- **Copula avoidance**: replacing "is" with "serves as", "stands as", "marks", "represents" — just say "is"
- **Title Case In Every Header**
- **Bold key terms** in lists like a textbook
- **Inline-header lists**: `1. **Step Name**: description` format

### Tone tells

- Too clean, too balanced — every paragraph the same length, every argument neatly countered
- Promotional puffery — everything is "significant", "crucial", "vibrant"
- Vague attribution — "experts say", "observers note", "research suggests" without specifics
- Concluding with future outlook or "challenges and opportunities"
- Hedging then asserting — "While X is relatively minor, it plays a crucial role in..."

## How to rewrite

1. Read the text and identify which tells are present
2. Strip the structure — remove or merge overly neat section headers, let the text flow
3. Vary sentence length — mix short fragments with longer ones, don't keep everything uniform
4. Let it meander — real writing goes on tangents, circles back, repeats a point differently
5. Use casual language where appropriate — contractions, sentence fragments, "yeah", "look", "honestly"
6. Be specific — replace "significant impact" with what actually happened
7. Drop the balance — real opinions are lopsided, not "on one hand / on the other"
8. Leave rough edges — not every transition needs to be smooth, not every paragraph needs a topic sentence
9. Cut the filler — if a sentence only says "this is important" in fancy words, delete it
10. Read it out loud mentally — if it sounds like a press release or a Wikipedia article, it's not done

## What NOT to do

- Don't just swap AI words for synonyms — the structure matters more than individual words
- Don't add typos or broken grammar on purpose — that's fake, not human
- Don't over-correct into slang — match the register the user wants
- Don't lose the actual content while removing AI patterns — the ideas stay, the packaging changes

## Where to apply in the architect pipeline

- **Phase 4 (Compile)**: Goal descriptions, acceptance criteria, testing notes — everything in compiled issues
- **Phase 1 (Requirements)**: User stories — the "As a..., I want..., so that..." is a template but the acceptance criteria should read naturally
- **Any user-facing text**: If it's going in an issue tracker, a PR description, or documentation that humans read, run it through this filter
