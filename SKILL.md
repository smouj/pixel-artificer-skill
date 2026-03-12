name: Pixel Artificer
description: Sculpts retro-pixel masterpieces from conceptual sketches
tags: design, pixels, art, retro, creativity
version: 1.2.3
author: OpenClaw Team
license: MIT
dependencies:
  - imagemagick: Required for image processing and conversion
  - python-pillow: For advanced pixel manipulation
  - pixel-art-ai: Custom library for AI-assisted pixel generation
environment_variables:
  - PIXEL_ART_STYLE: Default style (e.g., "nes", "gba", "arcade")
  - PIXEL_ART_SIZE: Default canvas size (e.g., "16x16", "32x32")
  - PIXEL_ART_COLOR_PALETTE: Default color palette file path
requires_confirmation: false
rollback_available: true
---

# Pixel Artificer Skill

## Purpose

Pixel Artificer is a specialized skill designed to transform conceptual ideas, sketches, or text descriptions into high-quality retro pixel art masterpieces. It excels at creating pixel-perfect sprites, tilesets, and animations for games, UI elements, and digital art. The skill bridges creative concepts with technical precision, enabling rapid prototyping of pixel art assets from simple prompts or uploaded sketches.

### Real Use Cases

- **Game Development**: Generate character sprites (e.g., a pixel knight with sword) for indie games in NES or GBA style.
- **UI Design**: Create retro-themed icons and buttons for apps or websites, like a floppy disk icon in 8-bit style.
- **Animation Prototyping**: Produce frame-by-frame animations, such as a walking cycle for a pixel character.
- **Asset Creation**: Build tilesets for platformers, including grass, bricks, and clouds in specific color palettes.
- **Concept Visualization**: Turn rough sketches into polished pixel art for concept art reviews.

## Scope

Pixel Artificer handles pixel art creation tasks exclusively. It does not perform general image editing, 3D modeling, or non-pixel art tasks.

### Specific Commands

- `/pixel generate <prompt> [--style <style>] [--size <width>x<height>] [--palette <file>] [--animate <frames>] [--output <file>]`: Generate pixel art from text prompt.
- `/pixel sketch <image_path> [--style <style>] [--refine] [--output <file>]`: Convert uploaded sketch to pixel art.
- `/pixel tileset <theme> [--count <n>] [--size <width>x<height>] [--output <directory>]`: Create a tileset of related pixel assets.
- `/pixel palette <image_path> [--extract] [--apply-to <existing_art>] [--output <file>]`: Extract or apply color palettes.
- `/pixel animate <frames> [--fps <rate>] [--loop] [--output <gif>]`: Combine generated frames into animation.
- `/pixel verify <file> [--check-size] [--check-palette] [--preview]`: Validate pixel art quality and specifications.

### Flags and Options

- `--style`: Supported styles include "nes" (NES palette, 16x16 max), "gba" (GBA colors, up to 32x32), "arcade" (bright colors, variable size), "custom" (user-defined palette).
- `--size`: Dimensions in pixels (e.g., "16x16", "64x32"). Must be multiples of 8 for retro authenticity.
- `--palette`: Path to a JSON file defining colors (e.g., `{"background": "#000000", "primary": "#FF0000"}`).
- `--animate`: Number of frames for animation (max 16 for performance).
- `--fps`: Animation speed in frames per second (default 12).
- `--refine`: Enable AI refinement on sketches for better edge detection.
- `--extract`: Extract dominant colors from an image into a palette file.
- `--check-size`: Verify dimensions match specifications.
- `--check-palette`: Ensure colors conform to retro constraints (max 16 colors).
- `--preview`: Display ASCII preview in terminal.

## Work Process

Pixel Artificer follows a structured workflow to ensure consistent, high-quality output. The process adapts based on input type (text prompt vs. sketch).

### Detailed Steps for Text-Based Generation

1. **Prompt Parsing**: Analyze the input prompt for key elements (subject, style, colors, actions). E.g., "red dragon breathing fire" extracts "dragon", "red", "fire breathing".
2. **Style Application**: Apply the specified style's constraints (color palette, pixel density).
3. **AI Generation**: Use integrated AI models to sketch initial pixel layout.
4. **Refinement Loop**: Iteratively refine pixels for sharpness and retro authenticity (up to 3 passes).
5. **Color Quantization**: Reduce to style-appropriate color count using dithering algorithms.
6. **Output Export**: Save as PNG with metadata (creation date, style, palette used).

### Steps for Sketch Conversion

1. **Image Loading**: Load and preprocess the sketch image (resize, grayscale if needed).
2. **Edge Detection**: Apply Canny edge detection to identify outlines.
3. **Pixelation**: Convert to pixel grid using adaptive scaling.
4. **Color Mapping**: Map detected colors to nearest palette entries.
5. **Manual Refinement**: If `--refine` is used, allow AI to suggest pixel adjustments.
6. **Final Render**: Export polished pixel art with alpha channels for transparency.

### Verification Steps

- Run `/pixel verify <output_file> --check-size --check-palette --preview` to confirm dimensions, color compliance, and visual quality.
- Compare output against original prompt/sketch for fidelity.
- Test animations by viewing the generated GIF in a browser.

### Dependencies and Requirements

- **Software**: ImageMagick (for image processing), Python 3.8+ with Pillow library.
- **Hardware**: At least 2GB RAM; GPU acceleration recommended for animations.
- **Files**: Access to palette JSON files in `/assets/palettes/` directory.
- **Permissions**: Write access to output directories; read access for input sketches.

## Golden Rules

1. **Retro Purity**: Always enforce pixel-perfect grids; no anti-aliasing unless explicitly requested for modern blends.
2. **Color Constraints**: Limit palettes to authentic retro counts (NES: 52 colors max, but typically 16 per sprite).
3. **Prompt Specificity**: Encourage detailed prompts (e.g., "8-bit isometric tree, green leaves, brown trunk" over "tree").
4. **Ethical Generation**: Reject prompts involving copyrighted characters or inappropriate content; respond with "Pixel Artificer specializes in original retro art only."
5. **Performance Limits**: Cap animations at 16 frames; warn if generations exceed 30 seconds.
6. **File Safety**: Never overwrite existing files without confirmation; auto-append timestamps to outputs.
7. **Style Consistency**: Default to "nes" if no style specified; maintain consistency within tilesets.

## Examples

### Example 1: Basic Sprite Generation

**User Input:**
```
/pixel generate "pixel knight with sword, blue armor, heroic pose" --style nes --size 16x16 --output knight.png
```

**Process Output:**
- Parsing: Subject "knight", attributes "blue armor", "sword", "heroic pose".
- Generation: Creates 16x16 sprite with NES palette.
- Verification: Colors within 52-NES limit, no pixel bleed.

**Final Output:** `knight.png` - A crisp pixel knight sprite ready for game integration.

### Example 2: Sketch Conversion

**User Input:**
```
/pixel sketch uploads/sketch_dragon.jpg --style gba --refine --output dragon_pixel.png
```

**Process Output:**
- Loading: Resizes sketch to fit GBA constraints (32x32 max).
- Refinement: AI detects wings and scales, sharpens edges.
- Export: Saves with transparent background.

**Troubleshooting Note:** If sketch is too low-resolution, suggest uploading a higher-quality version.

### Example 3: Tileset Creation

**User Input:**
```
/pixel tileset "forest" --count 8 --size 16x16 --palette palettes/nature.json --output tiles/forest/
```

**Output:** Generates 8 forest tiles (grass, trees, rocks) in matching style, saved as tile01.png to tile08.png.

### Example 4: Animation

**User Input:**
```
/pixel generate "running cat" --style arcade --animate 4 --fps 15 --output cat_run.gif
```

**Output:** 4-frame running animation loop.

## Rollback Commands

- `/pixel rollback <file>`: Deletes the specified generated file and removes from history.
- `/pixel undo --last`: Reverts the most recent generation, restoring any overwritten file.
- `/pixel purge --all`: Clears all generated assets in output directory (requires confirmation).
- `/pixel revert <version>`: If versioning is enabled, restore a previous version of a pixel art file.

### Rollback Verification

After rollback, run `/pixel verify --list` to confirm file removal and check for orphaned temp files.

## Troubleshooting

- **Generation Fails**: Check dependencies (`convert --version` for ImageMagick); ensure palette file exists.
- **Colors Look Wrong**: Verify palette JSON syntax; use `--check-palette` on input.
- **Animations Lag**: Reduce frame count or FPS; check RAM usage.
- **Sketches Not Converting**: Ensure image is in JPG/PNG; try `--refine` for complex sketches.
- **Output Too Large**: Resize input or specify smaller `--size`; compress with ImageMagick post-generation.