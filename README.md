# CommandPrompt

A Roblox client-side command console built with Lua. CommandPrompt provides a draggable command interface, command history, player information lookup, camera controls, movement utilities, visual toggles, and server navigation.

> **Important:** This project uses executor-specific functions such as `loadstring`, `writefile`, `readfile`, `queue_on_teleport`, and `game:HttpGet`. It is not a normal Roblox Studio LocalScript and will not work in an ordinary Roblox client without the required environment. Use it only in experiences and environments where you have permission. Client-side changes generally do not replicate to the server or give you authority over other players.

## Features

- In-game command console with a scrollable output feed.
- Draggable, hideable, fullscreen-capable interface.
- Command aliases and argument validation.
- Named arguments and positional arguments.
- Command help, command details, and repeat-last-command support.
- Player account information retrieved through public Roblox endpoints.
- Character movement, camera, and visual utilities.
- Temporary jump platforms with the `PartJump` command.
- Rejoin, server hop, and place teleport commands.
- Optional script loading from a raw URL.

## Interface

- **Right Alt** — open the console.
- **Backquote (`)** — focus the command input.
- **-** — hide the console.
- **O** — toggle fullscreen.
- **X** — close and destroy the console.

The console also opens when the script starts. The interface is intended for desktop; the script explicitly exits on iOS and Android.

## Installation

1. Make sure you understand the code and trust the environment in which you plan to run it.
2. Use the script in a compatible client-side environment that supports the executor-specific functions it calls.
3. Run the script.
4. Press **Right Alt** to reopen the console if it is hidden.

This source is not packaged as a standard Roblox Studio installation. It creates its own `ScreenGui` and client script at runtime.

## Command usage

Enter a command in the input line and press **Enter**.

```text
command argument1 argument2
```

Arguments can also be assigned by name:

```text
command Name=value Toggle=true
```

The parser accepts these argument types:

| Type | Accepted input |
|---|---|
| `String` | Text |
| `Number` | A valid number |
| `Boolean` | `true` or `false` |
| `Player` | A player name, or an unambiguous beginning of a player name |

Commands and aliases are matched without regard to capitalization. Player arguments resolve against players currently in the server. The parser splits input on spaces, so string arguments cannot contain spaces.

For toggle commands, use `true` to enable and `false` to disable.

## Commands

The following command names and aliases are defined in the source.

| Command | Aliases | Arguments | Description |
|---|---|---|---|
| `Help` | `cmds`, `commands`, `h` | — | Displays available commands. |
| `GetCommandInfo` | `info`, `gci`, `command`, `commandinfo` | `Command` (String) | Displays a command's arguments, aliases, cleanup setting, and description. |
| `GetPlayerInfo` | `player`, `playerinformation`, `playerinfo`, `whois`, `who` | `OtherPlayer` (String) | Displays public account information for a Roblox username. |
| `Teleport` | `Tp`, `Goto`, `Moveto` | `OtherPlayer` (Player) | Moves your character to the specified player. |
| `RepeatCommand` | `redo`, `again`, `rep`, `twice`, `repeat` | — | Repeats the last command. |
| `Rejoin` | `rj` | — | Rejoins the current game server. |
| `ServerHop` | `hop`, `move`, `randomserver` | — | Searches for another available public server. |
| `JoinGame` | `place`, `joinplace`, `game` | `PlaceId` (Number) | Teleports to a game by PlaceId. |
| `Clear` | `cls`, `empty` | — | Clears command and output logs. |
| `Freecam` | `fc`, `freecamera` | `Amount` (Number), `Toggle` (Boolean) | Enables a free-roaming camera. |
| `View` | `Watch`, `Inspect` | `OtherPlayer` (Player), `Toggle` (Boolean) | Views another player's character and outputs inventory and Humanoid information. |
| `Follow` | `pointtowards`, `aim`, `point` | `OtherPlayer` (Player), `Toggle` (Boolean) | Turns your character toward another player. |
| `Fly` | `fly`, `flight` | `Amount` (Number), `Toggle` (Boolean) | Enables camera-relative character flight. |
| `Noclip` | `clip` | `Toggle` (Boolean) | Disables or restores character-part collisions. |
| `AntiVoid` | `antivoid`, `novoid` | `Toggle` (Boolean) | Creates an invisible platform above the experience's destroy height. |
| `PartWalk` | `partwalk`, `walkpart`, `platformwalk`, `float` | `Toggle` (Boolean), `Floored` (Boolean) | Creates a platform that follows beneath your character. |
| `PartJump` | `pj`, `jumpplatform` | `Toggle` (Boolean) | Spawns a temporary platform underneath your character when you jump. The platform is removed after 0.1 seconds. |
| `SetWalkspeed` | `speed`, `walkspeed`, `setspeed` | `Value` (Number), `Toggle` (Boolean) | Applies a chosen WalkSpeed value. |
| `SetJumppower` | `jumppower`, `setjumppower`, `jump` | `Value` (Number), `Toggle` (Boolean) | Applies a chosen JumpPower value. |
| `SetHipheight` | `hipheight`, `hip`, `sethipheight`, `height` | `Value` (Number), `Toggle` (Boolean) | Applies a chosen HipHeight value. |
| `Fullbright` | `bright`, `fb` | `Toggle` (Boolean) | Changes local lighting for a brighter view. |
| `XRay` | `transparency`, `see`, `wallhack` | `Toggle` (Boolean) | Increases transparency of workspace BaseParts. |
| `ESP` | `highlights` | `Toggle` (Boolean) | Highlights other players and displays their health and distance. |
| `RightClickTP` | `rctp`, `clicktp`, `clickteleport`, `rightclickteleport` | `Toggle` (Boolean) | Moves your character to the point selected with right-click. |
| `VFowardTeleport` | `vft`, `dash`, `fowardteleport`, `ft` | `Distance` (Number), `Toggle` (Boolean) | Enables a forward movement action on the **V** key. |
| `LoadScript` | `loadstring`, `import` | `Script` (String) | Fetches and executes a script from a raw URL. |
| `GetPlayerList` | `getplayers`, `players`, `playerlist` | - | Shows a list of every player in game. |

## Examples

```text
help
info Freecam
player Roblox
rejoin
serverhop
freecam 20 true
freecam 20 false
speed 32 true
speed 16 false
fly 25 true
fly 25 false
noclip true
noclip false
fullbright true
fullbright false
esp true
esp false
partjump true
partjump false
pj true
pj false
playerlist
```

Player-targeting examples:

```text
teleport Builderman
view Builderman true
view Builderman false
follow Builderman true
follow Builderman false
```

Use actual player names that are present in your current server for commands that require a `Player` argument.

## Player information lookup

`GetPlayerInfo` accepts a Roblox username and attempts to retrieve information using Roblox's public web APIs. The output can include:

- Username and display name
- User ID and profile URL
- Verified badge status
- Account creation date and calculated account age
- Public profile description
- Ban status, when the endpoint provides it
- Friend, follower, and following counts
- Previous usernames
- Avatar headshot URL
- Membership information when the target is in the current server

Some fields may show as unavailable if an endpoint fails, the information is not returned, or the target is not in the current server. Premium/Roblox Plus membership is only checked through the in-server `Player` object in this implementation; it is not reliably looked up for arbitrary offline users.

## Notes and limitations

- **Client-side behavior:** Movement and visual changes are performed locally. Whether they affect gameplay for other players depends on the experience and Roblox networking; this script does not grant server permissions.
- **Compatibility:** The script uses executor APIs that are not part of the standard Roblox Lua API. Availability and behavior vary by environment.
- **Studio:** Some commands explicitly reject Studio, and script loading is disabled there. Other executor-dependent operations may also fail.
- **Desktop only:** Mobile platforms are detected and unsupported.
- **Server hopping:** The command stores visited server IDs in `NotSameServers.json` and may fail if requests are restricted or the service is rate-limited.
- **Persistence and teleport queue:** The script attempts to save a copy as `CommandPrompt.lua` and queue it to run after teleporting. These behaviors require compatible file and queue APIs.
- **External scripts:** `LoadScript` executes code retrieved from a URL. Only use URLs whose source and contents you have personally reviewed. Remote code can change after you inspect it and can access whatever the execution environment permits.
- **Public APIs:** Roblox endpoints can change, return incomplete data, or impose request limits.

## Security warning

Do not paste unknown URLs into `LoadScript`, run code you have not reviewed, or provide account credentials to scripts or executors. A script can potentially misuse the permissions of the environment in which it runs. CommandPrompt does not need your Roblox password to retrieve the public account information described above.

## Version

The source currently sets the internal version attribute to **1.4**.
