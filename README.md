<!-- omit in toc -->
# AmongUsOverlay: Among Us Dynamic Overlay for OBS

**AmongUsOverlay** is a fully customizable Among Us Dynamic Overlay for OBS.

Dynamically update the overlay for the OBS according to the Among Us status, i.e.:

- Automatically hide your Lobby Code only when you are in the Lobby.
- Display players and update its icons according to the liveness for each players
- Automatically display appropreate maps and switch the map version depends on the phase in the game (Tasks, Discussion, etc.)
- Show the match summary at the end of the game.
- Build your own overlay using simple HTML + CSS with predefined classes and IDs.

![demo](https://github.com/kurokobo/blob-storage/blob/main/amongus-overlay/merged.gif)

This repository includes some ready-to-use demos, but they can be easily customized and easily create your own overlay using HTML and CSS. If you have experience with JavaScript, now you can customize everything.

For example, if you want to display images only when you in the Lobby, all you need to write is the following one line of HTML!

```html
<img class="per_state only_lobby" src="<path/to/image>">
```

<!-- omit in toc -->
## Table of Contents

- [How This Works](#how-this-works)
- [Demos / Examples](#demos--examples)
- [Getting Started](#getting-started)
  - [Prepare AmongUsCapture](#prepare-amonguscapture)
  - [Prepare AmongUsOverlay](#prepare-amongusoverlay)
  - [Testing](#testing)
  - [Use in OBS](#use-in-obs)
  - [Reuse the Browser Sources you've created for the Next Time](#reuse-the-browser-sources-youve-created-for-the-next-time)
- [Tips](#tips)
- [Customization](#customization)
  - [Customize Demos](#customize-demos)
  - [Build Your Own Overlay](#build-your-own-overlay)
  - [Other Customization](#other-customization)
- [Related Projects](#related-projects)

## How This Works

The Browser source in OBS can overlay any web page.

This means that if we have a web page that automatically updates based on the state of the Among Us, we can overlay that as well.

**AmongUsOverlay** is an exactly tool that makes it easy to achieve hosting that web page.

Technically, it receives information about in-game state via WebSocket from a tool (called [AmongUsCapture](https://github.com/automuteus/amonguscapture)) that captures the game state and hosts the web page locally by [Caddy](https://caddyserver.com/), which is updated dynamically by JavaScript.

## Demos / Examples

See the example implementations and how they work:

- [▶️ Hide Lobby Code Automatically](example-auto-lobbycode-censor)
- [▶️ Display Players' Liveness Dynamically](example-auto-player-status)
- [▶️ Automated Map Switcher](example-auto-map-switcher)
- [▶️ Display Match Summary and Timelines when the Game is Over](example-auto-result-screen)

## Getting Started

### Prepare AmongUsCapture

Install and launch [AmongUsCapture](https://github.com/automuteus/amonguscapture). You got `[WAITING FOR AMONG US]` screen.

Next, click the gear icon on the upper-left corner and open `[Settings]` > `[General]` screen, then enable `[API Server]`.

### Prepare AmongUsOverlay

[Download the latest version of AmongUsOverlay via Release page](https://github.com/kurokobo/amongus-overlay/releases), then extract it and launch `AmongUsOverlay.bat`. Note that [Caddy](https://caddyserver.com/) (a lightweight HTTP server) will be downloaded automatically on the first startup.

![Starting Up](https://user-images.githubusercontent.com/2920259/118363954-95be4880-b5d1-11eb-928b-217822253bcd.png)

### Testing

For testing purpose, access the URL [`http://localhost:42080/example-debug/`](http://localhost:42080/example-debug/) in your browser and make sure you get a screen like the following.

![Testing](https://user-images.githubusercontent.com/2920259/118364037-cdc58b80-b5d1-11eb-951e-aff38d8c0ce2.png)

Now, launch Among Us and enter the FREEPLAY.

If everything works good, the page in your browser updated automatically like the following.

![Updated](https://user-images.githubusercontent.com/2920259/118364399-3c571900-b5d3-11eb-811a-7e6843df9fde.png)

### Use in OBS

Launch the OBS then add multiple **Browser** source with following URLs and size to see how preconfigured demos work. Nothing shows up until you start the game, which is expected.

| Demos                                                            | URL                                                     | Recommended Size                    |
| ---------------------------------------------------------------- | ------------------------------------------------------- | ----------------------------------- |
| [📁 Hide Lobby Code Automatically](example-auto-lobbycode-censor) | `http://localhost:42080/example-auto-lobbycode-censor/` | W `480`<br>H  `240`                 |
| [📁 Display Players' Liveness](example-auto-player-status)        | `http://localhost:42080/example-auto-player-status/`    | W `150 * <# of Players>`<br>H `240` |
| [📁 Map Switcher](example-auto-map-switcher)                      | `http://localhost:42080/example-auto-map-switcher/`     | W `1920`<br>H `1080`                |
| [📁 Result Screen](example-auto-result-screen)                    | `http://localhost:42080/example-auto-result-screen/`    | W `1280`<br>H `720`                 |

Now enter the Lobby and play the game! Your overlay will be updated dynamically.

### Reuse the Browser Sources you've created for the Next Time

After the initial setup and test in the previous steps, if you quit OBS and start it again, the overlay display may not be automatically updated properly.

Once this happened, you should open the `Properties` for every Browser souce on OBS and click `Refresh cache of current page`.

![Refresh cache of current page](https://user-images.githubusercontent.com/2920259/124773552-0487a480-df78-11eb-9791-ef19ba17f0fa.png)

This is because the Browser source of OBS does not necessarily load the page dynamically, but may just display the internally cached images.

Note that only the data received after clicking `Refresh cache of current page` will be used for the overlay display in OBS. For reliable tracking, it is recommended that you do this before launching Among Us (or at least before entering the lobby).

## Tips

- **AmongUsCapture** and **AmongUsOverlay** must both be running at all times.
- **The recommended startup order** is the following. If you start them in any other order, **the game status may not be tracked properly**.
  1. **AmongUsCapture**
  2. **AmongUsOverlay**
  3. **OBS**
     - Including adding Browser sources
     - Or to reuse exising Browser sources you've created before, invoke `Refresh cache of current page` as described above
  4. **Among Us**
- If the player status in the overlay goes wrong, re-enter the Lobby or use the PC on Lobby to change the player's color once and change it back.

## Customization

### Customize Demos

See `README.md` for each directory.

- [📁 **example-auto-lobbycode-censor**](example-auto-lobbycode-censor)
- [📁 **example-auto-player-status**](example-auto-player-status)
- [📁 **example-auto-map-switcher**](example-auto-map-switcher)
- [📁 **example-auto-result-screen**](example-auto-result-screen)

### Build Your Own Overlay

AmongUsOverlay has lots of useful features to build your own overlay.

See `README.md` in [📁 **template**](template) directory.

### Other Customization

- **Change the port that HTTP server listening**
  - Change `:42080` in `AmongUsOverlay.bat`.
- **Use AmongUsCapture running on a different PC**
  - Change `const ENDPOINT = "ws://localhost:42069/api"` in `common/js/const.js`.

## Related Projects

The following projects, products, and pages have been of great help to AmongUsOverlay.

I greatly appreciate for these professionals and expertise.

- **[Among Us](https://innersloth.com/gameAmongUs.php) by [InnerSloth](https://innersloth.com/)**
  - Thanks for the awesome game
  - They have all the rights to the images and trademarks
- **[AmongUsCapture](https://github.com/automuteus/amonguscapture)**
  - Thanks for awesome memory reader and wonderful APIs, and some graphics of the players
- **[Among Us Wiki](https://among-us.fandom.com/)**
  - Thanks for great information about this game, and some graphics of the maps
- **[Caddy](https://caddyserver.com/)**
  - Thanks for the great web server
