# Terminal-wide

A Mixxx skin for an 8.8" **1920×480** ultrawide touchscreen: two decks, the
waveforms between them, no mixer and no meters, DVS control on the deck, and
four colour schemes generated from a palette.

It was built for a DJM-T1 DVS appliance, where the panel is the only screen and
there is no keyboard: every control is a finger-sized block, nothing readable is
under 14px, and the settings that would otherwise live in a menu are on an
options page in the skin itself.

![Terminal-wide, Btop scheme](Terminal-wide/skin_preview_Btop.png)

## Layout

| Path | What it is |
|---|---|
| [`Terminal-wide/`](Terminal-wide/) | the skin — copy or symlink this one directory into Mixxx's skins path |
| [`tools/`](tools/) | the generator: palettes in, `style_<scheme>.qss` and the `<Schemes>` block out |
| [`scripts/`](scripts/) | a Windows launcher that runs Mixxx at the panel's exact size |
| [`patches/`](patches/) | Mixxx core changes the skin wants, and what you lose without each |

[`Terminal-wide/README.md`](Terminal-wide/README.md) is the long one: what is on
each page, why the deck is laid out the way it is, and the findings behind the
awkward parts.

## Installing

```bash
ln -s "$PWD/Terminal-wide" ~/.mixxx/skins/Terminal-wide
```

On Windows the skins path is `%LOCALAPPDATA%\Mixxx\skins\`. Then pick
**Terminal-wide** in Preferences → Interface, and a scheme beside it: *Btop*,
*Amber*, *Green Phosphor* or *White Phosphor*.

The skin runs on stock Mixxx 2.5+. The *two columns* waveform arrangement needs
patch 0007; everything else in [`patches/`](patches/) is cosmetic.

## Colours

Nothing in `Terminal-wide/style_*.qss` is hand-written, and neither is the
`<Schemes>` block in `skin.xml`. Edit a palette and regenerate:

```bash
python tools/generate.py                 # all four schemes
python tools/generate.py --palette btop  # one of them
python tools/generate.py --check         # write nothing, fail if stale
```

The skin ships **no image assets** apart from the library sidebar icons — every
mark on it is text on a block, which is what lets a scheme be a JSON file.

## Previewing

`scripts/preview-terminal-wide.ps1` launches Mixxx with the skin installed into
a throwaway settings directory, sized so the *client* area is exactly 1920×480
with no menu bar — the same canvas the skin gets fullscreen on the panel.

```powershell
$env:MIXXX_EXE = "C:\path\to\mixxx.exe"
$env:MIXXX_RES = "C:\path\to\mixxx\res"
.\scripts\preview-terminal-wide.ps1
```

It re-installs the skin on every run and says so if a file did not take, which
on this machine it silently did not for three days.

## Licence

CC-BY-SA 3.0 Unported, inherited from the skins this one's design comes from.
See [LICENSE](LICENSE).
