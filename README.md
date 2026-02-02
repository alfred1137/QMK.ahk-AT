# QMK.ahk - Keyboard Layers and Home Row Modifiers in AutoHotkey v2

---
## TLDR:
QMK is a powerful open-source firmware for mechanical keyboards, enabling custom key mappings, layers, and shortcuts for enhanced productivity and ergonomics. This AutoHotkey v2 script, split into `QMKClass.ahk` (core logic) and `QMK.ahk` for user-defined shortcuts  brings QMK-like functionality to any keyboard on Windows. It supports **homerow modifiers** (e.g., using `a` as Ctrl), **hold actions** (e.g., hold `h` to snap a window left), **combos** (e.g., `a` + `h` for an actions), and **instant combos** for quick triggers. Benefits include streamlined workflows, reduced finger movement, and highly customizable shortcuts, making it ideal for power users, programmers, and anyone seeking a more efficient keyboard experience.
Similar to my `MouseGestures.ahk` and `8BitDo.ahk` scripts, it uses **context-aware mappings** with global fallbacks, so you only define common actions once. With `OnWebsite.ahk` and Descolada's UIA library, you can create **website-specific** hold shortcuts as well. If enough interest, I may expand that context specificity to combos as well.

## My Modifications

This fork has been customized with the following changes to better suit my workflow:

*   **Automatic Suspension in Fullscreen:** The script now automatically suspends itself when a fullscreen application is detected (e.g., games, movies). This prevents conflicts and improves performance.
*   **Mirrored Home Row Modifiers:** The modifier keys have been remapped to a mirrored layout for easier access with both hands:
    *   `a` and `j` are `Ctrl`
    *   `s` and `k` are `Shift`
    *   `d` and `l` are `Win`
    *   `w` and `i` are `Alt`
*   **Updated Keybindings:** Due to the new modifier layout, many keybindings have been updated or disabled.
    *   The **Editing layer** has been moved from the `f` key to the `h` key.
    *   The **Website layer** has been moved from the `w` key to the `f` key.
    *   Several other layers and holds have been disabled to avoid conflicts.
*   **Disabled Dependencies:** Optional dependencies such as `Monitor Manager`, `scroll`, `mouse`, `OnWebsite`, and `TabActivator` have been disabled by default.

## Examples:
- Custom layer/combos - Holding the 'v' key - and pressing either h, j, k, l for media controls, or 'c' and keys on the right of the keyboard for a numpad without moving your hands. Any key can become it's own 'modifier' of sorts.
- Hold-Actions - Holding 'c' alone past threshold to trigger an action like launch/hide a program. Can be context sensitive.
- Homerow mods - 'a' can be mapped to control, 's' to Shift, and'd' to Win when held down, normally, 'a', 's', 'd' - pressed with k will send Control+Shift+Win+k.

## Benefits/key keatures inlude:
- Ability to try QMK/ZMK-like functionatlity without the need to flash firmware. Edits can be made quickly and reloaded.
- Simpler Syntax, plugs directly into your existing Autohotkey functions
- Buffering and Rollover Support - keys are buffered with a 'First-in-first-out' approach. Even if you release your keys in a slightly different order, they are processed by the oldest in the queue.
- Home row modifiers - supports 'stacking' - if 'a' is control, and 's' is shift, you can press 'asl' and have it send the keys ^+l. One benefit is reduced movement of fingers from the homerow, potentially reducing wrist pain.
- SetupCombo and Setupinstantcombo - any two keys can trigger an action. The former allows rollover support.
- Very accurate timing from query performance counter - to the microsecond, even if on heavy load or on battery saver.
- Light. Tested on a 10+ year-old laptop with no noticeable delay/lag.
-  QMK.ahk does not affect the functionality of normal hotkeys/hotstrings from firing.

## Basic timing logic:
  - Keys that are pressed separately go through very little processing, sent nearly insantly through the buffer
  - When 2 or more keys overlap
        - Normal typing - All released withing threshold (200ms) => sent as taps in order they were typed 
        - 'Quick' - retroactive and proactive
                - Either 200ms after the first key was pressed down (retroactive), or if the second key pressed up before then (proactive) >> action occurs
        - Held - if held past threshold, modifiers, combos, etc. Fires with little to no delay
------

## Quick Example

In `QMK.ahk`, define user shortcuts: (this file has user configurations and the #includes for the QMKClass.ahk and other dependencies

``
    QMK.SetupModifier("a", "Ctrl")  ; 'a' acts as Ctrl when held
    QMK.SetupHold("h", ["global"], (*) => mm.SnapLeft("A"))  ; Hold h to snap left
    QMK.SetupCombo("a", "h", (*) => mm.SnapLeft("A"))  ; a+h snaps window left
    QMK.SetupInstantCombo("a", ";", (*) => SendEvent("{Backspace}"))  ; a+; instantly sends Backspace
    QMK.SetupHold("k", ["youtube.com"], (*) => Send("{Space}"))  ; Hold k on YouTube to play/pause. 
``

---

## Getting Started

Download the source code on Github, unzip it, and open `QMK.ahk` in an editor to view or modify user-defined shortcuts. I have a few suggestions to get you started, but feel free to edit as you'd like! The core logic resides in `QMKClass.ahk` File. Dependencies are included in the same folder, so double-clicking `QMK.ahk` should run it.

---

**Required Files:**
``
    #Requires AutoHotkey v2.0
    #Include QMK ; Core class for shortcut logic
    #Include QMKConfig.ahk  ; User-defined shortcuts
``

**Recommended Dependencies (optional, can be deleted if not needed):**
``
    #Include OnWebsite.ahk  ; URL caching for website-specific shortcuts
    #Include UIA\Lib\UIA.ahk  ; Browser automation
    #Include UIA\Lib\UIA_Browser.ahk  ; Browser-specific automation
    #Include MonitorManager.ahk  ; Window snapping and monitor management
    #Include scroll.ahk  ; Smooth Scrolling
    #Include mouse.ahk  ; Move the mouse using a layer
    #Include TabActivator.ahk  ; Activate a specific tab in Edge. If already on tab, cycle through to next instance. Updates in background.
``

---
For sample mappings, advanced configuration, and troubleshooting, please see [MAPPINGS_AND_CONFIGURATION.md](doc\MAPPINGS_AND_CONFIGURATION.md).
