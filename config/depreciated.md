```

        /*
        v_EXCL: v_EXCL {
            compatible = "zmk,behavior-mod-morph";
            label = "V_EXCL";
            bindings = <&kp EXCL>, <&kp N1>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_AT: v_AT {
            compatible = "zmk,behavior-mod-morph";
            label = "V_AT";
            bindings = <&kp AT>, <&kp N2>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_HASH: v_HASH {
            compatible = "zmk,behavior-mod-morph";
            label = "V_HASH";
            bindings = <&kp HASH>, <&kp N3>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_DOLLAR: v_DOLLAR {
            compatible = "zmk,behavior-mod-morph";
            label = "V_DOLLAR";
            bindings = <&kp DOLLAR>, <&kp N4>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_PERCENT: v_PERCENT {
            compatible = "zmk,behavior-mod-morph";
            label = "V_PERCENT";
            bindings = <&kp PERCENT>, <&kp N5>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_CARET: v_CARET {
            compatible = "zmk,behavior-mod-morph";
            label = "V_CARET";
            bindings = <&kp CARET>, <&kp N6>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_AMPS: v_AMPS {
            compatible = "zmk,behavior-mod-morph";
            label = "V_AMPS";
            bindings = <&kp AMPS>, <&kp N7>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_ASTRK: v_ASTRK {
            compatible = "zmk,behavior-mod-morph";
            label = "V_ASTRK";
            bindings = <&kp ASTRK>, <&kp N8>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_LPAR: v_LPAR {
            compatible = "zmk,behavior-mod-morph";
            label = "V_LPAR";
            bindings = <&kp LPAR>, <&kp N9>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_RPAR: v_RPAR {
            compatible = "zmk,behavior-mod-morph";
            label = "V_RPAR";
            bindings = <&kp RPAR>, <&kp N0>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_COLON: v_COLON {
            compatible = "zmk,behavior-mod-morph";
            label = "COLON";
            bindings = <&kp COLON>, <&kp SEMI>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_UNDER: v_UNDER {
            compatible = "zmk,behavior-mod-morph";
            label = "V_UNDER";
            bindings = <&kp UNDER>, <&kp MINUS>;
            #bbinding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_DBLQ: v_DBLQ {
            compatible = "zmk,behavior-mod-morph";
            label = "V_DBLQ";
            bindings = <&kp DOUBLE_QUOTES>, <&kp SINGLE_QUOTE>;
            #inding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        v_DBLQ: v_DBLQ {
            compatible = "zmk,behavior-mod-morph";
            label = "V_DBLQ";
            bindings = <&kp DOUBLE_QUOTES>, <&kp SINGLE_QUOTE>;
            #binding-cells = <0>;
            mods = <(MOD_LSFT|MOD_RSFT)>;
        };
        */


        cb_df_shift {
            bindings = <&sk LSHIFT>;
            key-positions = <>;
            timeout-ms = <35>;
        };

        cb_comdot_up {
            bindings = <&kp UP>;
            key-positions = <56 57>;
            timeout-ms = <75>;
        };

        cb_m_down {
            bindings = <&kp DOWN>;
            key-positions = <55 56>;
            timeout-ms = <75>;
        };

        cb_cv_right {
            bindings = <&kp RIGHT>;
            key-positions = <49 50>;
            timeout-ms = <75>;
        };

        cb_xc_left {
            bindings = <&kp LEFT>;
            key-positions = <48 49>;
            timeout-ms = <75>;
        };

        cb_sdf_cmd-shift {
            bindings = <&sk LS(LGUI)>;
            key-positions = <30 31 32>;
            timeout-ms = <75>;
        };

        cb_er_tab {
            bindings = <&kp TAB>;
            key-positions = <>;
            timeout-ms = <75>;
        };

        cb_io_ctrl {
            bindings = <&sk RCTRL>;
            key-positions = <24 25>;
            timeout-ms = <75>;
        };

        cb_jk_rshift {
            bindings = <&sk RSHIFT>;
            key-positions = <>;
            timeout-ms = <35>;
        };

        cb_sd_lcmd {
            bindings = <&sk LCMD>;
            key-positions = <31 30>;
            timeout-ms = <25>;
        };

        cb_kl_rcmd {
            bindings = <&sk RGUI>;
            key-positions = <42 43>;
            timeout-ms = <35>;
        };

        cb_jkl_cmd-shift {
            bindings = <&sk RS(RGUI)>;
            key-positions = <41 42 43>;
            timeout-ms = <50>;
        };

        cb_wr_up {
            bindings = <&kp UP>;
            key-positions = <16 18>;
            timeout-ms = <75>;
        };

        cb_xv_down {
            bindings = <&kp DOWN>;
            key-positions = <48 50>;
            timeout-ms = <75>;
        };

        cb_we_rctrl {
            bindings = <&sk RCTRL>;
            key-positions = <16 17>;
            timeout-ms = <50>;
        };

        cb_wer_shft-ctrl {
            bindings = <&sk LC(LSHIFT)>;
            key-positions = <16 17 18>;
            timeout-ms = <75>;
        };

        cb_uio_shft-ctrl {
            bindings = <&sk RC(RSHIFT)>;
            key-positions = <23 24 25>;
            timeout-ms = <75>;
        };

        cb_62-63_lalt {
            bindings = <&sk LALT>;
            key-positions = <62 63>;
            timeout-ms = <75>;
        };

        cb_72-73_ralt {
            bindings = <&sk RALT>;
            key-positions = <72 73>;
            timeout-ms = <75>;
        };

        cb_r5l_spc {
            bindings = <&kp SPACE>;
            key-positions = <62 63 64>;
        };

        cb_r5r_spc {
            bindings = <&kp SPACE>;
            key-positions = <71 72 73>;
        };
```
