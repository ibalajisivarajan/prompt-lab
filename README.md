# Prompt Lab

A single-file, zero-backend, interactive sandbox that teaches four core LLM
engineering concepts by having you feel the difference each one makes,
using real calls to the [Groq API](https://groq.com).

**Live app:** [ibalajisivarajan.github.io](https://ibalajisivarajan.github.io/)

## What it teaches

You pick any topic — a product idea, business idea, or problem, in 4–25
words — and the same topic is carried through four progressively more
capable stages:

1. **Prompt** — the same idea, phrased vague vs. structured vs. with an
   example. One input, one raw output, no retry. You feel how far wording
   alone can take you, and where it stops working.
2. **Context** — the same prompt, but with a scoring rubric prepended
   before the model sees the task. Toggle it on/off and re-run to feel
   the difference between the model guessing your criteria and knowing
   them.
3. **Harness** — the app now demands a JSON response, validates it with
   `JSON.parse`, and auto-retries once on failure, logging every attempt.
   You stop reading prose and start reading a run log.
4. **Loop** — score → critique → revise → re-score, repeating until the
   idea clears a goal score or a max-iteration brake trips. The unit of
   work becomes the *run*, not the reply.

The example rubric used in Stages 2–4 is intentionally generic — edit the
`RUBRIC` constant in `index.html` for your own use case.

## Getting a Groq API key

1. Sign up at [console.groq.com](https://console.groq.com).
2. Create a key at [console.groq.com/keys](https://console.groq.com/keys) —
   Groq's free tier is enough to run this app.

## Deploying your own copy

1. Fork this repo.
2. In your fork, go to **Settings → Secrets and variables → Actions** and
   add a repository secret named `GROQ_API_KEY` with your Groq key.
3. Go to **Settings → Pages** and confirm **Build and deployment → Source**
   is set to **GitHub Actions**.
4. Push to `main` (or run the `Deploy Prompt Lab to Pages` workflow
   manually from the **Actions** tab). The workflow in
   `.github/workflows/deploy.yml` injects your secret into a generated
   `config.js`, then deploys the static site.
5. Your app will be live at `https://<your-username>.github.io/<repo-name>/`
   (or `https://<your-username>.github.io/` if the repo is named
   `<your-username>.github.io`).

**Never commit a real key to `config.js`.** It's git-ignored on purpose —
the workflow generates it fresh on every deploy from the `GROQ_API_KEY`
secret. `config.example.js` is tracked in the repo only to show the file's
shape; it ships with an empty key and is never read by the deployed app.

## Local development

There's no build step — it's one HTML file. To test locally with a real
key, copy `config.example.js` to `config.js`, paste in your key, and serve
the folder with any static file server, e.g.:

```
python3 -m http.server 8000
```

Then open `http://localhost:8000/index.html`.
