# Infinite Tales

A dynamic **Choose Your Own Adventure** engine that generates a unique, branching story with AI. Every adventure is different — your choices shape the story and its endings.

The entire app is a single self-contained HTML file: `index.html` (no build step, no dependencies except the Lucide icon CDN).

## Run it

Just open `index.html` in a browser:

```sh
open index.html
```

Or serve it locally:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

## AI Providers

Pick a provider on the **Connect Your AI** screen. Keys and selected models are stored separately per provider in `localStorage`, so you can switch between providers without losing your setup.

| Provider | Description | Setup |
| --- | --- | --- |
| **OpenRouter** | Free tier with many open models | Get a free API key at [openrouter.ai/openrouter/free](https://openrouter.ai/openrouter/free). Only free models are listed. |
| **Ollama** | Runs locally (or cloud models via your Ollama account) | No key needed. Requires the [Ollama app](https://ollama.com/download) running at `localhost:11434`. |
| **ChatGPT** | OpenAI's models | API key from [platform.openai.com/api-keys](https://platform.openai.com/api-keys). The available chat models are fetched live with your key. |
| **MiniMax** | OpenAI-compatible endpoint with the M-series models | API key from [platform.minimax.io](https://platform.minimax.io/user-center/basic-information/interface-key). |

Keys never leave your browser and are stored only in your browser's `localStorage`.

### Scene Illustrations

Enable **Story Illustrations** in the settings. MiniMax is used automatically whenever a MiniMax API key is configured (as the active provider or saved under the MiniMax key); otherwise it falls back to Puter:
- **MiniMax (API key)** — uses the MiniMax image API with your MiniMax key. A fixed per-story seed keeps the visual style consistent across every scene.
- **Puter (free)** — unlimited, no API key needed. Powered by Puter.js (Google Imagen); players cover their own usage with a free Puter account.

You can force a specific provider with the **Illustrations Provider** toggle. If you pick MiniMax but no key is configured, it falls back to Puter automatically.

## Features

- **Dynamic stories** — the AI writes every scene in second person, reacting to your past choices.
- **18 built-in topics** — fantasy, sci-fi, mystery, horror, cyberpunk, vampires, and more — or describe your own custom tale.
- **Branching endings** — stories escalate and conclude in good, bad, or neutral endings after ~10–20 turns.
- **Undo** — go back and take a different path at any time.
- **Settings** that shape every adventure:
  - **Tone**: Balanced, Light, Dark, Horror, Humorous
  - **Difficulty / Stakes**: Low, Medium, High
  - **Language**: English, Español, Français, Português, Deutsch, Italiano
  - **Language Level**: Kid, Learning, Basic, Intermediate, Advanced
  - **Writing Style**: Immersive, Concise, Literary, Cinematic
- **Fully translated UI** in all 6 languages.

## Technical notes

- Story generation uses the OpenAI-compatible chat completions API with a strict JSON response format (`story`, `choices`, `is_ending`, `ending_type`, `summary`).
- A running story summary is kept for continuity across turns, and the language/level are re-enforced on every turn so the model doesn't drift back to English.
- Models are fetched live from each provider's API (OpenAI and OpenRouter), with built-in timeouts and fallbacks to curated model lists.
- Lucide icons are loaded from `unpkg.com/lucide@latest`.
