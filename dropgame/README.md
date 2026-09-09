A stylized leaderboard for use with https://www.pixelplush.dev/twitch.html?type=parachute

I use Firebot to store and provide data to the website in order to render the scores but any stream bot service that supports custom variables should work

# OBS

Add a new browser source (preferably in a scene that is always visible)

URL: https://rdmdk.github.io/obs/dropgame/

Width: 1920

Height: 1080

Custom CSS: (empty)

Rename the browser source to "Drop Game Leaderboard"

# Firebot

## Commands

On the Commands tab, create a New Custom Command

Skip the Trigger and go straight to Base Effects as we will only be testing this command and not saving it

Base Effects:
- Custom Variable
  - Variable Name: drop_game_leaderboard
  - Variable Data: `{}`
 
Once the Custom Variable is added, click the blue play button beside Manage Effects to run the effects list. This should have now created the drop_game_leaderboard variable and the command can now be discarded.

Create a New Custom Command

Trigger: !droprecord

Base Effects:
- Set OBS Browser Source URL
  - OBS Browser Source: Drop Game Leaderboard
  - URL: `https://rdmdk.github.io/obs/dropgame/?$$drop_game_leaderboard`
- Toggle OBS Source Visibility
  - Sources: Drop Game Leaderboard
    - Show
- Delay
  - Duration: 10 seconds
- Toggle OBS Source Visibility
  - Sources: Drop Game Leaderboard
    - Hide

## Events

On the Events tab, create a New Event

Trigger On: Chat Message

Name: Drop Game

Filters

- **Message Text** contains **landed for**
- **Viewer's Roles** include **Streamer**
- Manage Effects
  - Conditional Effects
    - If
      - Conditions (any)
        - Custom: `$$drop_game_leaderboard[$replace[$chatMessage, \s.*, "", true]]` is ``
        - Custom: `$replace[$chatMessage, .*\s|!, "", true]` is greater than `$$drop_game_leaderboard[$replace[$chatMessage, \s.*, "", true]]`
      - Manage Effects
        - Custom Variable
          - Variable Name: drop_game_leaderboard
          - Variable Data: `$replace[$chatMessage, .*\s|!, "", true]`
          - Property Path: `$replace[$chatMessage, \s.*, "", true]`
