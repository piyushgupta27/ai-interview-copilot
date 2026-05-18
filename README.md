# ai-interview-copilot

A browser-based utility that runs alongside you during job interviews — tracks time, fires milestone alerts, records your audio, and transcribes live. No install. No account. No audio leaves your browser.

**[→ Open the live demo](https://piyushgupta27.github.io/ai-interview-copilot/)**

![Interview Copilot screenshot](docs/screenshot.png)

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
- **Round timer** with 5 milestones tuned to a 45-min interview, phase labels (Opening / Discussion / Late / Closing / Overtime), hard stop at 60 min
- **Live milestone alerts** — 30s warning, then "Now", then "Overdue" if you miss the window
- **Audio recording** — local mic capture via MediaRecorder, saves `.webm` on stop
- **Live transcript** — Web Speech API with aggressive auto-restart watchdog (exponential backoff)
- **Status badges** — Mic / Transcript / Backup always visible so you know what's working
- **Crash-safe backup** — audio chunks saved to IndexedDB every 30s; recovery dialog on next open if browser crashed
- **Pause / resume** — timer pauses, recording keeps going
- **Keyboard shortcuts** — `S` to start/pause, `R` to reset

---

## Browser support

| Browser | Timer | Recording | Live transcript |
|---|---|---|---|
| Chrome / Edge | ✅ | ✅ | ✅ |
| Firefox | ✅ | ✅ | ❌ (no Web Speech API) |
| Safari | ✅ | ✅ | ⚠️ limited |

**Recommended: Chrome.** Live transcript requires the Web Speech API.

---

## Known issues

- **Interviewer's voice not captured** — MediaRecorder captures your mic only. The other side of the call (on Meet, Zoom, etc.) is on your speaker output, not your mic. Tab audio capture is on the roadmap (v0.3).
- **Live transcript can drop** — Web Speech API sessions can silently end when Chrome re-prompts for mic permission (e.g. on screen-share toggle). The watchdog restarts it, but there may be gaps. Run audio through [whisper.cpp](https://github.com/ggerganov/whisper.cpp) post-call for a reliable transcript.
- **Milestones hardcoded to 45 min** — configurable round presets are roadmap (v0.4).

---

## Post-call transcription (recommended)

The built-in Web Speech transcript is best-effort. For a full transcript after the call, run the downloaded `.webm` through whisper.cpp:

```bash
# Install whisper.cpp (macOS)
brew install whisper-cpp

# Transcribe
whisper-cli --model small.en --language en --output-txt interview-audio.webm
```

---

## Roadmap

| Phase | Theme | Status |
|---|---|---|
| **v0.1** | Foundation — single file, crash recovery, live badges | ✅ shipped |
| **v0.2** | Reliability — mic meter, permission monitoring, bookmarks | 🔜 next |
| **v0.3** | Coverage — tab audio capture, interviewer voice | planned |
| **v0.4** | Post-call — in-browser Whisper, one-click debrief, round presets | planned |

---

## Contributing

Issues and PRs welcome. If you've used this in a real interview, I'd love to hear what broke or what you wished it did differently — open an issue.

---

## License

MIT
