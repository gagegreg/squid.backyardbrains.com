# Squid Ephys Data Viewer

Browser-based viewer for the squid feeding-tentacle recordings. Scrub through data, toggle raw vs. filtered, select an epoch by click-and-drag, then compute firing rate or RMS on the selection.

## How to launch

Browsers refuse to load local data files via `file://` for security reasons, so you need to serve the folder over `http://localhost` instead. One line in Terminal:

```bash
cd "/path/to/Squid ephys/viewer"
python3 -m http.server 8000
```

Then open **http://localhost:8000** in any browser. Ctrl+C in Terminal to stop. macOS has Python 3 preinstalled.

## Sessions

| Squid | Prep | Date | Time | Channels | Duration | Events |
|---|---|---|---|---|---|---|
| 1 | Tentacle 1 | 5/19 | 10:21 | 1 | 7 min | — (background) |
| 1 | Tentacle 1 | 5/19 | 10:33 | 1 | 17 min | 2 shrimp, 2 seawater |
| 1 | Tentacle 1 | 5/19 | 11:03 | 1 | 4 min | — (electrical stim) |
| 3 | Tentacle 1 | 5/19 | 15:27 | 2 | 41 s | — (passive) |
| 3 | Arm 1 | 5/19 | 15:50 | 2 | 2 min | — (passive) |
| 3 | Tentacle 2 | 5/19 | 15:54 | 2 | 4 min | 1 shrimp, 1 seawater |
| 4 | Tentacle 1 | 5/20 | 11:02 | 2 | 3 min | 3 seawater |
| 4 | Tentacle 1 | 5/20 | 11:07 | 2 | 2 min | — (baseline) |
| 4 | Tentacle 1 | 5/20 | 11:08 | 2 | 3 min | — (baseline) |
| 4 | Tentacle 1 | 5/20 | 11:11 | 2 | 4 min | 1 shrimp, 1 seawater |
| 4 | Tentacle 1 | 5/20 | 11:16 | 2 | 9 min | 5 shrimp, 1 seawater, 2 sponge |
| 4 | Tentacle 1 | 5/20 | 11:27 | 2 | 8 min | — (baseline) |
| 4 | Arm 1 | 5/20 | 11:48 | 2 | 7 min | 2 shrimp, 1 seawater, 1 sponge |

**Marker IDs in the data files:** 1 = shrimp, 2 = seawater (5/20 files; 5/19 used `0` for seawater), 3 = sponge.

**Recording source:** 5/19 morning Squid 1 from Mac BackyardBrains recorder; 5/19 afternoon and all 5/20 from iOS SpikerBox.

## Signal processing applied

Each recording was decimated from 20–44 kHz to **2.5 kHz** for storage (still captures spike shape; reduces file size to ~5–10 MB per session). Raw and filtered signals are both stored; filtered = 60/120/180/240 Hz notch + 300–3000 Hz bandpass.

## Viewer controls

- **Session dropdown** — switch recording
- **Filtered / Raw** — toggle signal type
- **Zoom** — 1, 2, 5, or 10 second window
- **Channel chips** — click to hide/show individual channels
- **Time scrubber + Prev/Next**
- **Event buttons** — jump to any marked stimulus
- **Click + drag on canvas** — select an epoch (highlighted yellow)
- **Compute Firing Rate** — counts positive crossings above 5×noise SD on filtered signal, with 4 ms refractory, per channel
- **Compute RMS** — root-mean-square of the selected epoch (in raw or filtered units, whichever is currently displayed)

## Suggested student exercises

1. **Baseline vs. response.** Open `5/19 Squid 1 Tentacle 1 10:33`. Click the "shrimp @ 48.9s" jump button. Select 30 s **before** the event and compute firing rate. Then select 30 s **after** and compare.
2. **Stimulus selectivity.** Same session, repeat for "seawater @ 176.8s". Is the response smaller?
3. **Multi-stimulus comparison.** Open `5/20 Squid 4 Tentacle 1 11:16`. It has shrimp, seawater, AND sponge deliveries. Compute the post-stim firing rate for each stimulus type and rank them.
4. **Tentacle vs. arm.** Compare `5/20 Squid 4 Tentacle 1 11:11` against `5/20 Squid 4 Arm 1 11:48`. Are baseline rates different? Is the response to shrimp different?
5. **Noise vs. signal.** Toggle Raw vs. Filtered on the same epoch and compute RMS for each. Notice how the filter strips low-frequency drift.
6. **Find the artifact.** In `5/19 Squid 3 Tentacle 2 15:54`, scrub to ~175 s. Compute firing rate in a 5 s window centered there, then in a clean 5 s window elsewhere. The artifact inflates the count — discuss why.

## Limitations

The 2.5 kHz storage decimation costs about 25% spike-detection accuracy compared to the original sample rate (some narrow spikes get smoothed out). The viewer is for browsing and qualitative analysis; for precise rates, students should work with the raw WAVs in Python or MATLAB.
