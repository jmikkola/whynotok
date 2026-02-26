# whynotok

A static mental-wellness tool that guides users through a series of yes/no questions to help them identify why they might be feeling down.

## Architecture

**Single file: `index.html`**
Everything lives in one file — HTML, CSS, JavaScript, and the question data. No build step, no dependencies beyond Google Fonts (Inter). Designed to be uploaded directly to S3 or any static host.

No state is persisted between page loads. Each visit starts fresh.

## How it works

1. On load, questions are shuffled (Fisher-Yates) and the welcome screen is shown.
2. Questions are asked one at a time with **Yes** / **Not really** buttons.
3. **Yes** → advance to the next question.
4. **Not really** → show an insight + actionable suggestion for that topic, then offer **Keep exploring** (continue with remaining questions) or **Start over**.
5. If the user answers Yes to all questions → "You're covering all the basics" ending screen.
6. If the user has said No to at least one and exhausts all questions → "You've explored everything" ending screen.

The `answeredNo` boolean flag distinguishes the two ending states.

## Configuration

All content is stored in a `<script id="config" type="application/json">` block near the bottom of `index.html`, above the app script. Edit the JSON there to change:

- `welcomeTitle` / `welcomeSubtitle` — welcome screen copy
- `allGoodTitle` / `allGoodBody` — shown when user said Yes to everything (`\n` separates paragraphs)
- `finishedTitle` / `finishedBody` — shown when user said No to at least one question and finished all questions
- `questions` array — the question set (see schema below)

### Question schema

```json
{
  "id": "unique-string",
  "question": "The yes/no question to display",
  "hint": "Italic subtext below the question (use \"\" to omit)",
  "emoji": "Shown large on the insight screen",
  "insight": "Why this topic might be affecting mood",
  "suggestion": "A concrete, actionable next step"
}
```

## Deployment

Upload `site/index.html` to S3 (or any static host) and serve it. No CORS configuration needed — all assets are either inline or loaded from Google Fonts CDN.

To preview locally, open the file directly in a browser (no server needed — there are no fetch calls).
