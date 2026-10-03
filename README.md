# endless-sky_faux-gamepad
Fake controller support for Endless Sky through a UI plugin and AntimicroX profile and game internal key binds.

Endless Sky does not natively support a gamepad, but almost every function is bound to keyboard keys.
Hardly anything strictly requires a mouse.

As such, a very high 'fake' support can be achieved by combining a few techniques to get as many different key strokes onto the controller as possible.  
But these must also be documented, which is why the plugin 'mods in' button labels.

**This plugin requires tools and config outside of the game to be operational!**  
The plugin itself does not generate keystrokes from controllers or start external tools as this is not possible
with the data scripting of ES. It is more of a 'documentation' of the controller mappings.

**This is in a 'proof-of-concept' state** - the UI additions are a fast work done just to get something usable.

## TL;DR

1. Install AntiMicroX (use original from Github, sadly there exist scammer websites for AMX)
2. Get the plugin source into the `config/plugins` folder of Endless Sky.
3. Copy the keybinding setup file from `resources` (or its content) into your ES config dir.  
   No deviations/customizations if you don't plan to change the plugin and `.amgp` files as well to match it.
4. Use whatever script, wrapper, autostart config you want to make sure AntiMicroX is started with the `.amgp` file from the plugin's `resoures` folder when launching the game.

## Strategy:

1. A UI plugin enriches button labels and occasionally adds keybind hint labels.  
   Without this, the myriad of different contexts and functions are hard to learn and remember.  
   The plugin alone can't change ALL labels, some are hardcoded. For this, the `resource` directory contains 
   a rudimentary patch for version v.0.11.2.
2. A pre-configured profile for AntiMicroX is part of the plugin sources.  
   The user has to manually configure this
   profile for AntiMicroX outside of the game.
3. Assign as many of the 'hard-coded' key functions to controller buttons as possible, prefering multi-purpose key
   variants wherever possible.
4. 'Double up' on keys by re-using the 'hard-coded' keys in the freely bindable key contexts where possible without conflicts.  
   The keybind config will also be part of the sources, the user will have to either manually assign the 
   keys through the UI, or directly copy the settings file.
5. Use a special 'mouse mode' profile subset for anything that requires a mouse, users can toggle this manually with
   a button.
6. A select few functions will be dropped when there is a roughly working equivalent.

**Used features from AntiMicroX:**

 - Double functions on some buttons through 'click' and 'hold' definitions
 - 'meta' bindings key to shift to another profile subset

Aside from text input fields and ship group selections in the Shipyard and Outfitter, most of the basic gameplay can easily
be covered by a gamepad.

I'm including an `.amgp` config file for AMX because i am familiar with its capabilities and how to configure it.  
The SteamInput (and possibly other input mappers as well) support a similar featureset. As long as a relatively small subset
of AMX features is used, it is likely possible to 'port' the config for usage with SteamInput (or others).  
But i rarely use Steam so i won't make a file for it, but contributions related to that are welcome!

## Button labels

For now, the plugin uses textual representations for buttons. These labels are defined according to the following rules:

- Any single press, long press (hold) or sequence is enclosed in `()` to set it apart from the actual button label.
- Analog sticks are named after their position (first character)
- Face buttons by their Xinput (xbox/ms) names
- Directions up, right, down, left are represented by `^`, `>`, `v`, `<` respectively and can be prefixed with a stick position
- Simple, short presses just place the keys in braces, **holding down** is signified by `*` as prefix.  
  This can only be added to buttons where it makes sense.  
  (when the respective button isn't used for a continuous effect already => face buttons, bumpers, the 'middle buttons', dpad)
- Button sequences, where one must be held before the next one has to be pressed

**Result:**

- Face Buttons: `(A)`, `(B)`, `(X)`, `(Y)`
- Direction indicators for stick and dpad axis: `^`, `>`, `v`, `<`
- Analog sticks: `(L)`, `(R)` for clicking it down. `(Lx)` and `(Rx)` for directions.
- Bumpers: `(LB)`, `(RB)`
- Triggers: `(LT)`, `(RT)`
- The 'middle buttons' guide, 'view' (=select) and 'menu' (=start): `(G)`, `(S)`, `(M)`
- Holding down (long press): `(*[X])`, where `[X]` can be any of the above except triggers, sticks **with** directions and guide.
- Sequences using one button as a modifier: `([MOD]>[X])`, where `[MOD]` will primarily be a bumper and `[X]` any button or direction != `[MOD]`.  
  Here, it is assumed that `[M]` must be held continuously even when it is **not** prefixed with `*`.  
  `[X]`, on the contrary, will be prefixed with `*` if it has to be held down.

## Key Mappings / Functions

Given the rules and possibilities above, a regular xbox-style controller will have around 36 assignable key if one of the bumpers is used as modifier.

Its possible to cover any basic keyboard shortcut using lowercase letters and a few uppercase situations as well.
The hardest part when designing the mapping is **consistency**, which means:
- Buttons responsible for UI navigation should be the same in every panel:
    - Close or 'leave' a panel should be identical everywhere.  
    - Next and previous should be the same. This is currently NOT possible.  
      Problem: Different panels in ES use different keys for 'previous': In the map, it's `r`, but `p` for ship info.
- Thematically related functions should not be littered across different 'sections' of the controller (sticks, dpad, face buttons).  
  Example: Fleet commands should not be spread out across dpad and face buttons.

See [My working sheet](resources/esky-keys.ods) for my research details about keys, functions and interfaces and which keyboard keys i've
finally added into the controller profile.

## TODOs:

- [ ] Use glyphs/png icons instead of just patching in button names
- [ ] Make the selected glyph style configurable somehow, e.g. by a abusing a mission, outfit or conversation.
    (Alternatively try to contribute a way to ES in which plugins can hook into game prefs and provide custom keys)
- [ ] Change key tutorial popups to show button names instead of keyboard keys.
- [ ] Make key subset change visible, e.g. via a notification (linux only, lots of caveats)

## Problems and Caveats:

- Some UIs in general don't work without mouse (Preferences - changing options).  
  The fallback 'mouse mode' works for that, but it can't revert back and there is no way to display the current mode from within the game.
- Typing names. In theory, almost every key of the alphabet is somewhere in the controller, but these are interna.  
  What's needed here would be a proper on-screen keyboard.
- Shops permit changing control focus between the 'left' panel and the 'ship' panel, but there is no visual indication.
- Several of the patched labels in the POC are larger than their buttons/borders, leading to ugly overflow at the moment.
- It is close to impossible to get a streamlined control schema across all UI windows because the very same keys  are often used for wildly different things depending on context. The reason for that is that keys were assigned in a FIFO order based on starting letters of labels (as is usually done for menus and shortcuts in regular desktop applications). This results e.g. in having all functions from the outfitter spread out across the controller (Shoulder, trigger, face button, dpad) instead of placing them all in the same area. As external key providers like AntiMicroX are not aware of in-game contexts and there's no interaction between them, context changes are neither communicated nor visible.
    It would be possible to define a special shopping subset in AntiMicroX, but the user can't see which set is currently active which can quickly lead to confusion about the effective control schema. Using a special secondary 'mouse mode' can already lead to such confusion as it is.
  
