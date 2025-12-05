# REPO

**Last Updated**: 2025-12-04
**Status**: Optimized - No Frame Gen (Unity game)
**Engine**: Unity
**GE-Proton Version**: GE-Proton9-20

## Game Information

- **Engine**: Unity (UnityPlayer.dll detected)
- **Upscaling Support**: None (no native DLSS/FSR2)
- **Frame Generation**: Not compatible (OptiScaler requires DLSS/FSR2)
- **Optimization Type**: General performance optimizations

## Why No Frame Generation?

REPO is a Unity game that doesn't have native DLSS or FSR2 support. OptiScaler (and other frame generation tools) work by replacing/intercepting DLSS or FSR2 DLL files, which this game doesn't have. Therefore, we can only apply general Linux gaming optimizations.

## Available Optimizations

### 1. GameMode (CPU Performance)
- Automatically adjusts CPU governor to performance mode
- Prioritizes game process
- Already installed on your system

### 2. DXVK Async (Shader Compilation)
- Reduces shader compilation stuttering
- Compiles shaders asynchronously in background
- Improves first-time gameplay smoothness

### 3. NVIDIA GL Threaded Optimization
- Enables multi-threaded OpenGL optimization for NVIDIA GPUs
- Can improve performance in some games

### 4. MangoHud (Performance Monitoring)
- Real-time FPS counter
- GPU/CPU usage and temperatures
- Frame time graphs

## Steam Launch Options

**Recommended (Performance + Monitoring):**
```
DXVK_ASYNC=1 __GL_THREADED_OPTIMIZATION=1 gamemoderun mangohud %command%
```

**Performance Only (no overlay):**
```
DXVK_ASYNC=1 __GL_THREADED_OPTIMIZATION=1 gamemoderun %command%
```

**Basic (GameMode only):**
```
gamemoderun %command%
```

**With MangoHud:**
```
gamemoderun mangohud %command%
```

**For Debugging:**
```
PROTON_LOG=1 gamemoderun %command%
```

### What Each Option Does:

- `DXVK_ASYNC=1` - Asynchronous shader compilation (reduces stuttering)
- `__GL_THREADED_OPTIMIZATION=1` - NVIDIA multi-threaded OpenGL
- `gamemoderun` - Activates GameMode for CPU performance boost
- `mangohud` - Shows FPS/performance overlay (Shift+F12 to toggle)
- `PROTON_LOG=1` - Enables detailed logging for troubleshooting

## Proton Configuration

- Set REPO to use **GE-Proton9-20** (or latest version)
- Right-click REPO → Properties → Compatibility
- Check "Force the use of a specific Steam Play compatibility tool"
- Select GE-Proton from dropdown

## In-Game Settings

Optimize these settings for better performance:

1. **Graphics Quality**:
   - Start with "High" preset
   - Lower to "Medium" if experiencing FPS drops

2. **Resolution**:
   - Use native resolution for best clarity
   - Lower resolution for better performance

3. **V-Sync**:
   - Turn OFF for lower input latency
   - Turn ON if experiencing screen tearing

4. **Anti-Aliasing**:
   - Lower AA setting for better FPS
   - FXAA or TAA typically performs better than MSAA

5. **Shadows**:
   - Shadows have high performance impact in Unity
   - Lower shadow quality/distance for FPS boost

## Expected Performance

- **No FPS multiplier** (like with frame gen)
- **Reduced stuttering** from DXVK_ASYNC
- **5-15% FPS improvement** from GameMode + optimizations
- **Smoother gameplay** overall

## Performance Tips

1. **First Launch**:
   - Expect some stuttering as shaders compile
   - DXVK_ASYNC helps, but first playthrough will cache shaders
   - Performance improves after 10-15 minutes of gameplay

2. **Monitor Performance**:
   - Use MangoHud to watch FPS/frame times
   - Press Shift+F12 to toggle overlay
   - Check GPU usage - if not at ~95%, may be CPU bottlenecked

3. **System Optimizations**:
   - Close background applications
   - Ensure NVIDIA drivers are up to date (535+)
   - Monitor GPU temperatures

## Additional Optimization Options

### If You Want Even More Performance:

**Install Gamescope** (for FSR1 upscaling):
```bash
sudo apt install gamescope  # Ubuntu/Debian
```

Then use launch options:
```
gamescope -w 1920 -h 1080 -W 2560 -H 1440 -f -F fsr -- gamemoderun %command%
```
This renders at 1080p and upscales to 1440p using FSR1 (not as good as FSR3, but works with any game)

### CPU Governor (Manual):
If GameMode doesn't work properly:
```bash
# Check current governor
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

# Set to performance (temporary)
echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

## Troubleshooting

### Game Won't Launch
1. Check GE-Proton is selected
2. Remove launch options one by one to isolate issue
3. Check PROTON_LOG=1 output in `/tmp/proton_<user>/`

### Poor Performance
1. Verify GameMode is active: `gamemoded -s` (should show "active")
2. Check GPU usage in MangoHud - should be high
3. Lower in-game graphics settings
4. Disable DXVK_ASYNC temporarily to see if it's causing issues

### Stuttering
1. Let shaders compile (play for 15+ minutes)
2. Ensure DXVK_ASYNC=1 is in launch options
3. Check disk space - shader cache needs space

## Files Modified

None - this is launch-option only optimization

## ProtonDB

- Game Page: https://www.protondb.com/app/3241660
- Check for community reports and additional tips

## Notes

- Unity games generally don't support advanced upscaling technologies
- Frame generation requires DLSS/FSR2 support which Unity doesn't provide
- These optimizations apply to most Unity games
- Results may vary based on hardware
- GameMode is the most impactful optimization for CPU-bound games

## Alternative: Native Linux Version

Check if REPO has a native Linux version on Steam - native builds often perform better than Proton compatibility layer. Look in Steam → REPO → Properties → Compatibility to see if there's a native option.
