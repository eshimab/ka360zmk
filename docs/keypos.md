



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

