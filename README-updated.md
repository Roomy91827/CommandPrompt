# CommandPrompt

CommandPrompt is a Roblox command-line-style interface for running commands related to players, character movement, the camera, workspace visuals, and client diagnostics.

**Current version:** 2.6  
**Author:** Roomy  
**License:** MIT

## Features

- Draggable command interface with animated opening and closing.
- Command input with aliases, argument validation, and command information.
- Builder mode for selecting commands and argument values through dropdowns.
- Separate output styles for information, success, and errors.
- Player lookup, player lists, and player information.
- Toggleable features with stored state and cleanup handlers.
- Platform detection for desktop, mobile, and console platforms.
- Version checking and duplicate-instance handling.
- Optional re-execution after teleporting when supported by the execution environment.
- Mobile on-screen controls for supported features.

## Repository files

| File | Purpose |
| --- | --- |
| `CommandPrompt` | Entry-point loader. Downloads and runs the GUI and client scripts. |
| `PowershellGUI` | Builds the CommandPrompt interface and its UI elements. |
| `Client` | Contains the command framework, command definitions, and behavior. |
| `Version` | Current release version (`2.6`). |
| `LICENSE` | MIT License. |

## Installation

The `CommandPrompt` loader fetches the other two Lua files from this repository:

```lua
local GUIFile = "https://raw.githubusercontent.com/Roomy91827/CommandPrompt/refs/heads/main/PowershellGUI"
local ClientFile = "https://raw.githubusercontent.com/Roomy91827/CommandPrompt/refs/heads/main/Client"

local GUI = loadstring(game:HttpGet(GUIFile))()
local Client = loadstring(game:HttpGet(ClientFile))()

Client(GUI)
```

Run the loader in an environment that supports the required functions. `game:HttpGet()` and `loadstring()` are not generally available to ordinary Roblox LocalScripts. The project has separate detection paths for executor, Studio, and regular client-script execution, but command availability is limited outside supported environments.

## Opening and controlling the interface

| Input / control | Action |
| --- | --- |
| `Right Alt` | Open the interface. |
| `D-Pad Up` | Open the interface on supported gamepad setups. |
| `F6` | Focus the command input while the interface is open. |
| `Button X` | Focus the command input on supported gamepad setups. |
| Hide button | Hide the interface. |
| Reopen button | Reopen the interface when it is hidden; shown on supported touch layouts. |
| Settings button | Open interface settings, including Builder mode. |

The interface also includes on-screen controls for supported mobile actions, including forward teleport and right-click/tap teleport when those features are enabled.

## Using commands

Type a command followed by its arguments, separated by spaces. Command names and aliases are case-insensitive.

Examples:

```text
Help
GetPlayerList
GetPlayerInfo PlayerName
GetCommandInfo Teleport
Teleport PlayerName
SetWalkspeed 32
Noclip true
Noclip false
AntiIdle true
AntiIdle false
Network
```

Use `Help` to list available commands. Use `GetCommandInfo <Command>` to view a command's description, aliases, and arguments.

Boolean arguments use `true` or `false`. Player arguments are resolved against players in the current server. Numeric arguments must be valid numbers. Some commands accept multiple arguments; enter them in the order shown in the command reference.

### Builder mode

Builder mode lets you assemble a command using dropdowns instead of entering the command and its arguments manually. Choose a command, select values for its arguments, and send it.

Builder mode can be changed in Settings. It is automatically enabled for detected mobile and console platforms.

## Command reference

The following commands are defined as public commands in the current `Client` script. Arguments are shown in their expected order.

| Command | Aliases | Arguments | Description |
| --- | --- | --- | --- |
| `LoopCommand` | `loop`, `commandloop` | `Toggle`, `Delay` | Repeatedly runs the last command at the specified delay. The code notes this may be buggy. |
| `Teleport` | `Tp`, `Goto`, `Moveto` | `OtherPlayer` | Teleports your character to the specified player. |
| `Hide` | `hidespot`, `flee`, `afkspot`, `afkroom`, `room` | `Toggle` | Teleports your character to a temporary safe space. |
| `PartJump` | `pj`, `jumpplatform`, `infjump`, `infinitejump` | `Toggle` | Creates a temporary platform underneath you when you jump. |
| `AntiIdle` | `antiafk`, `noafk`, `afkoff` | `Toggle` | Attempts to prevent idle disconnection. |
| `RepeatCommand` | `redo`, `again`, `rep`, `twice`, `repeat` | — | Repeats the last executed command. |
| `GetPlayerInfo` | `player`, `playerinformation`, `playerinfo`, `whois`, `who` | `OtherPlayer` | Displays information about a specified player. |
| `GetCommandInfo` | `info`, `gci`, `command`, `commandinfo` | `Command` | Displays information about a command. |
| `Rejoin` | `rj` | — | Attempts to rejoin the current game. |
| `VFowardTeleport` | `vft`, `dash`, `fowardteleport`, `ft` | `Distance`, `Toggle` | Enables forward teleporting with `V` or a supported gamepad input. |
| `RightClickTP` | `rctp`, `clicktp`, `clickteleport`, `rightclickteleport` | `Toggle` | Enables teleporting to the point selected with right-click or a supported gamepad input. |
| `XRay` | `transparency`, `see`, `wallhack` | `Toggle` | Applies transparency to workspace objects. |
| `ESP` | `highlights`, `locations`, `markers` | `Toggle` | Highlights other players and displays character information. |
| `Help` | `cmds`, `commands`, `h` | — | Lists available commands. |
| `Controls` | `getcontrols`, `binds`, `keys`, `keybinds` | — | Displays keybind information. |
| `ServerHop` | `hop`, `move`, `randomserver` | — | Attempts to join a different public server. The operation can fail. |
| `Clear` | `cls`, `empty` | — | Clears the command output. |
| `Freecam` | `fc`, `freecamera` | `Amount`, `Toggle` | Enables a free-roaming camera. |
| `View` | `Watch`, `Inspect` | `OtherPlayer`, `Toggle` | Views another player's character and displays information. |
| `AntiVoid` | `antivoid`, `novoid` | `Toggle` | Creates a part above the world's destroy-height. |
| `Noclip` | `clip` | `Toggle` | Disables character-part collisions. |
| `Orbit` | `circle`, `orb`, `round` | `Distance`, `Player`, `Speed`, `Toggle` | Orbits around a player using the given distance and speed. |
| `Credits` | `creds`, `cred`, `dev`, `cmdprompt`, `commandprompt`, `prompt` | — | Displays CommandPrompt information. |
| `SetWalkspeed` | `speed`, `walkspeed`, `setspeed` | `Value`, `Toggle` | Sets the player's WalkSpeed. |
| `Fly` | `fly`, `flight` | `Amount`, `Toggle` | Enables or disables flying. |
| `JoinGame` | `place`, `joinplace`, `game` | `PlaceId` | Attempts to join a game using its PlaceId. |
| `SetJumppower` | `jumppower`, `setjumppower`, `jump` | `Value`, `Toggle` | Sets the character's jump power. |
| `Follow` | `pointtowards`, `aim`, `point` | `OtherPlayer`, `Toggle` | Points the character toward another player. The code warns this can be buggy. |
| `PartWalk` | `partwalk`, `walkpart`, `platformwalk`, `float` | `Toggle`, `Floored` | Creates a platform that stays underneath your character. |
| `SetHipheight` | `hipheight`, `hip`, `sethipheight`, `height` | `Value`, `Toggle` | Sets the character's HipHeight. |
| `Fullbright` | `bright`, `fb` | `Toggle` | Changes lighting to brighten the workspace. |
| `LoadScript` | `loadstring`, `import` | `Script` | Downloads and runs Lua source from a raw file. Use only source you have reviewed and trust. |
| `Reload` | `re`, `restart`, `update` | — | Disconnects active features and reloads CommandPrompt. |
| `GetPlayerList` | `players`, `playerlist`, `getplayers` | — | Lists players in the current server. |
| `Network` | `ping`, `fps`, `lag` | — | Displays network, FPS, memory, and instance diagnostics. |

The script also contains hidden easter-egg commands. They are intentionally omitted from this public command reference.

## Execution environments and limitations

CommandPrompt checks whether it is running in an executor, Roblox Studio, or a regular Roblox client-script context.

- **Executor:** The script attempts to identify the executor and retrieve the latest version. Executor-specific features are used only when the required functions are available.
- **Roblox Studio:** The script reports that commands are limited.
- **Regular Roblox client:** Executor-only functionality is unavailable.

Some commands depend on APIs, permissions, or environment-specific functions. Network requests, teleport operations, virtual input, and client-side character changes may fail or behave differently between experiences and execution environments. Client-side changes are not guaranteed to be accepted or replicated by the game's server.

## Safety and responsible use

- `LoadScript` downloads and executes code. Only load scripts from sources you have personally reviewed and trust.
- Review the source code before running CommandPrompt or any scripts it loads.
- Character movement, position, collision, camera, and visibility commands may be restricted by an experience or may conflict with its rules.
- Use the project only in experiences and environments where you have permission to do so.

The project is provided as-is, without a guarantee that every command will work in every environment.

## Version

The current version is **2.6**, matching the `Version` file and the `CVersion` value in `Client`.

## License

This project is distributed under the [MIT License](LICENSE).
