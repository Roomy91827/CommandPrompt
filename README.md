# CommandPrompt

**CommandPrompt** is a client-side command console for Roblox. It provides a draggable in-game interface, command parsing, player lookup, character and camera utilities, visual toggles, and server navigation.

> [!WARNING]
> CommandPrompt is designed for executor environments, not as a normal Roblox Studio LocalScript. The loader uses executor-specific capabilities such as `loadstring`, `game:HttpGet`, `writefile`, `readfile`, and `queue_on_teleport`. Availability and behavior depend on the environment.

## Contents

- [Files and loading](#files-and-loading)
- [Features](#features)
- [Interface and controls](#interface-and-controls)
- [Command syntax](#command-syntax)
- [Commands](#commands)
- [Command details](#command-details)
- [Compatibility and limitations](#compatibility-and-limitations)
- [Security](#security)
- [Version](#version)

## Files and loading

The project is split into three runtime files:

| File | Purpose |
| --- | --- |
| `CommandPrompt` | Main entry point. Downloads and executes `GUI` and `Client`. |
| `GUI` | Creates the `CommandPromptGUI` interface and returns the GUI instance. |
| `Client` | Returns an initializer function. The loader passes the generated GUI to this function, which connects the interface and initializes the commands. |
| `Version` | Contains the latest published version string used by the version check. |

The normal entry point is `CommandPrompt`. Its current loader follows this sequence:

1. Downloads and executes `GUI`.
2. Downloads and executes `Client`.
3. Calls the returned client initializer with the GUI instance.

The `Client` file is a module-style chunk: it returns a function that expects the generated `CommandPromptGUI`. Running it by itself without supplying that GUI will fail because it waits for GUI objects such as `MainFrame`, `MobileButtonsFrame`, and the command input controls.

If you are loading the pieces individually, preserve that order and pass the GUI returned by `GUI` into the initializer returned by `Client`.

The source URLs currently point to the repository's `main` branch:

- `https://raw.githubusercontent.com/Roomy91827/CommandPrompt/refs/heads/main/CommandPrompt`
- `https://raw.githubusercontent.com/Roomy91827/CommandPrompt/refs/heads/main/GUI`
- `https://raw.githubusercontent.com/Roomy91827/CommandPrompt/refs/heads/main/Client`
- `https://raw.githubusercontent.com/Roomy91827/CommandPrompt/refs/heads/main/Version`

## Features

- In-game command console with a scrolling output feed.
- Draggable window, fullscreen mode, hide, close, and reopen controls.
- Command aliases and typed argument validation.
- Positional and named arguments.
- Player arguments resolved against players in the current server.
- Command descriptions, aliases, and argument information.
- Command history output and repeat-last-command support.
- Conflict handling through command override relationships.
- Per-command stored state and disconnect functions for active features.
- Optional cleanup of commands when the character dies, according to each command's `CleanupOnDeath` setting.
- Player account information retrieved through public Roblox endpoints.
- Character movement, camera, and visual utilities.
- Rejoin, server-hop, and place-joining commands.
- Optional execution of a script from a raw URL.
- Mobile buttons for supported touch controls.

## Interface and controls

The console opens automatically when initialized.

| Input / control | Action |
| --- | --- |
| `Right Alt` | Open the console on keyboard devices. |
| `D-Pad Up` | Open the console on supported gamepads. |
| `F6` | Focus the command input while the console is open on keyboard devices. |
| `Button X` | Focus the command input while the console is open on supported gamepads. |
| `V` | Trigger `VFowardTeleport` when enabled on keyboard devices. |
| `Button L3` | Trigger `VFowardTeleport` on supported gamepads. |
| Right mouse button | Trigger `RightClickTP` on mouse devices. |
| `Button L2` | Aim-based `RightClickTP` on supported gamepads. |
| `-` button | Hide the console. |
| `O` button | Toggle fullscreen. |
| `X` button | Close and destroy the console. |

The title bar can be dragged with the mouse or touch. Fullscreen mode disables dragging until fullscreen is turned off.

On touch devices, the interface provides:
- An `ALT` button to reopen a hidden console.
- A `V` button while `VFowardTeleport` is enabled.
- An `RC` toggle button while `RightClickTP` is enabled. When armed, tapping a point in the world attempts to teleport the character there.

Use `controls` in the console to display the controls detected for the current platform.

## Command syntax

Enter a command in the input line and press **Enter**.

### Positional arguments

Arguments are entered in the order shown by the command:

```text
command argument1 argument2
```

For example:

```text
speed 32 true
orbit 10 Builderman 2 true
```

### Named arguments

Arguments can also be assigned by their declared names:

```text
command Name=value Toggle=true
```

For example:

```text
orbit Distance=10 Player=Builderman Speed=2 Toggle=true
```

All required arguments must be supplied. Named argument names must match the names declared by the command.

### Argument types

| Type | Accepted input |
| --- | --- |
| `String` | A text token. |
| `Number` | A value that `tonumber` can parse. |
| `Boolean` | `true` or `false`. |
| `Player` | A player in the current server, resolved by username or display name. |

Command names and aliases are case-insensitive. Player lookup checks an exact username first, then an exact display name, then a unique username/display-name prefix. If a prefix matches multiple players, use a more specific name.

**Important parsing limitation:** input is split on spaces. A single string argument therefore cannot contain spaces. For example, a URL or script argument must be supplied as one token.

For toggle commands, use `true` to enable and `false` to disable.

## Commands

The following list is taken from the `CommandData` table in the current `Client` source. Argument order matters when using positional arguments.

| Command | Aliases | Arguments | Description |
| --- | --- | --- | --- |
| `Teleport` | `Tp`, `Goto`, `Moveto` | `OtherPlayer` (`Player`) | Teleports you to the specified player. |
| `PartJump` | `pj`, `jumpplatform`, `infjump`, `infinitejump` | `Toggle` (`Boolean`) | Spawns a temporary platform underneath you whenever you jump. |
| `RepeatCommand` | `redo`, `again`, `rep`, `twice`, `repeat` | — | Repeats the last command executed. |
| `GetPlayerInfo` | `player`, `playerinformation`, `playerinfo`, `whois`, `who` | `OtherPlayer` (`String`) | Outputs information for the specified Roblox username. |
| `GetCommandInfo` | `info`, `gci`, `command`, `commandinfo` | `Command` (`String`) | Displays information about a command. |
| `Rejoin` | `rj` | — | Rejoins the current game. |
| `VFowardTeleport` | `vft`, `dash`, `fowardteleport`, `ft` | `Distance` (`Number`), `Toggle` (`Boolean`) | Teleports you forward by the specified distance when the V control is pressed. |
| `RightClickTP` | `rctp`, `clicktp`, `clickteleport`, `rightclickteleport` | `Toggle` (`Boolean`) | Teleports you to the point selected with the relevant mouse, gamepad, or touch input. |
| `XRay` | `transparency`, `see`, `wallhack` | `Toggle` (`Boolean`) | Applies transparency to BaseParts in the workspace. |
| `ESP` | `highlights`, `locations`, `markers` | `Toggle` (`Boolean`) | Highlights other players and displays character information. |
| `Help` | `cmds`, `commands`, `h` | — | Displays a list of available commands. |
| `Controls` | `getcontrols`, `binds`, `keys`, `keybinds` | — | Displays the detected platform controls. |
| `ServerHop` | `hop`, `move`, `randomserver` | — | Teleports you to a random server. |
| `Clear` | `cls`, `empty` | — | Clears the console output and command log. |
| `Freecam` | `fc`, `freecamera` | `Amount` (`Number`), `Toggle` (`Boolean`) | Lets you spectate the game from a free-roaming camera. |
| `View` | `Watch`, `Inspect` | `OtherPlayer` (`Player`), `Toggle` (`Boolean`) | Views another player's character and outputs their information. |
| `AntiVoid` | `antivoid`, `novoid` | `Toggle` (`Boolean`) | Creates a part above the experience's destroy height. |
| `Noclip` | `clip` | `Toggle` (`Boolean`) | Disables collisions with other parts for your character. |
| `Orbit` | `circle`, `orb`, `round` | `Distance` (`Number`), `Player` (`Player`), `Speed` (`Number`), `Toggle` (`Boolean`) | Orbits a player at the given distance and speed. |
| `Credits` | `creds`, `cred`, `dev`, `cmdprompt`, `commandprompt`, `prompt` | — | Displays CommandPrompt information and credits. |
| `SetWalkspeed` | `speed`, `walkspeed`, `setspeed` | `Value` (`Number`), `Toggle` (`Boolean`) | Sets your character's WalkSpeed. |
| `Fly` | `fly`, `flight` | `Amount` (`Number`), `Toggle` (`Boolean`) | Allows your character to fly. |
| `JoinGame` | `place`, `joinplace`, `game` | `PlaceId` (`Number`) | Joins a game using its PlaceId. |
| `SetJumppower` | `jumppower`, `setjumppower`, `jump` | `Value` (`Number`), `Toggle` (`Boolean`) | Sets your character's JumpPower. |
| `Follow` | `pointtowards`, `aim`, `point` | `OtherPlayer` (`Player`), `Toggle` (`Boolean`) | Points your character toward another player. |
| `PartWalk` | `partwalk`, `walkpart`, `platformwalk`, `float` | `Toggle` (`Boolean`), `Floored` (`Boolean`) | Creates a part that follows beneath your character. |
| `SetHipheight` | `hipheight`, `hip`, `sethipheight`, `height` | `Value` (`Number`), `Toggle` (`Boolean`) | Sets your character's HipHeight. |
| `Fullbright` | `bright`, `fb` | `Toggle` (`Boolean`) | Changes local lighting for a brighter view. |
| `LoadScript` | `loadstring`, `import` | `Script` (`String`) | Fetches and executes a script from a raw URL. |
| `Reload` | `re`, `restart`, `update` | — | Disconnects active command data and reloads CommandPrompt. |
| `GetPlayerList` | `players`, `playerlist`, `getplayers` | — | Displays the players currently in the server. |

## Command details

### Examples

Basic commands:

```text
help
controls
info Freecam
player Roblox
players
credits
clear
rejoin
serverhop
```

Movement and toggle examples:

```text
teleport Builderman
speed 32 true
speed 16 false
jumppower 75 true
jumppower 50 false
sethipheight 5 true
sethipheight 2 false
fly 25 true
fly 25 false
freecam 20 true
freecam 20 false
noclip true
noclip false
antivoid true
antivoid false
partjump true
partjump false
partwalk true false
partwalk false false
fullbright true
fullbright false
xray true
xray false
esp true
esp false
```

Player-targeting examples:

```text
teleport Builderman
view Builderman true
view Builderman false
follow Builderman true
follow Builderman false
orbit 10 Builderman 2 true
orbit 10 Builderman 2 false
```

Other examples:

```text
vft 25 true
vft 25 false
rctp true
rctp false
joingame 123456789
getcommandinfo Orbit
repeatcommand
```

Replace example player names and PlaceIds with valid values for your session. Commands that accept a `Player` argument require that player to be in the current server.

### Command information

`GetCommandInfo` accepts a command name or alias. It reports the command's declared arguments, aliases, `CleanupOnDeath` setting, and description.

### Player information

`GetPlayerInfo` accepts a Roblox username and attempts to retrieve information from public Roblox endpoints. Depending on endpoint availability, returned information can include:

- Username, display name, user ID, and profile URL.
- Verified badge status.
- Account creation date and calculated account age.
- Public profile description.
- Ban status, when returned by the endpoint.
- Friend, follower, and following counts.
- Previous usernames.
- Avatar headshot URL.
- Membership information when the target is in the current server.

Some fields may be unavailable if an endpoint fails, omits the information, or limits requests. Membership information is based on the in-server `Player` object and is not a reliable lookup for arbitrary offline users.

### Command overrides

Some active commands are mutually incompatible. When a command with a `Toggle` argument is enabled, CommandPrompt checks its declared override relationships and disables conflicting commands if their stored connection is active.

The current override relationships are:

- `Freecam` conflicts with `Orbit`, `Fly`, `Follow`, `View`, `SetWalkspeed`, and `SetJumppower`.
- `View` conflicts with `Freecam`.
- `Orbit` conflicts with `Fly`, `Follow`, and `Freecam`.
- `Fly` conflicts with `Orbit`, `Follow`, and `Freecam`.
- `SetWalkspeed` and `SetJumppower` each conflict with `Freecam`.
- `Follow` conflicts with `Orbit`, `Fly`, and `Freecam`.

These relationships are defined by the `Overrides` fields in `CommandData`.

## Compatibility and limitations

- **Executor environment:** The project calls APIs that are not part of the standard Roblox Lua API. An environment may not support every function the scripts use.
- **Roblox Studio:** Studio is detected and displays a warning. Some functionality, including `LoadScript`, is explicitly unavailable in Studio. Other executor-dependent operations may fail.
- **Client-side behavior:** Character movement and visual changes are initiated by the local client. This does not grant server permissions or guarantee that changes replicate to other players.
- **Platform support:** The source includes keyboard, gamepad, and touch input paths. The exact controls and available behavior depend on the device and input APIs exposed by the environment.
- **Server hopping:** Server discovery can fail because of request restrictions, unavailable endpoints, or rate limits. The implementation also tracks visited server IDs in `NotSameServers.json` when file APIs are available.
- **Teleport re-execution:** The client attempts to save the loader as `CommandPrompt.lua` and queue it to run after teleporting. This requires compatible file and teleport-queue APIs.
- **Public Roblox endpoints:** Account information lookups depend on public endpoints that may change, return incomplete data, or apply request limits.
- **Game-specific behavior:** Individual experiences may restrict or respond differently to client-side movement, camera, and visual changes.

## Security

`LoadScript` downloads source code from a raw URL and executes it using `loadstring`. Only use scripts from sources you trust and whose contents you have reviewed. Remote code can change after you inspect it and may access the capabilities of the environment in which it runs.

Do not paste unknown URLs into `LoadScript`, run unreviewed code, or provide account credentials to scripts or executors. CommandPrompt does not need your Roblox password to perform the public account lookups described above.

## Version

The current source version is **1.8**. The `Client` script sets the GUI's `Version` attribute to `1.8`, and checks the repository's `Version` file for a newer published version outside Studio.

## License

This project is distributed under the [MIT License](LICENSE).
