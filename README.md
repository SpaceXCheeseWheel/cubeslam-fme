Cube Slam - Florida Man Edition
=========

Cube Slam FME is a attempt to revitalize the original WebRTC Chrome Experiment that was shut down by Google after years of no maintenance. 


## Milestones

1. Get Cube Slam running on a sandboxed system with era-accurate dependencies (or as close as possible)
    - All components dependencies have been found and replaced with standard packages.
    - App is designed to run on google cloud (app.yaml). There is a gcloud simulator, but ancient app.yaml causes problems. 
    - Find and replace w/ the following components: a host for static content and a golang backend for online play.
      -  Golang backend is NOT in a modern go format. Will need to make a module? 
2. Update all user-facing dependencies to modern versions as needed.
    - Full scope of this tbd. Very likely that this relies on outdated or deprecated browser APIs.
    - Jade -> Pug
      - Uses a VERY outdated version of the jade format.
    - Update the python script for converting models to python3.
4. Fix bugs (no music, webrtc issues, etc)
    - Music is handled by dmaf.min.js; It seems like music logic, among other things, is stored in here. Is minified and no public source exists. Could un-minify? Look into https://gist.github.com/tencircles/c202c8835d5b02262afd and https://gist.github.com/shotgunner/9ed36812420e853f584e87141618ad77.


----
## Original Documentation:


## Building

  **Requirements:**

  * python (2.7.2 tested)
  * node (0.8.x tested)
  * make (GNU Make 3.81 tested, should come with xcode dev tools on mac)

  Building is done using a `Makefile` which in turn uses `npm` and `component` for packages and `jade` and `stylus` for templating.

  But to get started all you have to do it make sure `python` and `node` is installed and then:

    $ make

  While developing I'd recommend using [watch](http://github.com/visionmedia/watch) so you don't have to keep running that command manually. The Makefile is set up to only run whenever something changes.


## Testing

  **Requirements:**

  * Go App Engine (`brew install go-app-engine-64`)

  To get it up and running on a local machine the app engine dev server must be up and running. Start it with:

    $ dev_appserver.py .

  or use an nginx proxy to take care of the static files using:

    $ make proxy # and follow the instructions...


## Deploying

  To deploy you must first be added to the app engine user list. Then it's simply a matter of using `make`:

    $ make deploy-webrtcgame

  Where the `webrtcgame` part depends on the app you want to deploy to. See the Makefile for available options.

### Versioned deploys

  Right now it will automatically increment the version number on each deploy. Simply because it will be easier to refer to a version while doing QA.

  If you want to deploy to a specific version (like to version 1 which will be the default) you add `v=` like this:

    $ make deploy-webrtcgame v=1

  The version can be anything that can be a subdomain such as "hello-there-123". Which would be accessible at http://hello-there-123.webrtcgame.appspot.com/.


## Query string shortcuts

Add these to the URL querystring to toggle some flags.

### Network

  * `buffer` - should the rtc signal buffer candidates before sending them
  * `signal` - set to `ws` to use the WebSocket signal for rtc

### Multiplayer

  * `autonav` - automatically press buttons to get to the game (use https:// to go all the way without accepting camera more than once)
  * `render` - set to `sync` to render the sync game in a 2d renderer window
  * `ai` - turns on the AI for the opponent (not very useful)
  * `verify` - stores hashes and the states for verification. set to a number (ms) to start an interval which sends them over the network. or use `.`-key to do it manually.

### Singleplayer

  * `play` - go directly to a game against Bob

### Game play

  * `quality` - webgl rendering quality. possible values are `best`, `high`, `low`, `mobile`
  * `god` - no player can die
  * `momentum` - set to `off` to turn off paddle momentum
  * `round` - set to a number to start at that particular round
  * `extras` - set to a comma separated list of extras to override the level extras.
  * `framerate` - set to a number to set the games framerate
  * `speed` - set to a number to set the games unit speed. which all speeds are based upon.
  * `ns` - the namespace of levels. can be either `single`, `mobile` or `multi`.
  * `level` - the level to play. can be any number but there's up to 11 scripted levels. after that it'll be randomized.

### Localization

  * `lang` - set to a locale like `en` or `sv` to override the browser locale

### Debug

  * `dev` - enables the settings gui
  * `see` - set to a path to start at (see `lib/app.js` for available paths)
  * `mobile` - force mobile
  * `paused` - start game in paused mode. step forward a frame using `.`-key.
  * `silent` - don't load in sounds
  * `dmaf` - turn on dmaf logs

