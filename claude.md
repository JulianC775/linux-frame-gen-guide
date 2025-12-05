# Game Optimization Workflow for Claude

This document guides how to help optimize games on Linux with FSR3 Frame Generation using OptiScaler and GE-Proton.

## User's Setup

- **OS**: Linux
- **GPU**: NVIDIA
- **Tools**: Steam, GE-Proton, OptiScaler
- **Goal**: Enable FSR3 Frame Generation for better performance

## When User Requests Game Optimization

Follow this workflow when the user asks to optimize a game (e.g., "help me optimize Satisfactory"):

### Step 1: Gather Information

Ask the user for:
1. Game name
2. Whether they already have GE-Proton installed
3. Whether they already have OptiScaler downloaded
4. Any specific issues they're experiencing (optional)

### Step 2: Locate Game Files

Help the user find:
- Game installation directory (usually `~/.steam/steam/steamapps/common/[GAME_NAME]/`)
- Where the DLSS/FSR DLLs are located (common paths):
  - `./Binaries/Win64/`
  - `./Engine/Binaries/ThirdParty/NVIDIA/`
  - Game root directory

Use commands like:
```bash
cd ~/.steam/steam/steamapps/common/[GAME_NAME]/
find . -name "nvngx*.dll" -o -name "dlss*.dll"
```

### Step 3: Backup and Install OptiScaler

Guide the user to:
1. Backup original DLL files
2. Copy OptiScaler files to the correct directory:
   - nvngx.dll
   - nvngx_dlss.dll
   - libxess.dll
   - amd_fidelityfx_vk.dll

### Step 4: Create Configuration File

Create `nvngx.ini` in the same directory as the DLLs with a sensible starting configuration:

```ini
[General]
FrameGenEnabled=true
Upscaler=fsr21
OutputScaling=balanced
LogLevel=info

[FSR3]
FrameGenMode=on
EnableHUDFix=true

[DLSS]
OverrideDLSS=true

[Tweaks]
DlssReactiveMaskBias=0.45
Sharpness=0.5
MipLodBias=-0.75
```

### Step 5: Set Steam Launch Options

Provide the launch options:
```
PROTON_ENABLE_NVAPI=1 WINEDLLOVERRIDES="nvngx.dll=n,b;nvngx_dlss.dll=n,b" %command%
```

Optional additions:
- Add `mangohud` for FPS monitoring
- Add `PROTON_LOG=1` for debugging

### Step 6: Configure Proton Version

Remind user to:
- Set GE-Proton as the compatibility tool in Steam
- Use latest GE-Proton version unless there's a known issue

### Step 7: In-Game Settings

Provide game-specific recommendations:
- Enable DLSS/FSR in game settings (this activates OptiScaler)
- Suggested graphics settings based on GPU tier
- V-Sync should typically be OFF
- Frame rate limit settings

### Step 8: Provide Final Summary

**ALWAYS** end the optimization session by displaying:

1. **Steam Launch Options** (in a code block for easy copying):
   ```
   PROTON_ENABLE_NVAPI=1 WINEDLLOVERRIDES="nvngx.dll=n,b;nvngx_dlss.dll=n,b" %command%
   ```
   (Include any additional flags like mangohud if requested)

2. Brief reminder of next steps:
   - Where to paste launch options (Steam > Game Properties > Launch Options)
   - Need to enable DLSS/FSR in-game
   - What to expect (FPS increase, first launch shader stutter)

### Step 9: Save Configuration

After successful setup, create a file in `game-configs/[game-name].md` with:
- Game name and date
- DLL location path
- Working nvngx.ini configuration
- Steam launch options used
- In-game settings that work well
- Any game-specific quirks or notes

Example template:
```markdown
# [Game Name]

**Last Updated**: [Date]
**Status**: Working / Issues

## File Locations
- DLL Path: `[path]`

## nvngx.ini
[paste config]

## Steam Launch Options
[paste options]

## In-Game Settings
- [setting]: [value]

## Notes
- [any quirks or specific issues]
```

## Common Troubleshooting Steps

If issues arise:

1. **Game won't launch**:
   - Check WINEDLLOVERRIDES syntax
   - Verify DLL files are in correct location
   - Try restoring backups

2. **Frame gen not working**:
   - Ensure DLSS/FSR is enabled IN-GAME
   - Check nvngx.ini has FrameGenEnabled=true
   - Look for nvngx_log.txt for errors

3. **Poor performance**:
   - Try OutputScaling=performance
   - Reduce in-game settings
   - Check VRAM usage

4. **Visual artifacts**:
   - Ghosting: Lower DlssReactiveMaskBias (0.3-0.4)
   - Blurry: Increase Sharpness (0.6-0.8)
   - UI flicker: Ensure EnableHUDFix=true

## Configuration Presets by GPU

**RTX 3060/4060 (Budget)**:
```ini
OutputScaling=performance
DlssReactiveMaskBias=0.35
Sharpness=0.4
```

**RTX 3070/4070 (Mid-range)**:
```ini
OutputScaling=balanced
DlssReactiveMaskBias=0.45
Sharpness=0.5
```

**RTX 3080/4080+ (High-end)**:
```ini
OutputScaling=quality
DlssReactiveMaskBias=0.5
Sharpness=0.6
```

## Key Resources

- GE-Proton: https://github.com/GloriousEggroll/proton-ge-custom/releases
- OptiScaler: https://github.com/cdozdil/OptiScaler
- ProtonDB: https://www.protondb.com/

## Important Notes

- Frame generation works best at 60+ native FPS
- First launch may stutter (shader compilation)
- Not all games work perfectly - some experimentation needed
- Always backup original DLL files
- Keep configurations in game-configs/ for future reference
