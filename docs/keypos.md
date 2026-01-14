
# Keymapping for the Kinesis Advantage 360 Pro with ZMK

<!-- vim: ts=4 sw=4 sts=4 et -->

### Key Positions/Numbers

-   the kineses advantage 360 pro (henceforth "keyboard" or "kb") is a split keyboard with left and right sides
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

### Hold-Tap 

-   here is an example hold-tap behavior with all properties listed

```

hm: homerow_mods {
    label = "HOMEROW_MODS";
    // ====== dont edit ok
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>;
    // ====== timing properties
    tapping-term-ms = <200>; // hold time, this needs to be largest timing prop
    quick-tap-ms = <150>;
    require-prior-idle-ms = <0>; // prevent hold during fast typing (default = <0>)
    // ====== interupt flavor
    flavor = "tap-preferred";
    // ====== behavior bindings
    bindings = <&kp>, <&kp>; // the first binding is the hold behavior
    // ====== positional hold-tap
    //hold-trigger-key-positions = <1 2 3>; // default: not set 
    //hold-trigger-on-release; // default: not set (false)
    // ====== special behaviors
    //hold-while-undecided; // defaut: not set (false) | holds modifier immediately on press (bad for fast typing)
    //hold-while-undecided-linger; // defaut: not set (false) | continues hold during sticky key transition (no effect on typing speed)
    //retro-tap; // defaut: not set (false) | tap on release if not interupted
};

```
