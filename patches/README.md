# Mixxx core patches

Terminal-wide loads on stock Mixxx 2.5+, but several of the things it does
need core changes that are not upstream yet. They are here so this repository
stands on its own: without them the skin still runs, and the list below says
what you lose.

These are the `src/` parts of commits on a Mixxx working branch. Three of those
commits also changed the Terminal skin that lives in the Mixxx tree; that half
is not here, which is why two subjects mention a skin whose files the patch
does not touch.

## Applying

Against Mixxx `main` at
`5adc3efebf33918b1e46bb4b0f5e342d8c39655e` (*Merge pull request #17002 from
mixxxdj/sync-branch-2.6-to-main*):

```bash
cd /path/to/mixxx
git checkout -b terminal-wide 5adc3efebf33918b1e46bb4b0f5e342d8c39655e
git am /path/to/mixxx-terminal-wide/patches/*.patch
```

They are small and touch quiet corners, so they usually rebase onto a newer
`main` without trouble. Nothing here is required to *build*: drop any patch you
do not want and the rest still apply.

## What each one is for

| Patch | Without it |
|---|---|
| 0001 RGB overview honours the skin's waveform colours | the overview keeps Mixxx's own colours while the waveform follows the scheme |
| 0002 skins can override the library sidebar icons | the sidebar keeps Mixxx's built-in orange icons |
| 0003 skin-provided icons drawn without the selection tint | an overridden icon is tinted when its row is selected |
| 0004 tree-item icons go through the same override | playlists, crates and history keep the built-in icons |
| 0005 per-scheme icon directory (`library/<scheme>/`) | all four schemes share one icon set, so three of them get the wrong hue |
| 0006 `<UseCueColor>` and `<TrackTableBackgroundColorOpacity>` | the monochrome schemes get a stray coloured hotcue marker and a track-colour wash |
| 0007 vertical waveform rotation | **the *two columns* lane arrangement renders wrong** — the one functional loss; stay on *two rows* |
| 0008 clear the long-press pixmap | coloured fringing around a button held down |
| 0009 delete a chain preset from its unit | the fx page can save presets but not remove one |
| 0010 `WKey` honours the skin's alignment | the deck's key reads centred instead of from the left |
| 0011 gap between the key colour bar and the key text | the bar and the first glyph butt together |
| 0012 wet-only effect units (send/return) | **no post-fader effects**: every unit on the fx page reads *pre-fader*, and the DJM-T1_Custom mapping leaves units 1 and 2 as ordinary units and says so in the log |
| 0013 library device lists follow mounts | a USB drive plugged in or pulled mid-session only shows up in (or leaves) Computer > Removable Devices, Rekordbox and Serato when that node is collapsed and expanded again |

0007, 0012 and 0013 cost features. The rest are cosmetic, and the skin is built to
degrade quietly without them.

0012 is not cosmetic in the other direction either: it changes the audio engine.
It adds `[EffectRack1_EffectUnitN],wet_only`, which makes a unit output only what
its effects produce, with its mix knob as the send level into them, and silence
when it is off or empty. That is what lets a unit on a crossfader bus be an
effects return that Mixxx's main output sends past a hardware mixer's channel
faders. It carries its own unit test (`engineeffectchain_wetonly_test`) and was
checked against `appliance/integration` as well as this series.
