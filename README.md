- [Demo](#demo)
  - [Demo Explanation](#demo-explanation)
- [What is this?](#what-is-this)
- [Why?](#why)
- [What can I use this for?](#what-can-i-use-this-for)
- [Who is this for?](#who-is-this-for)
- [Related apps](#related-apps)
- [Dependencies](#dependencies)
- [Installation](#installation)
- [Running and Usage](#running-and-usage)
- [FAQ](#faq)
  - [Why implement core features in plugins?](#why-implement-core-features-in-plugins)
  - [What is this built with?](#what-is-this-built-with)
  - [Why does the screen freeze on paste for Gnome on X11?](#why-does-the-screen-freeze-on-paste-for-gnome-on-x11)
- [TODO](#todo)
- [Possible features](#possible-features)
- [App Behaviors](#app-behaviors)
- [Attribution](#attribution)

# Demo
https://github.com/user-attachments/assets/1bc29a26-ea55-49ec-833d-6c52ea5bc840

## Demo Explanation
The contrived scenario in the demo shows a handful of the features of Epte:
- Bookmarks search with prefix "bm " in the default search view (to get to Gmail)
- Clipboard history
- Google autocomplete search
- Result Actions on clipboard history (open PGN with Lichess)
- Launcher (open iTerm2)
- Bookmarks search to get to ChatGPT
- Command output history with "zo " to get output from recent shell command
	- This is enabled by adding zsh hooks that use Tmux's `pipe-pane` to capture command output to files in a history directory.
	- TODO elaborate
- Result Actions (to wrap the command output in backticks)
- Launcher (open iTerm2 again)
- Gmail search with "gm "
	- not enabled currently in the release packages


# What is this?
Epte (Entry Point to Everything) is a cross-platform (Windows TODO) launcher and clipboard manager similar in principal to apps like Alfred, Raycast, Albert, or Cerebro
For those unfamiliar with the terms:
- A clipboard manager is a program which collects a history of the content you've copied to your clipboard and provides a way of searching through that history and pasting from it.
- A launcher is an application which launches other applications. Eg. Spotlight on MacOS.
# Why?
There is an abundance of launcher apps already, especially for MacOS (see the [Related apps](#related%20apps) section), so why make yet another one?
- I use MacOS and Ubuntu for work and wanted to be able to have a consistent experience with my workflows across both.
- I want to be able to create and share workflows that can be used by those using MacOS, Ubuntu, and Windows
- I wanted to build something in Flutter
- Existing launchers are either platform specific or built with Electron. The resource usage for Electron apps is much higher than should be required for an app that is meant to left running constantly in the background.

# What can I use this for?
- Clipboard history - one of the main usages of the app is just to keep an easily viewable and searchable history of text and images copied to the clipboard.
- Bookmarks search with prefix "bm "
- Zsh or bash history search with "z "
- Google autocomplete searches with "g "
- Custom operations on clipboard contents (ala Alfred universal actions) - the Python plugin system can be used to define custom actions to take on clipboard entries. Some of the uses I've found for it: (1) find, parse, and prettify json in selected text, (2) open a search on Datadog/Sumologic for the selected text, (3) open entry in an editor to make quick changes, (4) wrap entry in triple backticks to easy paste as a code block, ...
# Who is this for?
- The clipboard history and some built-in functionality like bookmarks search are generally useful for anyone. There are probably better options on each platform for these features though if that's all that's needed
- This is currently targeted at programmers who
	- plan on adding their own plugins to improve their workflows
	- want those workflows to work across the platforms they use
	- want to share plugins with teammates using other platforms

# Related apps
- [Alfred](https://www.alfredapp.com/)
- [Raycast](https://www.raycast.com/)
- [Cerebro](https://github.com/cerebroapp/cerebro)
- [Albert](https://github.com/albertlauncher/albert)
- [Arvis](https://github.com/jopemachine-arvis/arvis)
- [LaunchMenu](https://github.com/LaunchMenu/LaunchMenu)
- [Rofi](https://github.com/davatorium/rofi)
- [Dmenu](https://tools.suckless.org/dmenu/)
- [ULauncher](https://github.com/Ulauncher/Ulauncher/)
- [Quicksilver](https://qsapp.com/index.php)
- [Ueli](https://github.com/oliverschwendener/ueli)
# Dependencies
- Linux (only tested on Ubuntu)
	- X11 (Wayland not supported yet)
	- these are currently bundled with the app:
		- xclip for correct pasting
			- otherwise text entries have an image mime type for some reason with the go clipboard package used
		- wmctrl
			- enables setting the flutter window order
		- from https://github.com/golang-design/clipboard
			- "Linux: require X11 dev package. For instance, install libx11-dev or xorg-dev or libX11-devel to access X window system."
		- xdotool
			- to trigger ctrl+v
- Windows (TODO)
	- c++ redistribution dlls
- MacOS
	- permissions to various folders and accessibility permissions to trigger cmd + v
# Installation
There are no installers currently, just download one of the bundles from the releases, unzip it, and run

# Running and Usage
- NOTE: the app may crash if it can't bind the global system shortcuts which are ctrl + shift + V and ctrl + shift + space by default. These can be changed in the config.toml in the app data directory
- ctrl + shift + v to open the clipboard history
- ctrl + shift + space to bring up the default search box
- right arrow on an entry to get to Result Actions
- Vim keys can be used to navigate entries by holding shift (so shift + hjkl)
- Esc to exit
# FAQ
## Why implement core features in plugins?
Two reasons primarily:
1. To exercise the plugin system while developing features. It makes it easier for me to find bugs with the plugin implementation if I'm using it myself
2. To provide a reference for plugin features (TODO use this repo for the plugins source)
## What is this built with?
Flutter for the UI, Go for the backend, Python for the plugins, Protobuf for communication between. Having the additional Go process isn't the most sound architectural decision but having an additional ecosystem of libraries helped increase the coverage of app functionality provided by dependencies. It's also been a fun way to learn Go.
## Why does the screen freeze on paste for Gnome on X11?
- This is due to a bug in the gnome mutter library but can be fixed without too much trouble (but does require manually fixing a line in mutter and rebuilding it)
	- https://gitlab.gnome.org/GNOME/mutter/-/issues/398
	- https://www.reddit.com/r/linux_gaming/comments/qaxz3m/comment/hqankhn/


# TODO
- avoid blocking code in plugin sdk
	- and cancel outdated query requests
- allow for shell script / on-demand plugins (rather than only long-lived subprocesses)
- proper signing of the MacOS app
- open launcher when dock icon is clicked
- add settings page
- don't open app if already open
- handle hotkey bind failure
- show keyword result entry for possible keyword results
- use pyinstaller to avoid python3 dependency on macOS for default app
- auto-update default config and copied config (if unchanged) when new version run
- allow for more modular plugins
- fix issue with app hanging on Ubuntu when the clipboard is empty
- default results sometimes show above other results
- smart ordering of results based on usage
- fix occasional incorrect window sizing
- extra actions on results when holding modifiers
- easy escape from nested pages

# Possible features
I'm currently just developing Epte to fit my needs so will only be adding things that I find useful. If there are features you would like, including any of the below, please open a Github Issue to request it
- Launcher usuals
	- calculator
	- currency conversion
	- etc.
- integrations
	- Gmail
	- Outlook
	- Notion
	- Jira
	- Confluence
	- etc.
- Adapt plugins for other launchers like Alfred, Raycast, Albert, Cerebro, ... (TODO investigate which of these this would be possible for)
- AI generated result actions / plugins

# App Behaviors
Here is a list of the things the app will do so there are hopefully no surprises:
- Register global shortcuts
	- Ctrl + Shift + V to open the clipboard history view
	- Ctrl + Shift + Space to open the primary search view
- Monitor and paste from the clipboard
	- TODO make configurable
- Create user app data directory
	- `~/Library/Application Support/epte/` on macos, `~/.epte/` elsewhere
	- Log to the user app data path (when logging is enabled)
	- create a sqlite db there for clipboard history
- Create a directory in `~/Documents/Epte` with a place to put packaged plugins or create a new plugin
- spawn multiple process
	- "Epte" - the Flutter frontend
	- "eptebe" - the Go backend
	- one process per Python plugin
- misc plugin
	- Read browser bookmarks
	- Make requests to http://suggestqueries.google.com/complete/search for "g " keyword results handling
	- Read from `~/.zsh_history` for the zsh history "z " keyword results handling
	- (macos) run OSAscripts to fetch chrome tabs for the "ct " (chrome tabs) keyword results handling
	- gmail enabled (TODO)
		- request gmail auth
- get apps plugin
	- search common application directories to get a list of applications to show in the default search view
- no network requests are made by this app other than to Google for the autocomplete results mentioned above
	
# Attribution
- Screenshot icon by Flatpik: https://www.flaticon.com/free-icons/screenshot
