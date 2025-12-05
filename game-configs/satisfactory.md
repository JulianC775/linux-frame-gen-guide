# Satisfactory

**Last Updated**: 2025-12-04
**Status**: Configured - Ready to test
**OptiScaler Version**: 0.7.9
**GE-Proton Version**: GE-Proton9-20

## File Locations

- DLL Path: `~/.steam/steam/steamapps/common/Satisfactory/FactoryGame/Plugins/DLSS/Binaries/ThirdParty/Win64/`
- Game uses native DLSS support

## nvngx.ini Configuration

```ini
[General]
# Enable FSR3 Frame Generation
FrameGenEnabled=true

# Use FSR 2.1 for upscaling
Upscaler=fsr21

# Quality preset - balanced is good for most systems
# Options: performance, balanced, quality, ultra_performance
OutputScaling=balanced

# Enable detailed logging (disable after testing for performance)
LogLevel=info

[FSR3]
# Frame generation settings
FrameGenMode=on

# HUD fix helps with UI elements
EnableHUDFix=true

# Auto exposure fix for better visuals
EnableAutoExposure=true

[DLSS]
# Override native DLSS with FSR3
OverrideDLSS=true

[Tweaks]
# Reactive mask - adjust if seeing ghosting
# Lower values (0.2-0.4) = less ghosting but may reduce quality
# Higher values (0.5-0.7) = better quality but may cause ghosting
DlssReactiveMaskBias=0.45

# Sharpness - adjust to preference
# 0.0 = no sharpening, 1.0 = maximum sharpening
Sharpness=0.5

# Texture detail enhancement
MipLodBias=-0.75
```

## Steam Launch Options

```
PROTON_ENABLE_NVAPI=1 WINEDLLOVERRIDES="nvngx.dll=n,b;nvngx_dlss.dll=n,b" %command%
```

Optional with MangoHud for FPS monitoring:
```
PROTON_ENABLE_NVAPI=1 WINEDLLOVERRIDES="nvngx.dll=n,b;nvngx_dlss.dll=n,b" mangohud %command%
```

## Proton Configuration

- Set Satisfactory to use **GE-Proton9-20** (or latest version)
- Right-click Satisfactory → Properties → Compatibility
- Check "Force the use of a specific Steam Play compatibility tool"
- Select GE-Proton from dropdown

## In-Game Settings

After launching the game:

1. **Enable DLSS**:
   - Go to Settings → Graphics
   - Find NVIDIA DLSS setting
   - Enable DLSS
   - Set quality to "Balanced" or "Quality"
   - This will activate OptiScaler's FSR3 Frame Generation

2. **Recommended Graphics Settings**:
   - Window Mode: Fullscreen
   - Resolution: Native resolution
   - V-Sync: Off (let frame gen handle pacing)
   - Frame Rate Limit: Unlimited or 165+
   - View Distance: High or Epic
   - Anti-Aliasing: TAA (DLSS handles this)
   - Post Processing: High
   - Shadows: Medium-High
   - Effects: High
   - Foliage: Medium-High

## Expected Performance

- First launch may stutter (shader compilation - normal)
- Expect 60-90% FPS increase with frame generation enabled
- Best results with 60+ native FPS

## Files Installed

- nvngx.dll (OptiScaler core - renamed from OptiScaler.dll)
- nvngx_dlss.dll.backup (original DLSS backup)
- amd_fidelityfx_vk.dll
- amd_fidelityfx_dx12.dll
- amd_fidelityfx_framegeneration_dx12.dll
- amd_fidelityfx_upscaler_dx12.dll
- libxess.dll
- libxess_dx11.dll
- nvngx.ini (configuration file)

## Troubleshooting

### If game won't launch:
1. Verify launch options are correct (no typos)
2. Check GE-Proton is selected in compatibility settings
3. Restore backup: `mv nvngx_dlss.dll.backup nvngx_dlss.dll`

### If frame gen not working:
1. DLSS **must** be enabled in-game
2. Check nvngx_log.txt in the DLL directory for errors
3. Try changing `Upscaler=xess` in nvngx.ini

### For better performance:
- Change `OutputScaling=performance` in nvngx.ini
- Lower in-game shadow/effects quality

### For better quality:
- Change `OutputScaling=quality` in nvngx.ini
- Increase `Sharpness=0.6` or higher

## Notes

- Satisfactory uses Unreal Engine 5 with native DLSS support
- Frame generation works by intercepting DLSS calls
- Check for log file after first launch to verify OptiScaler is active
- Can adjust settings in nvngx.ini without reinstalling
