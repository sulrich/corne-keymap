# sulrich crkbd layout

this is the personal keyboard layout of [@sulrich](https://github.com/sulrich),
and is heavily oriented towards operation in the Mac OS environment.

there are no LED rules in this keymap and the OLED display uses the default
corne images

## layout notes

this layout uses a standard QWERTY layout on the default layer followed by a
separate layer for symbols and numbers (and F-keys)

### thumb cluster: left

this contains the Mac OS modifier keys as well as the enter key.  these are
transparently mapped through the remaining layers with the exception of the
enter key which is toggled to backspace on the symbols layer.

### thumb cluster: right

the largest right thumb button provides the space and the layer toggles.

### default layer
```text
  //,-----------------------------------------------------.                    ,-----------------------------------------------------.
          TAB,       Q,       W,       E,       R,       T,                            Y,       U,       I,       O,      P,        \,
  //|--------+--------+--------+--------+--------+--------|                    |--------+--------+--------+--------+--------+--------|
         LCTL,       A,       S,       D,       F,       G,                            H,       J,       K,       L,       ;,       ',
  //|--------+--------+--------+--------+--------+--------|                    |--------+--------+--------+--------+--------+--------|
         LSFT,       Z,       X,       C,       V,       B,                            N,       M,       ,,       .,       /,    RSFT,
  //|--------+--------+--------+--------+--------+--------+--------|  |--------+--------+--------+--------+--------+--------+--------|
                                             OPT,      CMD,   ENTER,        SPC,   MO(1),   MO(2)
                                      //`--------------------------'  `--------------------------'
```

### symbols layer [1]

this layer contains all of the standard symbols.  the arrow keys are mapped to
the default vi bindings for single character motion (h, j, k, l).

```text
  //,-----------------------------------------------------.                    ,-----------------------------------------------------.
          ESC,       !,       @,       {,       },       |,                            ^,       +,       =,       -,       *,        ,
  //|--------+--------+--------+--------+--------+--------|                    |--------+--------+--------+--------+--------+--------|
             ,       #,     $,         (,       ),       `,                         LEFT,    DOWN,      UP,   RIGHT,        ,   ENTER,
  //|--------+--------+--------+--------+--------+--------|                    |--------+--------+--------+--------+--------+--------|
             ,       %,       ^,       [,       ],       ~,                            _,       &,       <,       >,      \,         ,
  //|--------+--------+--------+--------+--------+--------+--------|  |--------+--------+--------+--------+--------+--------+--------|
                                             OPT,      CMD,  BSPACE,        SPC,   MO(1),   MO(2)
                                      //`--------------------------'  `--------------------------'
```

### number/media layer [2]

this layer has all of the numbers and the F-keys mapped to reasonably intuitive
locations.  additionally, media controls allow for quick access to volume and
track changes.

```text
  //,-----------------------------------------------------.                    ,-----------------------------------------------------.
          ESC,      F1,      F2,      F3,      F4,      F5,                           F6,      F7,      F8,      F9,     F10,     F11,
  //|--------+--------+--------+--------+--------+--------|                    |--------+--------+--------+--------+--------+--------|
             ,       1,       2,       3,       4,       5,                            6,       7,       8,       9,       0,     F12,
  //|--------+--------+--------+--------+--------+--------|                    |--------+--------+--------+--------+--------+--------|
             ,    PREV,    PLAY,    NEXT,  VOL_DN,  VOL_UP,                             ,    MUTE,        ,        ,        ,        ,
  //|--------+--------+--------+--------+--------+--------+--------|  |--------+--------+--------+--------+--------+--------+--------|
                                             OPT,      CMD,   ENTER,        SPC,   MO(1),   MO(2)
                                      //`--------------------------'  `--------------------------'

```

### special modifiers

currently, caps word is enabled and bound to a double-tap of the left shift key.
shift invert is also enabled with the default invert callbacks that are
described in the qmk documentation.

### RGB under-lighting

currently this is set to be disabled. this is appropriately called out in the
`rules.mk` file.

### 20260401 notes to self

i'm able to build this with the vanilla qmk compilation process.  the flashing
process is a bit of a hassle and it's hung up on the fact that i'm currently
using a clone controller (pro?, iirc)  this entails a slightly different
flashing process.

```text
# for DFU bootloader
dfu-programmer atmega32u4 erase
dfu-programmer atmega32u4 flash ./crkbd_rev1_sulrich.hex
dfu-programmer atmega32u4 reset
```

sometimes i need to hit the reset button on the keyboard to make it get
recognized by the `dfu-programmer` utility.
