<p align="center">
  <img src="https://i.imgur.com/IkC5S0L.png"><br/>
  <b>wind.lua</b>: Modern polyfills for execution platforms.
</p>

***

* 🚀 **Empower your execution platform with the latest APIs.**  
`wind.lua` injects any missing functions into your environment through isomorphic [polyfills](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill).
* ☄️ **Fix countless incompatibilities between environments.**  
`wind.lua` automagically fixes API incompatiblities between executor environments _without_ user or developer configuration.
* 💱 **Emulate foreign environments for development purposes.**  
`wind.lua` can emulate foreign execution environments if you are developing a script targeting multiple products.
* 🎲 **Inject intelligent stubs for unimplemented native functionality.**  
`wind.lua` will inject intelligent, non-breaking stubs for the native functionality that it cannot fully reimplement within Lua.
* 📑 **Provide sensible definitions for scripts built on top of the library.**  
`wind.lua` can also be used as a library providing common definitions to other Lua scripts, avoiding environment pollution.

***
## Getting started
### As a user
To add `wind.lua` into your execution platform, simply download the latest version of `wind.autoexec.min.lua` from our [Release](https://github.com/ccreaper/wind/releases) page and insert it into your platform's `autoexec` folder or equivalent. **Then,** move _any_ scripts you want to automatically run with `wind.lua`'s fixes into the `wind-autoexec` folder in `autoexec`, creating it if it doesn't exist. Scripts ran manually through the code editor or through a file load will already have the definitions imported into their environment.

### As a developer
As a developer, you have three ways of using `wind.lua`: either as an incompatibility-resolving monkey patch, a custom definition provider, or an emulator for foreign execution platforms.
* **To use `wind.lua` as a monkey patch,** simply require it at the top of your script as such:  
```lua
-- A bundler may be necessary depending on your platform.
require "wind" {
  environment = true
}

-- Any code ran after the require can now use common aliases.
print(identifyexecutor())
```
* **To use `wind.lua` as a custom definition provider,** require the library and use functions from its returned table.  
```lua
local wind = require("wind")

-- Consult our documentation for these definitions.
print(wind.identify())
```
* **To use `wind.lua` as an environment emulator,** require the library while passing the `target` property specifying the product you want to emulate. The library will then return a function that accepts a callback to emulate. The following targets are supported: `synapse-x`, `script-ware`, `krnl`, `fluxus`, `celery`.
```lua
require "wind" {
  target = "synapse-x"
} (function()
  -- Emulated code goes here.
end)
```
## Options
```lua
{
  -- If true, then all wind definitions will be imported in the requiree's environment.
  environment = false;
  
  -- If specified, then the require will return a callback accepting a function to emulate. See above for accepted values.
  target = nil;
  
  -- Potentially great patches for certain cases.
  magicPatches = {
    -- If true, then the library will automatically disable many of the functions and variables
    -- in the environment that can be used for telemetric purposes and establishing a fingerprint.
    -- The script will not be able to figure out fingerprint resistence has been enabled.
    privacy_resistFingerprinting = true;
    
    -- If true, then scripts by default will not be able to make any network requests without
    -- your consent. All URLs will have to be manually reviewed through a dialog.
    privacy_blockNetworking = false;
  
    -- If true, then scripts retrieved through a network request will be cached to the filesystem.
    -- You can configure the TTL through the cacheTTL property (in hours).
    cacheFetches = false;
    cacheTTL = 12;
    
    -- If true, then common security practices will be automatically applied to the environment,
    -- and unsafe behavior will error when attempted. This shouldn't interfere with most scripts.
    injectCommonSecurityPatches = false;
    
    -- If true, then commonly blacklisted functions and methods imported from the game's environment
    -- will be replaced with non-breaking, non-erroring stubs. Currently, some execution platforms simply
    -- crash your process instead of blocking the behavior altogether - this option fixes that.
    injectStubsIntoBlacklistedFunctions = false;
    
    -- If true, then an alternative, more aggressive (but potentially more unstable) method will
    -- be used for injecting polyfills and stubs, which may work better for obfuscated scripts,
    -- or scripts running behind an encryption layer such as SecureLua. Try the script without
    -- this option enabled first - if it fails, then try it with this on.
    alternativeInjection = false;
  };
}
```

## Notes
- **`wind.lua` is not a cure-all and cannot reimplement all missing native functionality.** While it emulates _some_ native behavior (such as the `Drawing` library in platforms that do not implement it), any functionality requiring native solutions will receive stub injections instead of polyfills. This means that while a lack of native implementations will _not_ break scripts, it may cause unexpected behavior, especially if the functionality is related to the filesystem.
- **`wind.lua`'s injections are slower than their native equivalents.** If the library cannot reuse an existing alias, it will attempt to reimplement the functionality of the API through an environment injection. The injection will have behavior isomorphic to the specification, but will suffer from degraded behavior due to it being implemented in Lua.
- **`wind.lua` can cause environment confusion.** Certain scripts rely on the availability of certain unique variables in order to guess the identity of its environment. However, the library's environment injections may interfere in the accuracy of this process. If this breaks your script in some way or another, simply run it in an emulated environment as described above.
- **`wind.lua`'s optional magic patches can potentially break the functionality of some scripts.** They are principally geared towards developers consuming the library, not random scripts!

## Contributing
**We are always ready to add any commonly-used APIs to our list.** If any APIs are missing, create a [pull request](https://github.com/ccreaper/wind/pulls) and suggest changes!
