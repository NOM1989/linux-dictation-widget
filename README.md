# Linux Dictation Widget (Speech to Text)
Offline dictation shortcut with control widget for Linux, made for Wayland (adaptable for X11 *- untested*). This project combines [nerd-dictation](https://github.com/ideasman42/nerd-dictation) and [LanguageTool](https://github.com/languagetool-org/languagetool) for local speech to text, and wraps control of it neatly into a custom control widget using [eww](https://github.com/elkowar/eww).
## Features
- Fast and (relatively) accurate speech to text entered into the current window
- Dictation toggle and microphone status indication
- Graceful one-click termination
## Setup
This project combines various tools into one to create a seamless dictation experience. This does mean that the following tools must be configured for it to work.
### LanguageTool Server
Follow their [installation guide](https://dev.languagetool.org/http-server.html) (fastText is recommended and covered).

Optionally, set `maxCheckThreads=2` in `server.properties` to limit resource usage.
### Nerd Dictation
Follow the [installation guidance](https://github.com/ideasman42/nerd-dictation) and configure a voice model of your choice.

Then use [this LanguageTool example](https://github.com/ideasman42/nerd-dictation/blob/aceb2bf650422c99d96c14b33c01b464b027aa92/examples/language_tool_auto_grammar/nerd-dictation.py) (from nerd-dictation) and change the language tool URL to `https://localhost:8081/…`. Optionally edit the `PUNCTUATION` and the `language` variables.
### eww
Follow the [installation guidance](https://elkowar.github.io/eww/). Then copy the `eww.yuck` and `eww.scss` from this repo into your eww config. Lastly, clone the `scripts` folder into your eww config directory and run `chmod +x /path/to/scrips/folder/*` to make them executable.
### (Optional) Font Awesome Icons
I chose to use icons provided by [Font Awesome](https://fontawesome.com/search?m=free&o=r), and these should be installed (via your package manager) if you want to use them too. The icons used in this widget are from the free section.
## Usage
Set a key bind to run the `launch-langserver` script (likely: `~/.config/eww/scripts/launch-langserver`). This will launch the language server for grammar correction in the background, as well as the dictation widget. Use `launch-langserver active` to have dictation automatically trigger when the key bind is first pressed. The key bind will only launch the language server if it is not currently running.

Clicking the mic icon on the widget will toggle the dictation on (red) or off.

Clicking the standby icon will:
1. End dictation if it is running
2. Terminate the language server
3. And close the widget

Warning - If the widget is opened directly with eww, e.g. `eww open dictation` then the local language server will not be launched and the dictation will fail. If the language server process is not found when pressing the standby icon, an error notification will be sent and the widget will not hide as an indication something has gone wrong. Check that the `nerd-dictation` or `languagetool-commandline.jar` processes are not lingering if this occurs.
## Non-Wayland
If you are using X11, you will have to edit the input method for nerd dictation by changing the command line arguments in the `toggle-dictation` script (--simulate-input-tool), see [here](https://github.com/ideasman42/nerd-dictation?tab=readme-ov-file#input-simulation-utilities) for more info.
## Troubleshooting
If you are having difficulties getting it working, please ensure each component, e.g. nerd dictation, is working individually by testing it following their installation. Additionally, use a process monitoring tool to ensure the `nerd-dictation` and `languagetool-commandline.jar` processes are actually being launched by the relevant scripts when dictation is active.

Removing some of the `> /dev/null` parts of the commands scattered about the scripts may help you find the problem via checking the `eww logs`.
