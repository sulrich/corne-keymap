# CLAUDE.md - AI Assistant Guide for Corne Keymap Repository

## Repository Overview

This repository contains a custom QMK firmware keymap for the Corne (CRKBD) keyboard, a 42-key split ergonomic keyboard. The keymap is optimized for macOS usage and maintained by @sulrich.

**Repository Type:** QMK Firmware Keymap
**Target Hardware:** Corne/CRKBD (split 3x6 + 3 thumb keys per side)
**Primary OS Target:** macOS
**License:** GPL v2+

## Codebase Structure

```
corne-keymap/
├── keymap.c       # Main keymap definition and OLED display logic
├── config.h       # Hardware and feature configuration
├── rules.mk       # Build rules and feature flags
└── README.md      # User-facing documentation with layer layouts
```

### File Descriptions

#### keymap.c (7,216 bytes)
- **Purpose:** Core keymap implementation
- **Key Sections:**
  - Lines 21-58: Three-layer keymap definition (`keymaps[][]`)
    - Layer 0: Base QWERTY layer
    - Layer 1: Symbols layer with arrow keys
    - Layer 2: Numbers and function keys layer
  - Lines 60-155: OLED display driver code (conditionally compiled)
    - Layer state rendering
    - Keylog display for debugging
    - Corne logo rendering

#### config.h (1,106 bytes)
- **Purpose:** Compile-time hardware and feature configuration
- **Key Settings:**
  - Line 24: `MASTER_LEFT` - defines left half as master for split keyboard
  - Lines 26-34: RGB lighting configuration (if enabled)
  - Line 36: OLED font definition pointing to crkbd default font

#### rules.mk (48 bytes)
- **Purpose:** Feature flags for QMK build system
- **Enabled Features:**
  - `EXTRAKEY_ENABLE = yes` - Media keys and system control
  - `OLED_DRIVER_ENABLE = yes` - OLED display support

#### README.md (4,609 bytes)
- **Purpose:** User documentation
- **Contains:** Visual ASCII representations of all three layers with detailed key mappings

## Development Workflows

### Typical Modification Tasks

1. **Changing Key Mappings**
   - Edit the `keymaps[][]` array in `keymap.c`
   - Use QMK keycodes (format: `KC_*` for basic keys, see QMK documentation)
   - Maintain visual layout comments for readability

2. **Adding/Removing Layers**
   - Modify the `keymaps[][]` array structure
   - Update layer access keys (currently `MO(1)` and `MO(2)` in thumb clusters)
   - Update OLED layer state rendering if enabled

3. **Modifying OLED Display**
   - Edit functions within the `#ifdef OLED_DRIVER_ENABLE` block
   - Key functions: `oled_render_layer_state()`, `oled_render_keylog()`, `oled_render_logo()`

4. **Enabling/Disabling Features**
   - Modify `rules.mk` for build-time features
   - Modify `config.h` for configuration values

### Build and Flash Workflow

**Note:** This repository contains only the keymap files, not a complete QMK build environment.

**Standard QMK Build Process:**
```bash
# This keymap should be placed in:
# qmk_firmware/keyboards/crkbd/keymaps/sulrich/

# Build command (from QMK root):
qmk compile -kb crkbd -km sulrich

# Flash command:
qmk flash -kb crkbd -km sulrich
```

**Integration with QMK:**
1. Clone QMK firmware: `git clone https://github.com/qmk/qmk_firmware.git`
2. Place these files in `qmk_firmware/keyboards/crkbd/keymaps/sulrich/`
3. Run build commands from QMK firmware root directory

### Git Workflow

**Current Branch:** `claude/claude-md-mi6372jto4zdpk1z-01VwLDLa8HYaE1SojoHawtKS`
**Repository Status:** Clean (as of session start)

**Commit Guidelines:**
- Keep commits focused on single logical changes
- Update README.md if layer layouts change
- Use descriptive commit messages explaining the "why" of changes

## Key Conventions and Patterns

### Keymap Structure

**Layer Philosophy:**
- Layer 0: Base typing layer (QWERTY)
- Layer 1: Symbols and navigation (vim-style arrows on hjkl)
- Layer 2: Numbers, function keys, and media controls

**Layer Access:**
- `MO(1)` and `MO(2)` in right thumb cluster provide momentary layer access
- Modifiers (Ctrl, Shift, Opt, Cmd) are transparent through layers (`KC_TRNS`)

### Thumb Cluster Layout

**Left Thumb (consistent across layers except layer 1):**
- Position 1: `KC_LOPT` (Alt/Option)
- Position 2: `KC_LGUI` (Cmd)
- Position 3: `KC_ENT` (Enter) - becomes Backspace on layer 1

**Right Thumb:**
- Position 1: `KC_SPC` (Space)
- Position 2: `MO(1)` (Symbols layer access)
- Position 3: `MO(2)` (Numbers/media layer access)

### QMK Keycode Naming Conventions

```c
// Standard keys
KC_A, KC_B, ... KC_Z    // Letters
KC_1 ... KC_0           // Numbers
KC_TAB, KC_ESC, KC_ENT  // Special keys

// Modifiers
KC_LCTL, KC_LSFT        // Left control, shift
KC_LGUI, KC_LOPT        // Left Cmd (GUI), Alt (Option)

// Symbols (shifted)
KC_EXLM (!), KC_AT (@), KC_HASH (#)
KC_DLR ($), KC_PERC (%), KC_CIRC (^)

// Brackets and parens
KC_LBRC ([), KC_RBRC (])
KC_LCBR ({), KC_RCBR (})
KC_LPRN ((), KC_RPRN ())

// Navigation
KC_LEFT, KC_DOWN, KC_UP, KC_RIGHT

// Media controls
KC_MPLY, KC_MPRV, KC_MNXT  // Play, previous, next
KC_VOLU, KC_VOLD, KC_MUTE   // Volume controls

// Layer switching
MO(n)      // Momentary layer switch while held
KC_TRNS    // Transparent - passes through to lower layer
```

### Code Style Conventions

1. **Visual Alignment:** Keymap arrays use extensive spacing for visual clarity
2. **Comments:** ASCII art comments show physical key layout above code
3. **Layer Definitions:** Each layer gets its own block with clear visual separator
4. **Conditional Compilation:** OLED code wrapped in `#ifdef OLED_DRIVER_ENABLE`

## Modification Guidelines for AI Assistants

### When Modifying Keymaps

1. **Always maintain visual comments** - The ASCII art layout comments are critical for readability
2. **Preserve layer structure** - Three layers are intentional; don't add layers without discussion
3. **Keep macOS focus** - Default modifier mappings assume macOS (Cmd vs Ctrl placement)
4. **Update README.md** - If layer layouts change, update the visual representations in README.md

### When Modifying OLED Code

1. **Respect conditional compilation** - All OLED code must be within `#ifdef OLED_DRIVER_ENABLE`
2. **Memory constraints** - AVR microcontrollers have limited RAM; keep strings in PROGMEM
3. **Layer state constants** - Lines 68-70 define layer state masks; keep in sync with keymaps

### Configuration Changes

1. **Master hand selection** - `MASTER_LEFT` (config.h:24) determines which half is USB-connected
2. **Feature flags** - Adding features in `rules.mk` increases firmware size
3. **RGB lighting** - Currently configured but may not be enabled at build time

### Common Pitfalls to Avoid

1. **Don't break layer access keys** - Ensure `MO(1)` and `MO(2)` remain accessible
2. **Don't forget KC_ prefix** - QMK keycodes require the prefix
3. **Watch firmware size** - Corne uses Pro Micro with limited flash space
4. **Test both halves** - OLED display code differs between master and slave sides
5. **Maintain symmetry** - Split keyboard layouts should feel balanced

## Testing and Validation

### Pre-Commit Checks

1. **Syntax validation:** Ensure keymap compiles
   ```bash
   qmk compile -kb crkbd -km sulrich
   ```

2. **Keymap completeness:**
   - All 42 keys defined for each layer
   - No undefined keycodes
   - Layer access keys functional

3. **Documentation sync:**
   - README.md matches keymap.c layer definitions
   - Any config changes documented

### Physical Testing (Post-Flash)

1. Test all keys on base layer
2. Verify layer switching with thumb keys
3. Test modifier combinations (especially Cmd, Ctrl)
4. Verify OLED display shows correct layer names
5. Test media keys (layer 2)
6. Verify arrow keys (layer 1, hjkl positions)

## QMK Resources

- **Official QMK Documentation:** https://docs.qmk.fm/
- **Keycode Reference:** https://docs.qmk.fm/#/keycodes
- **CRKBD Build Guide:** https://github.com/foostan/crkbd
- **QMK Configurator:** https://config.qmk.fm/ (visual keymap editor)

## Contact and Contributions

**Maintainer:** @sulrich (https://github.com/sulrich)
**Primary Use Case:** Personal keymap for daily development work on macOS

When making changes, consider that this is a personal keymap optimized for specific workflows and muscle memory. Significant layout changes should be discussed before implementation.

---

**Last Updated:** 2025-11-19
**Repository State:** Initial CLAUDE.md creation
