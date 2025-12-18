# PC Configurations

This directory contains configuration files for each PC that uses this repository.

## Files

- **TEMPLATE.json**: Template for creating new PC configs
- **laptop.json**: Configuration for the laptop PC
- **active-pc.json**: Symlink pointing to the currently active PC config

## How to Use

### Adding a New PC

1. Copy `TEMPLATE.json` to a new file (e.g., `desktop.json`)
2. Fill in your PC's specifications
3. Set `is_active: false` (only one PC should be active at a time)

### Switching Active PC

When you switch to a different PC:

```bash
# Remove old symlink
rm active-pc.json

# Create new symlink to the PC you're using
ln -s desktop.json active-pc.json
```

Alternatively, just update the `is_active` field in each config file.

### PC Config Structure

Each config includes:
- **Hardware specs**: GPU, CPU, RAM
- **System info**: Distro, kernel, Steam path
- **Tools installed**: GE-Proton, OptiScaler, mangohud, gamemode status
- **Optimization preferences**: Default settings based on GPU tier

## Notes

- The active PC config is used by Claude to provide PC-specific optimization recommendations
- Update `tools_installed` fields as you install new tools on each PC
- GPU tier (budget/mid-range/high-end) determines default optimization presets
