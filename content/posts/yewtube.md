---
title: "Yewtube"
author: "JefeBromden"
date: 2024-04-21T17:16:17-03:00
draft: false
---

# Yewtube - Terminal YouTube Player
## Installation
```bash
pip install git+https://github.com/mps-youtube/yewtube.git
```

## Configuration
- `set ddir <download direcory>`: Set where downloads are saved
- `set search_music <true|false>`: Search only music
- `set <item> default`: Set an item to its default value
- `set -t <item>`: Temporarily change a setting without saving it

## Cookbook
```bash
# Search
/charly garcia # Search videos
//charly garcia # Search playlists
//charly garcia --duration short # Filter by duration

# Play
1 # Plays the first result. `q` to exit the player

# Download
d 1 # Show available downloads for a result
da 1 # Download best available audio
dv 1 # Download best available video

# Help
h # Help list
h config # Show config help
```

## Files
- `~/.config/mps-youtube/config.json`

## References
[YewTube GitHub](https://github.com/mps-youtube/yewtube)