# Prompt Lab

A single-file, zero-backend, interactive sandbox that teaches four core LLM
engineering concepts by having you feel the difference each one makes,
using real calls to the [Groq API](https://groq.com).

**Nothing in this app is hardcoded to any one topic or domain.** Type
anything — a business idea, a love letter, a grocery list, a status report,
an app spec, a grant proposal, whatever — and every stage below generates
its content live, from your topic, via Groq. There is no fixed example
idea, no fixed rubric, and no schema field names tied to one kind of task.

**Live app:** [ibalajisivarajan.github.io/prompt-lab](https://ibalajisivarajan.github.io/prompt-lab/)
(this repo is named `prompt-lab`, so GitHub Pages serves it at that
subpath rather than the bare `ibalajisivarajan.github.io` root — that's
expected, not a bug.)

## What it teaches

You start by typing a topic (up to 10 words / 60 characters), then that
exact topic is carried through four progressively more capable stages:

1. **Prompt** — Groq writes you three starter prompts for your topic
   (vague, structured, with an embedded example). Pick one, edit it if you
   like, and send it. One input, one raw output, no retry. You feel how
   far wording alone can take you, and where it stops working.
2. **Context** — on first visiting this stage, Groq generates a short,
   genuinely useful piece of background (a template, style reference,
   checklist, or constraints) specific to your topic. Toggle it on/off and
   re-run the same task prompt to feel the difference between the model
   guessing and the model knowing.
3. **Harness** — the app now demands a JSON response (`qualityScore`,
   `keyStrength`, `improvementSuggestion`, and the actual generated
   `output` — the real letter/list/report/whatever your topic asked for),
   validates it with `JSON.parse`, and auto-retries once on failure,
   logging every attempt. You stop reading prose and start reading a run
   log.
4. **Loop** — starting from a rough first-pass draft (also generated from
   your topic, but fully editable), the app runs score → critique →
   revise → re-score, repeating until quality clears a goal score (85/100)
   or a max-iteration brake trips (4 iterations). The unit of work becomes
   the *run*, not the reply.

Changing your topic (via the "Change topic" control) throws away
everything generated so far and starts clean — nothing carries over
between topics.

## Getting a Groq API key

1. Sign up at [console.groq.com](https://console.groq.com).
2. Create a key at [console.groq.com/keys](https://console.groq.com/keys) —
   Groq's free tier is enough to run this app.

The app looks for a key in this order: a deploy-injected `config.js`
(see below), then a key you've previously pasted in on this device, then
it asks you once via a plain browser prompt and remembers it on that
device for next time.

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
   (or `https://<your-username>.github.io/` only if your repo is named
   exactly `<your-username>.github.io` — GitHub Pages routes by repo name).

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

Then open `http://localhost:8000/index.html`. If you skip the config.js
step, the app will just ask you for a key via a browser prompt the first
time it needs one.
