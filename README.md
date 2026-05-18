# ai-interview-copilot

A browser-based utility that runs alongside you during job interviews — tracks time, fires milestone alerts, records your audio, and transcribes live. No install. No account. No audio leaves your browser.

**[→ Open the live demo](https://piyushgupta27.github.io/ai-interview-copilot/)**

---

## Why this exists

Every existing tool is either heavyweight SaaS (Otter, Yoodli, Poised — cloud upload, account required) or a generic timer with no interview structure. I needed something I could open in a tab two minutes before a call, that would pace me through milestones, record locally, and not get in the way.

Built and dogfooded across real interviews.

---

## Quickstart

1. Open `src/index.html` in Chrome or Edge — or use the [live demo](https://piyushgupta27.github.io/ai-interview-copilot/)
2. Press **Start Call** when the interviewer joins — mic + timer start together
3. Watch the checkpoints, respond to alerts
4. Press **Reset** when done — audio (`.webm`) and transcript (`.txt`) download automatically

No build step. No dependencies. Single file.

---

## Features

- **Single "Start Call" button** — mic recording and round timer start together, no two-CTA fumble under pressure
- **Round presets** — HRBP 45, HM 60, SD 60, Peer 30; each with tuned milestones; select before the call, locks on start
- **Live milestone alerts** — 30s warning, then "Now", then "Overdue" if you miss the window
- **Tab audio capture** — Chrome: captures your Meet/Zoom/Teams tab audio + mic so you get both sides; Firefox/Safari fall back to mic-only gracefully
- **Audio recording** — local mic (+ tab) capture via MediaRecorder, saves `.webm` on stop
- **Live transcript** — Web Speech API with aggressive auto-restart watchdog (exponential backoff)
- **Post-call panel** — transcript shown inline after session ends; "Polish with Claude" copies a debrief prompt to clipboard and opens claude.ai — no API key needed
- **Status badges** — Mic / Transcript / Backup / Interviewer always visible so you know what's working
- **Crash-safe backup** — audio chunks saved to IndexedDB every 30s; recovery dialog on next open if browser crashed
- **Pause / resume** — timer pauses, recording keeps going
- **Keyboard shortcuts** — `S` to start/pause, `R` to reset, `B` to bookmark, `E` to export log

---

## Browser support

| Browser | Timer | Recording | Tab audio | Live transcript |
|---|---|---|---|---|
| Chrome / Edge | ✅ | ✅ | ✅ | ✅ |
| Firefox | ✅ | ✅ | ✅ | ❌ (no Web Speech API) |
| Safari | ✅ | ✅ | ❌ | ⚠️ limited |

**Recommended: Chrome.** Tab audio capture and live transcript both require Chrome/Edge.

---

## Known issues

- **Live transcript can drop** — Web Speech API sessions can silently end when Chrome re-prompts for mic permission (e.g. on screen-share toggle). The watchdog restarts it, but there may be gaps. Use "Polish with Claude" post-call with the downloaded `.webm` for a clean debrief.
- **Tab audio requires Chrome** — `getDisplayMedia` audio capture is not supported on Safari. Firefox supports it but the audio track may not always be available depending on OS audio routing.

---

## Post-call debrief

The built-in Web Speech transcript is best-effort. After the session, the **Post-call** panel shows your transcript inline and offers two options:

1. **Copy transcript** — paste into any tool
2. **Polish with Claude** — copies a structured debrief prompt to clipboard and opens [claude.ai](https://claude.ai/new). Paste the prompt. Claude will identify strong moments, flag missed opportunities, and suggest follow-up questions. No API key, no account needed beyond Claude.ai.

For a full transcript from the audio, run the downloaded `.webm` through whisper.cpp:

```bash
# macOS
brew install whisper-cpp
whisper-cli --model small.en --language en --output-txt interview-audio.webm
```

---

## Roadmap

| Phase | Theme | Status |
|---|---|---|
| **v0.1** | Foundation — single file, crash recovery, live badges | ✅ shipped |
| **v0.2** | Reliability — mic meter, permission monitoring, bookmarks | ✅ shipped |
| **v0.3** | Coverage — tab audio capture, Interviewer badge | ✅ shipped |
| **v0.4** | Post-call — debrief panel, Polish with Claude, round presets | ✅ shipped |

---

## Contributing

Issues and PRs welcome. If you've used this in a real interview, I'd love to hear what broke or what you wished it did differently — open an issue.

---

## License

MIT
