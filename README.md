# DLSS 5 Neural Rendering on AMD RX 6000 cards (experimental)

**Deutsch:** [README_DE.md](README_DE.md)

![In-game example](images/ingame2.jpg)

## What is this?

NVIDIA made a new graphics feature called **DLSS Neural Rendering** ("DLSS 5"). It makes games look
more realistic: better light, more detail, nicer colours. Normally it **only works on NVIDIA cards**.

This mod makes it run on **AMD Radeon RX 6000** graphics cards (RDNA2).

> **Experimental.** It can crash, look wrong, or cost a lot of FPS. Do not use it in online games with
> anti-cheat.

**New in v0.4:**
- **NVIDIA's new DLSS 5 model** (nvngx_dlssnr.dll 310.9.16) is used automatically: about **2x lighter**, ~10 ms per network run at 960x540 on an RX 6900 XT (v0.3: 24 ms). The old 310.8 DLL still works with the old model.
- **Cyberpunk 2077, 1440p, FSR Quality, no frame generation, RX 6900 XT:** 54.7 fps on High, 85.3 fps on Low (v0.3: 43.8 fps on High; danielblnc v0.6.0 on the same card: 21.7 fps).
- **Image much closer to NVIDIA's** (+6-8 dB vs the reference; 4 network bugs fixed), NVIDIA's default look, and the **OptiScaler NR sliders now work** (intensity, style, tone, structure, skin).
- **Much less flicker** (`edit_temporal`, flicker -90 %), network split over 3 frames for even, higher fps.
- **Looks to choose from** (`presets\`): Standard, Strong, Cinematic, HighQuality_720p, HiRes_1080p, Native_1440p, Performance, Community.
- **Network resolutions up to 1080p / 1440p** for more fine detail (costs fps).
- All kernels are our own; no NVIDIA data in the package. Full list: [CHANGELOG.md](CHANGELOG.md).

**New in v0.3.1:** uses the right GPU (never the iGPU), VRAM leak fix, HDR auto-exposure, clean NR-off with an unknown
`nvngx_dlssnr.dll`, all kernels our own, a bit faster and less VRAM.

**New in v0.3:** up to 60 FPS without frame generation on an RX 6900 XT, even frame pacing, no more stutter
in fast games, HDR fix (no more white characters), clean 2 passes, safe setup with backup and uninstaller.
See [CHANGELOG.md](CHANGELOG.md).

## Does my PC work?

You need **all** of these:

| | |
|---|---|
| ✅ Graphics card | AMD **RX 6000 series**, see the table below. RX 7000/9000, NVIDIA and Intel do **not** work. |
| ✅ Windows | Windows 10 or 11, 64-bit |
| ✅ AMD driver | A recent AMD Adrenalin driver ([download](https://www.amd.com/en/support/download/drivers.html)) |
| ✅ Game | A 64-bit DirectX 11 or DirectX 12 game where you can choose **DLSS, FSR or XeSS** in the graphics settings |
| ✅ NVIDIA model file | `nvngx_dlssnr.dll`. **Not included**, you have to get it yourself (see step 2) |

| Graphics card | Status (v0.4) | Network time per run, new model, 960x540 |
|---|---|---|
| RX 6900 XT / 6950 XT | ✅ tested | ~10 ms (measured) |
| RX 6800 / 6800 XT | ✅ works (user reports) | ~11-14 ms (estimate) |
| RX 6700 / 6700 XT / 6750 XT | ✅ works (user reports) | ~18-20 ms (estimate) |
| RX 6600 / 6600 XT / 6650 XT | ✅ works (user reports) | ~25-30 ms (estimate) |
| RX 6500 XT / 6400 | 🧪 should work, slow | 50 ms+ (estimate) |

**v0.4 runs on the whole RX 6000 (RDNA2) family.** First user reports average **about 55 FPS** in game. More reports
are welcome in [Issues](../../issues) (attach your `dlssnr_nr.log`, it lists your GPU and timings).

Not sure which graphics card you have? Press `Ctrl + Shift + Esc`, click **Performance**, click **GPU**.
The name is in the top right corner.

## Installation

### Step 0: Back up your game folder

Copy your whole game folder somewhere safe (for example to your desktop). If something goes wrong, copy it back.

### Step 1: Download

Go to [**Releases**](../../releases/latest) and download the ZIP (`DLSS5-RDNA2-Experimental-...zip`). Every RX 6000 card uses the same (latest) ZIP.

### Step 2: Get the NVIDIA model file

This mod does **not** contain any NVIDIA files. You need the file **`nvngx_dlssnr.dll`** (about 160 MB).
How to get it is explained in janblade's guide:
[Choose the correct runtime](https://github.com/janblade/OptiScaler-DLSSNR-PreSR-Multipass/blob/main/INSTALL-DLSSNR.md#choose-the-correct-runtime).

### Step 3: Find your game folder

This is the folder with the game's **real 64-bit** `.exe` file, the one that actually starts the game.
It is **not** always the main install folder: many games have a small launcher `.exe` at the top and the
real game `.exe` further down.

- **Steam:** right-click the game → **Manage** → **Browse local files**
- **Epic / GOG / others:** right-click the game → open install folder

Look for these subfolders first. If one of them has the game `.exe` inside, use **that** folder:

| Folder pattern | Example |
|---|---|
| main folder (no launcher) | `...\Fallout 4\` (has `Fallout4.exe`) |
| `bin\` or `bin\x64\` | `...\Cyberpunk 2077\bin\x64\` (has `Cyberpunk2077.exe`) |
| `x64\` | `...\GameName\x64\` |
| `<Name>\Binaries\Win64\` (Unreal games) | `...\Jedi Survivor\SwGame\Binaries\Win64\` |

Tip: start the game, open Task Manager → **Details**, right-click the game process → **Open file location**.
That opens exactly the right folder. `setup_windows.bat` warns you if the folder has no 64-bit `.exe`.

### Step 4: Copy the files

1. Open the ZIP from step 1.
2. Copy **everything** in it into the game folder. Click **Replace** if Windows asks.
3. Copy `nvngx_dlssnr.dll` from step 2 into the same game folder.

### Step 5: Run the setup

In the game folder, double-click **`setup_windows.bat`**. A black window opens and asks some questions:

> ⚠️ **Do you use ReShade or ENB?** Then **never** overwrite. If the script asks "overwrite?", answer `2` and pick `2` (`winmm.dll`). Overwriting deletes the ReShade/ENB file and the game may stop starting.

| Question | Type |
|---|---|
| "Do you want to delete these files?" (only if an old OptiScaler is there) | `1` + Enter |
| "Choose a filename for OptiScaler" | just Enter (= `dxgi.dll`) |
| "dxgi.dll already exists ... overwrite?" (you use ReShade or another mod) | `2` + Enter, then pick `2` (`winmm.dll`) |
| "Are you using an Nvidia GPU or AMD/Intel GPU?" | `1` + Enter (AMD) |

When the window says it is done, close it.

### Step 6: Start the game

1. In the game's graphics settings, turn on **DLSS**, **FSR** or **XeSS** (whatever the game offers).
2. In the game, press the **`Insert`** key. The OptiScaler menu opens.
3. Open **DLSS Neural Rendering** and make sure **Enable Neural Rendering** is ticked.
4. Press `Insert` again to close the menu.

![OptiScaler menu](images/menu.jpg)

The first start takes a few seconds longer. That is normal.

**DirectX 11 games** (for example Fallout 4): in the menu at the top, under **Upscalers**, pick an
upscaler that says **w/Dx12** (for example `FSR 2.2.1 w/Dx12`) and restart the game.

## Choosing a look

The folder `presets` has complete settings files. To switch, copy one over `nr_port.ini` in the game folder and restart the game.

| Preset | Look | FPS on an RX 6900 XT (est.) | When to use it |
|---|---|---|---|
| **Standard** (default) | NVIDIA's default look, 960x540 model | ~43 (baseline) | Start here |
| **Strong** | Clearly stronger light and faces, still clean | ~43 (no extra cost) | Standard looks too subtle |
| **Cinematic** | Strong + NVIDIA's Cinematic style: strongest, graded look | ~43 (no extra cost) | Screenshots, slow games |
| **HighQuality_720p** | 1280x720 model: faces and skin much closer to the full-resolution result | ~40 | You have fps to spare |
| **HiRes_1080p** | 1920x1080 model, finer detail | ~33 | Image quality first |
| **Native_1440p** | Network at full 1440p | ~26 | Comparison, screenshots |
| **Performance** | 640x360 model, softer | ~45-50 | Weaker GPUs, fast games |
| **Community** | `compose=nv`, `pass2_gain=1`: for NVIDIA multipass setups copied from NVIDIA users | per pass about as Standard; 2 passes cost more | You use OptiScaler's 2 passes |

FPS: Fallout 4 at 1440p, network split over 3 frames; only Standard is measured in a game, the others are estimated from the
network time (RX 6900 XT, per network run: 7.3 / 10.6 / 16.5 / 32.6 / 56.5 ms at 360p / 540p / 720p / 1080p / 1440p).
Every preset has `edit_temporal=0.7` (new in v0.4.3): it removes most of the remaining flicker at no cost.

**Tip:** for the most realistic look, use Style **Cinematic** together with a higher model resolution: copy `Cinematic.ini`
and set `model_res=1280x720` (or `1920x1080` if you have the fps).

The OptiScaler NR sliders (intensity, style, local tone, local structure, skin) now change the image live. A key in `nr_port.ini` (e.g. `local_tone=0.5`) overrides the slider.

## Which nvngx_dlssnr.dll?

Works with nvngx_dlssnr.dll 310.8 (old model) and 310.9.16 (new, ~2x lighter model, auto-selected; model=large forces the old one). With any other model, NR switches itself off — wait for a port update.

The game keeps running with its normal image. `dlssnr_nr.log` then says "has a different model than this port supports (310.8 / 310.9.16)".

## Best image: NR + FSR 4 on RX 6000

For the best image, combine NR with FSR 4 as the upscaler. FSR 4 does not officially run on RX 6000, but
[fsr4-shader-performance-overrides](https://github.com/zgauthier2000/fsr4-shader-performance-overrides) makes it work
there. Get it from that project and follow its instructions; it is not part of this ZIP.

## Known issues (v0.4)

- **Higher network resolutions (1080p / 1440p) can show artefacts, ghosting and odd lighting** in some games, and the visible gain over 540p is smaller than expected. We are investigating; 960x540 (Standard) is the recommended setting.
- Strong settings (intensity 2, tone 2, Cinematic) make the effect much stronger but also increase flicker.
- Ghosting can appear on fast motion with `nr_split=3/4`; set `nr_split=0` and `edit_temporal=0` in `nr_port.ini` to compare.
- A few rare crash/edge cases found in an internal code review (feature released while in use, format mismatches, a >500 ms stall during split frames) will be fixed in the next update.

## It does not work. What now?

Open the file **`dlssnr_nr.log`** in the game folder with Notepad and look at the last lines:

| What the log says | What to do |
|---|---|
| `runtime ready` | It works. Check that Neural Rendering is enabled in the `Insert` menu. |
| `nvngx_dlssnr.dll missing or wrong version` | `nvngx_dlssnr.dll` is missing in the game folder, or it is a version with different weights. Get it again (step 2). |
| `not an RDNA2 GPU` | Your graphics card is not supported (see "Does my PC work?"). |
| `hipModuleLoad ...` | Your card could not load the program. Please report it in [Issues](../../issues) with the log. |
| Game says a DirectX / d3d11 / dxgi file is missing | In Steam: right-click the game → Properties → Installed Files → **Verify integrity of game files**. If you use ReShade or ENB, reinstall it. |
| No `dlssnr_nr.log` file at all | The mod did not load. Run `setup_windows.bat` again and check that you selected an upscaler in the game. |

## Uninstall

In the game folder, double-click **`Uninstall_DLSS5.bat`** and press `1`. It removes OptiScaler and our files
(`nr_data`, `nr_runtime.dll`, `nr_port.ini`, `nvngx.dll_dlssnr.dll`, logs) and puts back every file that
`setup_windows.bat` replaced or deleted (those are saved in `DLSS5_backup\`). Your `nvngx_dlssnr.dll` is kept.

## Is this faster or slower?

It costs FPS, but with **v0.4** and NVIDIA's new model it runs well. The AMD card has to do work that NVIDIA cards have special hardware for.
With the v0.4 defaults (960x540 network, NVIDIA's new model, network split over three frames), measured on an RX 6900 XT in the
**Cyberpunk 2077 benchmark at 1440p, FSR Quality, no frame generation: 54.7 FPS on High, 85.3 FPS on Low** (v0.3: 43.8 FPS on High).
First user reports across RX 6000 cards average **about 55 FPS**. Higher network resolutions (presets HiRes / Native) cost more FPS.

## Who made what?

| Part | By |
|---|---|
| The AI model (DLSS Neural Rendering) | **NVIDIA**. It lives in `nvngx_dlssnr.dll`, which you get yourself. |
| Getting the model to run on AMD RX 6000 | **This project** (`nr_runtime.dll`, `nvngx.dll_dlssnr.dll`, `nr_data`) |
| Getting it into games (menu, settings) | [**janblade's OptiScaler-DLSSNR**](https://github.com/janblade/OptiScaler-DLSSNR-PreSR-Multipass) and the [OptiScaler](https://github.com/optiscaler/OptiScaler) team, see `docs/CREDITS.md` in the release |

Started in danielblnc's Discord; early versions were based on his gfx1100 kernels — all kernels are now our own.

This project contains no NVIDIA files or weights. DLSS and NVIDIA are trademarks of NVIDIA
Corporation. This project is not affiliated with or endorsed by NVIDIA or AMD.

## For developers

The source code of `nr_runtime.dll` and `nvngx.dll_dlssnr.dll` is in [`src/`](src). Build with
`src\build.bat` (needs AMD ROCm 6.4 for Windows and Visual Studio 2022 Build Tools).

## License

[GPL-3.0](LICENSE)
