# CommandPrompt

A Roblox command-line-style GUI with a collection of player, character, camera, and utility commands.

**Current version:** 2.4  
**Author:** Roomy  
**License:** MIT

## Features

- A draggable command interface with open, hide, close, and fullscreen controls.
- Text-based command input with command aliases and argument validation.
- Builder mode with dropdowns for selecting commands and their arguments.
- Command output with separate information, success, and error colors.
- Player information and command help utilities.
- Toggleable features with stored state and cleanup functions.
- Platform detection for Windows, macOS, iOS, Android, UWP, Xbox, and PlayStation.
- Version checking and duplicate-instance handling.
- Optional re-execution after teleporting when the executor supports it.

## Repository files

| File | Purpose |
| --- | --- |
| `CommandPrompt` | Entry-point loader. Downloads and initializes the GUI and client code. |
| `PowershellGUI` | Creates the CommandPrompt interface and its UI elements. |
| `Client` | Contains the command framework, command definitions, and behavior. |
| `Version` | Contains the current release version (`2.4`). |
| `LICENSE` | MIT License. |

## Installation and execution

The repository's `CommandPrompt` entry-point file downloads the other two Lua files from this repository and initializes them.

The loader uses `game:HttpGet()` and `loadstring()`. These are not available to ordinary Roblox LocalScripts in the same way they are in supported executor environments. The project detects whether it is running in an executor, Roblox Studio, or a regular LocalScript, and some features are limited outside an executor.

Only run scripts in environments and experiences where you have permission to do so. Do not run modified copies from sources you do not trust.

## Opening the interface

The client script defines these controls:

| Input | Action |
| --- | --- |
| `Right Alt` | Open the interface |
| `D-Pad Up` | Open the interface on supported gamepad setups |
| `F6` | Focus the command input while the interface is open |
| `Button X` | Focus the command input on supported gamepad setups |
| `O` | Toggle fullscreen |
| `X` | Close and remove the interface |
| `-` | Hide the interface |

On touch devices, the interface also provides an on-screen reopen button when appropriate. Some commands provide their own on-screen controls.

## Using commands

Enter a command followed by its arguments, separated by spaces.

Examples:

```text
Help
GetPlayerList
GetCommandInfo Teleport
Teleport PlayerName
SetWalkspeed 32
Noclip true
Noclip false
AntiIdle true
AntiIdle false
```

Command names and aliases are case-insensitive. The command parser checks the number and type of arguments before calling a command. Arguments can also be supplied by name using `Name=Value` syntax.

Use `Help` to display the available commands and `GetCommandInfo <Command>` to view information about a specific command.

### Builder mode

Builder mode lets you construct a command using dropdowns instead of typing it manually. Select a command, choose values for its arguments, and press the send button to execute it. The available dropdown values depend on the argument type and the command definition.

Builder mode is automatically enabled on detected mobile and console platforms. It can also be changed in Settings.

## Command reference

The following commands are defined in the current `Client` file. Some commands may behave differently or be unavailable depending on the game, platform, execution environment, and executor capabilities.

| Command | Arguments | Description |
| --- | --- | --- |
| `Teleport` (`Tp`, `Goto`, `Moveto`) | `OtherPlayer` | Teleports your character to the specified player. |
| `Hide` (`hidespot`, `flee`, `afkspot`, `afkroom`, `room`) | `Toggle` | Creates a temporary remote safe area and moves your character there. |
| `PartJump` (`pj`, `jumpplatform`, `infjump`, `infinitejump`) | `Toggle` | Creates a temporary platform underneath you when you jump. |
| `AntiIdle` (`antiafk`, `noafk`, `afkoff`) | `Toggle` | Connects to `Player.Idled` and attempts to send virtual input. |
| `RepeatCommand` (`redo`, `again`, `rep`, `twice`, `repeat`) | — | Repeats the last executed command. |
| `GetPlayerInfo` (`player`, `playerinformation`, `playerinfo`, `whois`, `who`) | `OtherPlayer` | Displays information about a specified player. |
| `GetCommandInfo` (`info`, `gci`, `command`, `commandinfo`) | `Command` | Displays information about a command. |
| `Rejoin` (`rj`) | — | Attempts to rejoin the current game. |
| `VFowardTeleport` (`vft`, `dash`, `fowardteleport`, `ft`) | `Distance`, `Toggle` | Enables forward teleporting with `V` or a supported gamepad input. |
| `RightClickTP` (`rctp`, `clicktp`, `clickteleport`, `rightclickteleport`) | `Toggle` | Enables teleporting to the location you point at or tap. |
| `XRay` (`transparency`, `see`, `wallhack`) | `Toggle` | Increases the transparency of workspace parts. |
| `ESP` (`highlights`, `locations`, `markers`) | `Toggle` | Highlights other players and displays character information. |
| `Help` (`cmds`, `commands`, `h`) | — | Displays available commands. |
| `Controls` (`getcontrols`, `binds`, `keys`, `keybinds`) | — | Displays keybind information. |
| `ServerHop` (`hop`, `move`, `randomserver`) | — | Attempts to join a different public server. |
| `Clear` (`cls`, `empty`) | — | Clears the command output. |
| `Freecam` (`fc`, `freecamera`) | `Toggle` | Enables a free-roaming camera. |
| `View` (`Watch`, `Inspect`) | `OtherPlayer` | Views another player's character and displays information. |
| `AntiVoid` (`antivoid`, `novoid`) | `Toggle` | Creates a platform above the world's destroy-height. |
| `Noclip` (`clip`) | `Toggle` | Toggles character-part collisions. |
| `Orbit` (`circle`, `orb`, `round`) | `OtherPlayer`, `Distance`, `Toggle` | Orbits around another player at the specified distance. |
| `Credits` (`creds`, `cred`, `dev`, `cmdprompt`, `commandprompt`, `prompt`) | — | Displays CommandPrompt information. |
| `SetWalkspeed` (`speed`, `walkspeed`, `setspeed`) | `Amount` | Sets the local character's WalkSpeed. |
| `Fly` (`fly`, `flight`) | `Toggle`, `Amount` | Toggles flying with a specified speed. |
| `JoinGame` (`place`, `joinplace`, `game`) | `PlaceId` | Attempts to join a game using its PlaceId. |
| `SetJumppower` (`jumppower`, `setjumppower`, `jump`) | `Amount` | Sets the character's jump power. |
| `Follow` (`pointtowards`, `aim`, `point`) | `OtherPlayer` | Points the character toward another player. |
| `PartWalk` (`partwalk`, `walkpart`, `platformwalk`, `float`) | `Toggle` | Creates a platform that stays underneath your character. |
| `SetHipheight` (`hipheight`, `hip`, `sethipheight`, `height`) | `Amount` | Sets the character's HipHeight. |
| `Fullbright` (`bright`, `fb`) | `Toggle` | Changes lighting settings to brighten the workspace. |
| `LoadScript` (`loadstring`, `import`) | `Script` | Downloads and executes Lua source from a URL. |
| `Reload` (`re`, `restart`, `update`) | — | Disconnects active command features and reloads CommandPrompt. |
| `GetPlayerList` (`players`, `playerlist`, `getplayers`) | — | Lists players in the current server. |

`Toggle` arguments accept `true` or `false`. Player arguments are resolved against players in the current server. Numeric arguments must be valid numbers.

## Execution environments and limitations

CommandPrompt detects three execution types:

- **Executor:** Enables executor-specific functionality when the required functions are available. The script also attempts to identify the executor, save a copy of its loader, and queue re-execution after teleporting.
- **Roblox Studio:** Uses the Studio detection path and reports that commands are limited.
- **Regular LocalScript:** Runs in a standard client-script context and reports that executor-specific commands are unavailable.

Some commands rely on capabilities or APIs that may not be available in every environment. For example, `LoadScript` is explicitly restricted to executor mode in the code. Network requests, teleport operations, virtual input, and other client-side behavior can also fail depending on the environment or Roblox restrictions.

This project does not guarantee that client-side character changes will be accepted or replicated by a game's server.

## Safety notes

- `LoadScript` executes code downloaded from the supplied URL. Only use URLs whose source you have reviewed and trust.
- Commands that alter character movement, position, collision, camera, or visibility may not work in every experience and may conflict with that experience's rules.
- Do not use the project to disrupt other players or bypass rules in experiences where these actions are prohibited.
- The project is provided as-is. Review the code before running it.

## Version

The current repository version is **2.4**, as recorded in `Version` and the client script.

## License

This project is distributed under the [MIT License](LICENSE).
