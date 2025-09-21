# i3 Simple Scratchpad

Bash script that simplifies i3 setup of scratchpads for specific applications, with sensible default behavior.

Example scratchpad binding:

```
bindsym $mod+p exec i3-simple-scratchpad Bitwarden "800 1200" "bitwarden"
```

For comparison, without this script you might do something like this:

```
exec --no-startup-id bitwarden
for_window [title="Bitwarden"] move to scratchpad
for_window [title="Bitwarden"] floating enable
for_window [title="Bitwarden"] resize set 800 1200
for_window [title="Bitwarden"] move position center
bindsym $mod+p [title="Bitwarden"] scratchpad show; move position center
```

The issue with this is that you have to manually restart the scratchpad program again if killed for whatever reason. 

This script takes care of managing the target program, starting it lazily when toggling. If the program crashes or is manually exited, it will automatically be started again.

The script configures the scratchpad as a floating window positioned in the center of the screen. To change this to your liking, edit the script.

## Installation

Download script, put it in your path and make it executable:

```
curl -o ~/.local/bin/i3-simple-scratchpad https://raw.githubusercontent.com/emanueldonalds/i3-simple-scratchpad/refs/heads/master/i3-simple-scratchpad
chmod +x ~/.local/bin/i3-simple-scratchpad
```

## Usage

```text
Usage: i3-simple-scratchpad <title> <size> <cmd>

  title   The window title that will be used to identify the target program's window. Must match the title of the node in i3.
  size    Scratch window width and height. Example: "800 500"
  cmd     Command to start the target program. Example: "alacritty --title audio -e pulsemixer"

If the program does not allow you to explicitly set the window title, you can find out the title value like this: Start your target program normally, then run "i3-msg get_tree | jq '.nodes'" and find your program's node. The title is found in ".window_properties.title"
```

## Examples

Binding a terminal (Alacritty) to `mod+t`:

```
bindsym $mod+t exec i3-simple-scratchpad terminal "800 500" "alacritty --title terminal"
```

Binding Bitwarden to `mod+p`:

```
bindsym $mod+p exec i3-simple-scratchpad Bitwarden "800 1200" "bitwarden"
```

Binding a terminal (Alacritty) running pulsemixer to `mod+a`:

```
bindsym $mod+a exec i3-simple-scratchpad audio "800 500" "alacritty --title audio -e pulsemixer"
```
