# endless-sky_faux-gamepad
Fake controller support for Endless Sky through a UI plugin and AntimicroX profile and game internal key binds.

Endless Sky does not natively support a gamepad, but almost every function is bound to keyboard keys.
Hardly anything strictly requires a mouse.

As such, a very high 'fake' support can be achieved by combining a few techniques to get as many different key strokes
onto the controller as possible. But these must also be documented, which is why the plugin 'mods in' button labels.

**This plugin requires tools and config outside of the game to be operational!**  
The plugin itself does not generate keystrokes from controllers or start external tools as this is not possible
with the data scripting of ES. It is more of a 'documentation' of the controller mappings.

## TL;DR

1. Install AntiMicroX
2. Get the plugin source into the `config/plugins` folder of Endless Sky.
3. Copy the keybinding setup file from `resources` (or its content) into your installation.
   No deviations/customizations if you don't plan to change the plugin and `.amgp` files as well to match it.
4. Use whatever script, wrapper, autostart config you want to make sure AntiMicroX is started with the `.amgp` file
   from the plugin's `resoures` folder when launching the game.

## Strategy:

1. The UI plugin enriches button labels and occasionally adds keybind hint labels
2. A pre-configured profile for AntiMicroX is part of the plugin sources. The user has to manually configure this
   profile for AntiMicroX outside of the game.
3. Assign as many of the 'hard-coded' key functions to controller buttons as possible, prefering multi-purpose key
   variants wherever possible.
4. 'Double up' on keys by re-using the 'hard-coded' keys in the freely bindable key contexts where possible without
   conflicts. The keybind config will also be part of the sources, the user will have to either manually assign the 
   keys through the UI, or directly copy the settings file.
5. Use a special 'mouse mode' profile subset for anything that requires a mouse, user can toggle this manually with
   a button.
6. A select few functions will be dropped when there is a roughly working equivalent.

**Used features from AntiMicroX:**

 - Double functions on some buttons through 'click' and 'hold' definitions
 - 'meta' bindings key to shift to another profile subset

Aside from text input fields and ship group selections in the Shipyard and Outfitter, most of the basic gameplay can easily
be covered by a gamepad.

I'm including an `.amgp` config file for AMX because i am familiar with its capabilities and how to configure it.  
The SteamInput (and possibly other input mappers as well) support a similar featureset. As long as a relatively small subset
of AMX features is used, it is likely possible to 'port' the config for usage with Steam (or others).  
But i rarely use Steam so i won't make a file for it, but contributions related to that are welcome!

## Key Mappings / Functions

| Controller Key | Click | Hold | Contexts |
| :-- | :-: | :-: | :-- |
| A | B | C | D |

## TODOs:
- [ ] Use glyphs/png icons instead of just patching in button names
- [ ] Make the selected glyph style configurable somehow, e.g. by a abusing a mission, outfit or conversation.
    (Alternatively try to contribute a way to ES in which plugins can hook into game prefs and provide custom keys)
- [ ] Change key tutorial popups to show button names instead of keys.
