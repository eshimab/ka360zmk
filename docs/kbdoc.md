
<!--vim: ts=4 sw=4 sts=4 et -->

# Keymapping for the Kinesis Advantage 360 Pro with ZMK

-   the kineses advantage 360 pro (henceforth "the keyboard" or "the kb") is a split keyboard with left and right sides

## Syntax Hints

-   to remove a property that defaults to enabled, prepend the property with `/delete-property/`
-   here is an example of property disabling with sticky key (`&sk`), which by default has  `ignore-modifiers` enabled
```

&sk {
    release-after-ms = <2000>;
    // quick-release; // property default is disabled if not explicitly listed
    ignore-modifiers; // property default is enabled 
}

&sk {
    release-after-ms = <2000>;
    quick-release; // property is now enabled
    /delete-property/ ignore-modifiers; // property is now disabled
}

```

### Using #define Macros in ZMK Keymaps

-   In ZMK, C preprocessor macros like `#define` are syntactic sugar to simplify repetitive device tree bindings in .keymap files. 
-   The macro expands before compilation, replacing placeholders with provided arguments for cleaner code.
-   Example: Layer Toggle Macro

```
#define MO_TOG(layer) &mo_tog layer layer   // Macro for momentary-on-hold/toggle-on-tap targeting the same layer
// Usage in keymap:
bindings = <
    MO_TOG(3)       // Expands to &mo_tog 3 3
>;
// this becomes the following before being parsed by zmk.
bindings = <
    &mo_tog 3 3    // Expands to &mo_tog 3 3
>;
```

-   This macro has no functional impact beyond text substitution—it saves typing and reduces errors when targeting identical layers for hold and tap behaviors. For different layers, use &mo_tog X Y directly.




---




## Behaviors

### Sticky Key

-   here is an example of all sticky key properties

```
skq: sticky_key_quick_release {
    compatible = "zmk,behavior-sticky-key";
    #binding-cells = <1>;
    bindings = <&kp>;
    release-after-ms = <1000>; // how long the sk is on before turned off
    //
    quick-release; // default = disabled, list to enable
    // default sk is active until next key has been pressed and released.
    // quick-release changes this to the sk being deactivated after next key pressed
    // quick-release is good for fast typing
    //
    ignore-modifiers; // default = enabled, use prepend `/delete-property/ to disable`
    // this lets you stack sk. 
    // if you want to use modifers like `&sk LS(ALT)` you need this enabled 
    //
    lazy; // default = disable, list for enable
    // default sk are activated on press until another key is pressed
    // enable lazy to not activate sk until the next key is pressed
    // this could help with mouse movement, so the mouse does not have the sk active
    // tapping a lazy sk will not trigger other behaviors e.g. release of other sk or layer
    // to make a lazy sk trigger those release behaviors: 
    //     wrap the sk in a macro that presses the release of the sticky behavior around your sk press or triggering a tap of a key like K_CANCEL
    
};
```


### Hold-Tap (ht)

-   hold-tap Behavior is divided into mod-tap and layer-tap, but the function is essentially the same.
-   in hold-tap definitions, the first `<&kp>` is the hold, and second is the tap
-   stock zmk defines two ht behaviors:
-   mod-tap `&mt` for modifier keys
    -   for mod-tap, default flavor is `hold-preferred`
    -   mt can be implemented "inline" with just `&mt LSHIFT A` where `LSHIFT` is the hold, and `A` is the tap
-   layer-tap `&lt` for (usually) `&mo` layers
    -   for layer-tap, default flavor is `tap-preferred`


![tapping-term-ms example](img/zmk-hold-tap.svg)

#### Hold-Tap Limitations
-   the hold-tap behavior has some limitations 
    -   the hold behavior cannot roll directly into a &sk modifier
    -   you can get around this by putting a &sk inside of a macro
        -   but that will prevent stacking &sk modifiers


#### Hold-tap properties

-   here is an example hold-tap behavior with all properties listed

```
hm: homerow_mods {
    label = "HOMEROW_MODS";
    // ====== init
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>; // probably is always 2
    //
    // ====== timing properties
    tapping-term-ms = <200>; // time to trigger hold, this needs to be largest timing prop
    //
    quick-tap-ms = <150>; // default disabled
    // timespan where pressing the same hold-tap key again triggers a second press
    // useful for keys like bspc, where you may tap then hold quickly to delete
    //
    require-prior-idle-ms = <0>; // prevent hold during fast typing (default = <0>)
    //
    // ====== interupt flavor
    flavor = "tap-preferred"; // 
    //
    // ====== behavior bindings
    bindings = <&kp>, <&kp>; // the first binding is the hold behavior
    //
    // ====== special behaviors
    hold-trigger-key-positions = <1 2 3>; // default: not set 
    hold-trigger-on-release; // default: not set (false)
    hold-while-undecided; // defaut: not set (false) | holds modifier immediately on press (bad for fast typing)
    hold-while-undecided-linger; // defaut: not set (false) | continues hold during sticky key transition (no effect on typing speed)
    //
    retro-tap; // defaut: not set (false) | tap on release if not interupted
};

```

#### Hold-Tap Interrupt `flavor`

-   `hold-preferred` flavor triggers the hold behavior when the `tapping-term-ms` has expired or another key is pressed.
-   `balanced` flavor will trigger the hold behavior when the `tapping-term-ms` has expired or another key is pressed and released while the hold-tap is held.
-   `tap-preferred` flavor triggers the hold behavior when the `tapping-term-ms` has expired. Pressing another key within tapping-term-ms does not affect the decision.
-   `tap-unless-interrupted` flavor triggers a hold behavior only when another key is pressed before tapping-term-ms has expired. It triggers the tap behavior in all other situations. Note that this flavor inverts the decision logic with respect to the tapping term.

![hold-tap flavors](img/zmk-hold-tap-flavors.svg)

#### `quick-tap-ms`

If you press a tapped hold-tap again within quick-tap-ms milliseconds of the first press, it will always trigger the tap behavior. This is useful for keys like backspace, where a quick tap-then-hold can be used to hold it down to delete long parts of text. By default this behavior is disabled.

#### `require-prior-idle-ms`

If a hold-tap is pressed within require-prior-idle-ms of another non-modifier key (not behavior), then the hold-tap will always resolve in a tap. This effectively disables the hold-tap when typing quickly, which can be quite useful for home-row mods. It can also have the effect of removing the input delay when typing quickly, since the hold-tap immediately resolves to a tap on key press.

-   The following hold-tap configuration enables require-prior-idle-ms with a 125 millisecond term, alongside quick-tap-ms with a 200 millisecond term.

```
rpi: require_prior_idle {
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>;
    flavor = "tap-preferred";
    tapping-term-ms = <200>;
    quick-tap-ms = <200>;
    require-prior-idle-ms = <125>;
    bindings = <&kp>, <&kp>;
};
```
If you press &kp A and then &rpi LEFT_SHIFT B within 125 ms, then ab will be output. Importantly, b will be output immediately since it was within the require-prior-idle-ms, without waiting for a timeout or an interrupting key. In other words, the &rpi LEFT_SHIFT B binding will only have its underlying hold-tap behavior if it is pressed 125 ms after the previous key press; otherwise it will act like &kp B.

Note that the greater the value of require-prior-idle-ms is, the harder it will be to invoke the hold behavior, making this feature less applicable for use-cases like capitalizing letters while typing normally. However, if the hold behavior isn't used during fast typing, then it can be an effective way to mitigate misfires.

#### Positional hold-tap and `hold-trigger-key-positions`

Including hold-trigger-key-positions in your hold-tap definition turns on the positional hold-tap feature. With positional hold-tap enabled, if you press any key not listed in hold-trigger-key-positions before tapping-term-ms expires, it will produce a tap.

In all other situations, positional hold-tap will not modify the behavior of your hold-tap. Positional hold-tap is useful when used with home-row modifiers: for example, if you have a home-row modifier key in the left hand, by including only key positions from the right hand in hold-trigger-key-positions, you will only get hold behaviors during cross-hand key combinations unless you exceed tapping-term-ms when using "balanced" or "hold-preferred" flavors.

For home-row mods, it is recommended to use this property with hold-trigger-on-release so that modifiers on the same hand can be combined.

NOTE: hold-trigger-key-positions is an array of key position indexes. Key positions are numbered sequentially according to your keymap, starting with 0. So if the first key in your keymap is Q, this key is in position 0. The next key (probably W) will be in position 1, et cetera.

-   The following example uses a hold-tap behavior definition configured with the hold-preferred flavor, and with positional hold-tap enabled:

```
#include <dt-bindings/zmk/keys.h>
#include <behaviors.dtsi>

/ {
    behaviors {
        pht: positional_hold_tap {
            compatible = "zmk,behavior-hold-tap";
            #binding-cells = <2>;
            flavor = "hold-preferred";
            tapping-term-ms = <400>;
            quick-tap-ms = <200>;
            bindings = <&kp>, <&kp>;
            hold-trigger-key-positions = <1>;    // <---[[the W key]]
        };
    };
    keymap {
        compatible = "zmk,keymap";
        default_layer {
            bindings = <
                //  position 0         position 1       position 2
                &pht LEFT_SHIFT Q        &kp W            &kp E
            >;
        };
    };
};
```

-   The sequence (pht_down, E_down, E_up, pht_up) produces qe. The normal hold behavior (LEFT_SHIFT) IS modified into a tap behavior (Q) by positional hold-tap because the first key pressed after the hold-tap key is the E key, which is in position 2, which is NOT included in hold-trigger-key-positions.
-   The sequence (pht_down, W_down, W_up, pht_up) produces W. The normal hold behavior (LEFT_SHIFT) is NOT modified into a tap behavior (Q) by positional hold-tap because the first key pressed after the hold-tap key is the W key, which is in position 1, which IS included in hold-trigger-key-positions.
-   If the LEFT_SHIFT / Q key is held by itself for longer than tapping-term-ms, a hold behavior is produced. This is because positional hold-tap only modifies the behavior of a hold-tap if another key is pressed before the tapping-term-ms period expires.
-   By default, hold-trigger-key-positions are evaluated upon the first key press after the hold-tap. For home-row mods, this is not always ideal, because it prevents combining multiple modifiers unless they are included in hold-trigger-key-positions. To overwrite this behavior, one can set hold-trigger-on-release. If set to true, the evaluation of hold-trigger-key-positions gets delayed until key release. This allows combining multiple modifiers when the next key is held, while still deciding the hold-tap in favor of a tap when the next key is tapped.

##### `hold-trigger-on-release`

If set, instead of the keys listed in hold-trigger-key-positions producing a tap when pressed before tapping-term-ms expires, they instead produce a tap when released before tapping-term-ms expires.

#### `hold-while-undecided`

If enabled, the hold behavior will immediately be held on hold-tap press, and will release before the behavior is sent in the event the hold-tap resolves into a tap. With most modifiers this will not affect typing, and is useful for using modifiers with the mouse.

##### `hold-while-undecided-linger`

If your tap behavior activates the same modifier as the hold behavior, and you want to avoid a double tap when transitioning from the hold to the tap, you can use hold-while-undecided-linger. When enabled, the hold behavior will continue to be held until after the tap behavior is released.

For example, if the hold is &kp LGUI and the tap is &sk LGUI, then with hold-while-undecided-linger enabled, the host will see LGUI held down continuously until the sticky key is finished, instead of seeing a release and press when transitioning from hold to sticky key.

#### `retro-tap`

If retro-tap is enabled, the tap behavior is triggered when releasing the hold-tap key if no other key was pressed in the meantime. The hold key does not activate until another key is pressed, meaning that it cannot be used for mouse events like Shift Click to select from your cursor position to mouse position.

For example, if you press &mt LEFT_SHIFT A and then release it without pressing another key, it will output a.

#### Custom Hold-tap examples

##### Homerow mods

The most popular form of home-row mods is known as "timeless home-row mods", configured to minimize the dependency on timing. Timeless home-row mods define both a "left hand" and a "right hand" behavior.

-   here is an example of left and right sides homerow mods

```
/ {
    behaviors {
        hml: home_row_mod_left {
            compatible = "zmk,behavior-hold-tap";
            #binding-cells = <2>;
            flavor = "balanced";
            require-prior-idle-ms = <150>;
            tapping-term-ms = <280>;
            quick-tap-ms = <175>;
            bindings = <&kp>, <&kp>;
            hold-trigger-key-positions = < ... >; // List of keys on the right side of the keyboard
            hold-trigger-on-release;
        };
        hmr: home_row_mod_right {
            compatible = "zmk,behavior-hold-tap";
            #binding-cells = <2>;
            flavor = "balanced";
            require-prior-idle-ms = <150>;
            tapping-term-ms = <280>;
            quick-tap-ms = <175>;
            bindings = <&kp>, <&kp>;
            hold-trigger-key-positions = < ... >; // List of keys on the left side of the keyboard
            hold-trigger-on-release;
        };
    };
};
```
-   If you wish to use this configuration of home-row mods, you will need to define the corresponding left and right behaviors as above, then use &hml for home-row keys on the left side of the keyboard, and &hmr for ones on the right side.
-   You may configure this behavior to your speed of typing using the `flavor` options


##### autoshift

A popular method of implementing Autoshift in ZMK involves a C-preprocessor macro, commonly defined as AS(keycode). This macro applies the LSHIFT modifier to the specified keycode when AS(keycode) is held, and simply performs a keypress, &kp keycode, when the AS(keycode) binding is tapped. This simplifies the use of Autoshift in a keymap, as the complete hold-tap bindings for each desired Autoshift key, as in &as LS(<keycode 1>) <keycode 1> &as LS(<keycode 2>) <keycode 2> ... &as LS(<keycode n>) <keycode n>, can be quite cumbersome to use when applied to a large portion of the keymap.
 
```
#include <dt-bindings/zmk/keys.h>
#include <behaviors.dtsi>

#define AS(keycode) &as LS(keycode) keycode     // Autoshift Macro

/ {
    behaviors {
        as: auto_shift {
            compatible = "zmk,behavior-hold-tap";
            #binding-cells = <2>;
            tapping_term_ms = <135>;
            quick_tap_ms = <0>;
            flavor = "tap-preferred";
            bindings = <&kp>, <&kp>;
        };
    };

    keymap {
        compatible = "zmk,keymap";
        default_layer {
            bindings = <
                AS(Q) AS(W) AS(E) AS(R) AS(T) AS(Y) // Autoshift applied for QWERTY keys
            >;
        };
    };
};
```

##### Tog-tap, Mo-Hold Layers 

This hold-tap example implements a momentary-layer when the keybind is held and a toggle-layer when it is tapped. Similar to the Autoshift and Sticky Hold use-cases, a MO_TOG(layer) macro is defined such that the &mo and &tog behaviors can target a single layer.

```
#include <dt-bindings/zmk/keys.h>
#include <behaviors.dtsi>

#define MO_TOG(layer) &mo_tog layer layer   // Macro to apply momentary-layer-on-hold/toggle-layer-on-tap to a specific layer

/ {
    behaviors {
        mo_tog: behavior_mo_tog {
            compatible = "zmk,behavior-hold-tap";
            #binding-cells = <2>;
            flavor = "hold-preferred";
            tapping-term-ms = <200>;
            bindings = <&mo>, <&tog>;
        };
    };

    keymap {
        compatible = "zmk,keymap";
        default_layer {
            bindings = <
                &mo_tog 2 1     // &mo 2 on hold, &tog 1 on tap
                MO_TOG(3)       // &mo 3 on hold, &tog 3 on tap
            >;
        };
    };
};
```

##### Mod/Layer-tap defaults

-   invoking `&mt` or `&lt` has these options by default

```
#include <dt-bindings/zmk/behaviors.h>

// mod-tap (&mt) default
/ {
    behaviors {
        mt: mod_tap {
            compatible = "zmk,behavior-hold-tap";
            #binding-cells = <2>;
            flavor = "hold-preferred";
            tapping-term-ms = <200>;
            bindings = <&kp>, <&kp>;
            display-name = "Mod-Tap";
        };

        lt: layer_tap {
            compatible = "zmk,behavior-hold-tap";
            #binding-cells = <2>;
            flavor = "tap-preferred";
            tapping-term-ms = <200>;
            bindings = <&mo>, <&kp>;
            display-name = "Layer-Tap";
        };
    };
};

```








### Mod-Morph

-   here is an example of stacked mod-morph. 
-   when `&morph_ABC` is pressed, it will output `A` by default.
-   when `&morph_ABC` is pressed with shift, it will output `B`, and if pressed with shift and control, it will output `C`

```
morph_BC: morph_BC {
    compatible = "zmk,behavior-mod-morph";
    #binding-cells = <0>;
    bindings = <&kp B>, <&kp C>; // first term is the no-mod tap
    mods = <(MOD_LCTL|MOD_RCTL)>; // modifers this is active for
};
morph_ABC: morph_ABC {
    compatible = "zmk,behavior-mod-morph";
    #binding-cells = <0>;
    bindings = <&kp A>, <&morph_BC>;
    mods = <(MOD_LSFT|MOD_RSFT)>;
};
// this is an example with the keep-mods property
bspc_del: backspace_delete {
    compatible = "zmk,behavior-mod-morph";
    #binding-cells = <0>;
    bindings = <&kp BACKSPACE>, <&kp DELETE>;
    mods = <(MOD_LSFT|MOD_RSFT)>;
    keep-mods = <(MOD_RSFT)>; // default = 0 i.e. not active
    // keep-mods keeps modifers active and applied to the mod behavior
    // in this example, a normal press is BSP, LS(BSP) = DEL, and RS(BSP) = RS(DEL)
};
```

-   Note that Krabiner-Elements will cause problems on MacOS
-   disable "Modify Events" in Karabiner-Elements to stop the interference


### Tap-Dance



---


## Advantage 360 Details

### Key Positions/Numbers

-   the kineses advantage 360 pro is a split keyboard with left and right sides
-   for the purposes of zmk, the key positions/numbers are in the code block below
-   the keyboard has thumb clusters, one on each side. 
    -   the left thumb cluster contains key numbers 35, 36, 52, 65, 66, 67
    -   right cluster: 37, 38, 53, 68, 69, 70

```
keynums {
bindings = <
// |---------- left ---------------|-------------- right -----------|
    00 01 02 03 04 05 06           :            07 08 09 10 11 12 13
    14 15 16 17 18 19 20           :            21 22 23 24 25 26 27
    28 29 30 31 32 33 34    35 36  :  37 38     39 40 41 42 43 44 45
    46 47 48 49 50 51          52  :  53           54 55 56 57 58 59
    60 61 62 63 64       65 66 67  :  68 69 70        71 72 73 74 75
//                      |--- thumb clusters --| 
>;
};
```


## ZMK Behavior Examples with Default Properties

Thanks for confirming the behaviors (hold-tap, sticky-key, combo). Based on official ZMK docs (zmk.dev/docs), here's the refined plan to show examples with all available properties set to defaults, including same-line comments on what each does. I've researched and compiled them for conciseness while retaining detail.

**Key Assumptions (Based on Your Confirmation):**
- Hold-Tap: Mod-tap variant (most common, as per docs).
- Sticky-Key: Lazy sticky (includes `lazy` and related properties).
- Combo: Basic simultaneous combo (includes timeout and idle properties).
- Defaults: From ZMK dev docs (e.g., `tapping-term-ms = <200>`). Properties are listed in logical order.
- Interactions: Noted in comments where properties affect each other (e.g., `require-prior-idle-ms` can override `tapping-term-ms`).
- No Customizations: All set to defaults; ready for copy-paste into a `.keymap` file.

**Research Summary:**
- Sourced from zmk.dev/docs/behaviors/hold-tap, sticky-key, combos, and config docs.
- Properties: Focused on documented ones; omitted non-timing like `label` unless core.
- Differences: Hold-tap focuses on hold/tap decisions; sticky-key on release timing; combo on recognition window.

**Examples (In Code Blocks):**

**Hold-Tap (Mod-Tap Example):**
```
my_mt: my_mod_tap {
    compatible = "zmk,behavior-hold-tap"; // Defines this as a hold-tap behavior
    #binding-cells = <2>; // Number of parameters for bindings (hold and tap)
    flavor = "hold-preferred"; // Interrupt handling: prefers hold on key press (interacts with tapping-term-ms)
    tapping-term-ms = <200>; // Max time (ms) to decide hold vs. tap; shorter for faster response
    quick-tap-ms = <0>; // Disabled; if set, allows quick re-tap for tap behavior (overrides tapping-term-ms for repeats)
    require-prior-idle-ms = <0>; // Disabled; if set, requires idle time before behavior starts (prevents fast-typing triggers, interacts with tapping-term-ms)
    bindings = <&kp>, <&kp>; // Hold action, tap action (e.g., &kp LSHIFT for hold, &kp A for tap)
};
```

**Sticky-Key (Lazy Sticky Example):**
```
my_sk: my_sticky_key {
    compatible = "zmk,behavior-sticky-key"; // Defines this as a sticky key behavior
    #binding-cells = <1>; // Number of parameters for bindings (key to make sticky)
    bindings = <&kp>; // Key to make sticky (e.g., &kp LSHIFT)
    release-after-ms = <1000>; // Time (ms) sticky key stays active if no other key pressed; longer for sustained use
    lazy; // If set, activates on next key press instead of immediate (avoids host seeing alone-press)
    ignore-modifiers; // If set, ignores modifier state (e.g., doesn't cancel on shift press)
    quick-release; // If set, releases on next key press instead of release (better for typing, but may cause double-shifts)
};
```

**Combo (Basic Simultaneous Combo Example):**
```
my_combo: my_combo {
    compatible = "zmk,behavior-combo"; // Defines this as a combo behavior
    #binding-cells = <1>; // Number of parameters for bindings (action to trigger)
    bindings = <&kp>; // Action to trigger (e.g., &kp A)
    key-positions = <0 1>; // Key positions that form the combo (numbered from 0 in keymap)
    timeout-ms = <50>; // Max time (ms) for all keys to be pressed (combo window; shorter for precision, interacts with require-prior-idle-ms)
    require-prior-idle-ms = <0>; // Disabled; if set, requires idle time before combo starts (prevents accidental triggers, interacts with timeout-ms)
    layers = <0>; // Layers where combo is active (bitmask; 0 for default layer only)
};
```

**Key Differences and Interactions:**
- **Hold-Tap:** Timing focuses on single-key hold/tap resolution; `require-prior-idle-ms` can block the entire behavior if not met, overriding `tapping-term-ms`.
- **Sticky-Key:** Timing controls release duration; `quick-release` and `lazy` interact to balance responsiveness vs. reliability (e.g., lazy prevents menu activation).
- **Combo:** Timing ensures simultaneous press within `timeout-ms`; `require-prior-idle-ms` adds a "cooldown" to avoid spam, potentially conflicting with `timeout-ms` if too high.
- **Cross-Behavior:** All `-ms` properties are in milliseconds; adjust based on typing speed/hardware latency. Defaults prioritize usability but may need tuning for fast typists.
