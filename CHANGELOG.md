# Changelog (DLSS 5 Neural Rendering, AMD RDNA2 port)

## v0.3

- **Up to 60 FPS without frame generation** on an RX 6900 XT (Fallout 4, 1440p, 960x540 model): about 40-45 FPS in heavy scenes, up to 60 in lighter ones.
- **Even frame pacing.** `nr_split=1` splits the network over two frames, so the frame time stays even and no longer alternates long/short.
- **Faster.** The c32 kernels now run natively.
- **Less flicker.** The temporal history follows the game's motion vectors.
- **Fast games no longer stutter.** v0.2.3 dropped to a native frame whenever the GPU was still busy (Assetto Corsa). Now it waits for the GPU instead (`busy_mode=wait`, `busy_wait_ms=100`). In the pacing test this gives 0 native frames.
- **HDR fix.** Characters and highlights no longer turn white in HDR games (RE Engine). `hdr_mode=auto` detects HDR input and converts it for the network.
- **Clean 2 passes.** With OptiScaler's "2 passes" the effect is no longer applied twice. The second pass is damped (`pass2_gain=0.5`).
- **Safe setup and uninstall.** `setup_windows.bat` backs up every file it replaces, and `Uninstall_DLSS5.bat` restores them. The setup text is now AMD-specific.
- **Wrong-folder warning.** Setup warns when the folder has no 64-bit game exe, for example a 32-bit launcher folder.
- **Hardware log and self-check.** `dlssnr_nr.log` records the GPU, driver and VRAM, warns when VRAM is low, and runs a self-check at start. If the self-check fails, NR switches off cleanly instead of crashing.
- **ShortFuse's nvngx_dlssnr.dll works.** The weights are found by name, so any build that carries the same weights works.
- New `nr_port.ini` defaults: `nr_split=1`, `busy_mode=wait`, `hdr_mode=auto`, `pass2_gain=0.5`.
- `nr_data` ships 960x540 (recommended) and 1280x720.
