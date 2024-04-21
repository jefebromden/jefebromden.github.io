---
title: "Beets"
author: "Rosendo Ayala"
date: 2024-04-20T09:04:10-03:00
draft: true
---
# Beets Music Tagger
## Installation
```bash
# dnf install beets beets-plugins
pip install beets
```

## Quickstart
```bash
beet import ~/Music
```

## Configuration
Config file must have `.yaml` extension, `yml` extension won't be recognized
```bash
cat > ~/.config/beets/config.yaml << 'EOF'
import:
    #move: yes # Move files instead of copying them (overwrites copy option)
    copy: no   # Do not copy files to de Music directory
    write: no  # Do not write tags when adding to the database
    incremental: yes # Skip importing directories already added
    log: beets.log

paths:
    # Filename format
    default: $albumartist/$album/$track-$title
    singleton: Singles/$artist-$title
    comp: $genre/$album/$track-$title # Compilations
    albumtype:soundtrack: Soundtracks/$album/$track-$title

asciify_paths: yes

EOF
```

Other options:
- `replace`: RegEx/Replacement pairs to be applied to all filenames
- `set_fields`: Default field values for new music, as a dictionary

## Plugins
```bash
# Install
pip install beets[fetchart,lyrics,lastgenre]
cat >> ~/.config/beets/config.yaml << 'EOF'
plugins: fetchart lyrics lastgenre
pluginpath:
    - /usr/lib/python3.12/site-packages/beetsplug
```

Some useful plugins. Those marked with `X` are external, the others native

Autotagger
- `chroma`: Use Acoustid fingerprinting to identify audio files
- `fromfilename`: Guess metadata for untagged tracks from their filenames

Metadata
- `parentwork`: Fetch work titles and works they are part of
- `fetchart`: Fetch album cover art from various sources
- `embedart`: Embed album art images into files’ metadata
- `ftintitle`: Add “featured” artists to the title field
- `beets-yearfixer`: Attempts to fix missing `original_year` or `year`(X)
- `beets-importreplace`: Perform regex replacements on incoming metadata (X)
- `beets-xtractor`: Fetch attributes like `is_instrumental` or `mood_happy` (X)

Files
- `beets-check`: Automatically checksums your files (X)
- `beets-alternatives`: Different versions of your songs for each device (X)
- `drop2beets`: Import singles as soon as they are dropped in a folder (X)
- `duplicates`: List duplicate tracks or albums
- `beets-noimport`: Add directories to the incremental import skip list (X)

Miscellaneous
- `beets-ydl`: downloads audio from youtube-dl (X)
- `beets-autofix`: Automates repetitive tasks (X)

Playback
- `random`: Randomly choose albums and tracks from your library
- `cmus`: Integrates with `cmus` music player(X)

## Files and Directories
- Database: ~/.config/beets/library.db
- Music Directory: ~/Music

## References
- [Beets ReadTheDocs](https://beets.readthedocs.io)
- [Inline Plugin](https://beets.readthedocs.io/en/stable/plugins/inline.html#inline-plugin)
- [Template Functions](https://beets.readthedocs.io/en/stable/reference/pathformat.html#template-functions)
- [Replace option](https://beets.readthedocs.io/en/stable/reference/config.html#replace)