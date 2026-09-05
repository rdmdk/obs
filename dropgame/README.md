A stylized leaderboard for use with https://www.pixelplush.dev/twitch.html?type=parachute

I use Firebot to store and provide data to the website in order to render the scores but any stream bot service that supports writing to local files should work

# Text file

A new text file will need to be created on your computer to store the Parachute game scores, and the path to the file will be referenced as path/to/file.txt in these instructions.

# OBS

Add a new browser source (preferably in a scene that is always visible)
URL: https://rdmdk.github.io/obs/dropgame/
Width: 1920
Height: 1080
Custom CSS: (empty)
Rename the browser source to "Drop Game Leaderboard"

# Firebot

On the Commands tab, create a New Custom Command
Trigger: !droprecord
Base Effects:
- Set OBS Browser Source URL
  - OBS Browser Source: Drop Game Leaderboard
  - URL: https://rdmdk.github.io/obs/dropgame/?$readFile[/path/to/file.txt]
- Toggle OBS Source Visibility
  - Sources: Drop Game Leaderboard
    - Show
- Delay
  - Duration: 10 seconds
- Toggle OBS Source Visibility
  - Sources: Drop Game Leaderboard
    - Hide
   
On the Events tab, create a New Event
Trigger On: Chat Message
Name: Drop Game
Filters
- Message Text contains landed for
- Viewer's Roles include Streamer
Manage Effects
- Write to File
  - Choose File: /path/to/file.txt
  - Write Mode: Suffix
  - Text: $user-$replace[$chatMessage, .*?\s|!, "", true]!
- Conditional Effects
  - If
    - Conditions (all)
      - $readFile[/path/to/file.txt] contains $replace[$chatMessage, \s.*, "", true]
