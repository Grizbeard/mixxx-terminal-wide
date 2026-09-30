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
| 0013 library device lists follow mounts | a USB drive plugged in or pulled mid-session only shows up in (or leaves) Computer > Removable Devices, Rekordbox and Serato when that node is collapsed and expanded again |
| 0014 numbered library favorites for controllers | SHIFT + the DJM-T1's effect buttons do nothing: DJM-T1_Custom binds them to `[Library],favorite_N_*`, which do not exist, so there is no jumping straight to a stored crate, playlist, folder or Rekordbox/Serato playlist |
| 0015 `[App],shutdown` asks, then quits | the options page's *shut down* button does nothing, so a panel with no keyboard has no way to quit Mixxx or, on the appliance, turn the box off |

0007, 0013, 0014 and 0015 cost features. The rest are cosmetic, and the skin is built to
degrade quietly without them.

There is no 0012 here. It was wet-only effect units, for post-fader effects
returns sent to the master past the hardware mixer's faders, and it is kept with
the skin and mapping that used it on the `post-fader-fx` branch of this
repository (and of djm-t1-linux). The number stays unused so that 0013 keeps the
name it is known by elsewhere.
