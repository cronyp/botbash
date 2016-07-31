## Purpose
This suggests how to implement multi-user functionality using the current release of PokemonGo-Bot from the dev branch
## Application
- Web View for PokemonGo-Bot
- Bash boot

## Prerequisite
### 1. Set up a SimpleHTTPServer
This can be found in the Wiki of PokemonGo-Bot [here.](https://github.com/PokemonGoF/PokemonGo-Bot/wiki/Google-Maps-API-(web-page)) If i can only add, for step 3. Change Base Directory of the Webserver, it is referring to line 36.
### 2. Copy config.json files
In your project folder, navigate to configs/ and copy the config file of your choice to bot1.json, bot2.json,...
You can set each config file you copy now if you wish but the bash will prompt you for each bot#.json you create when you start the bash.
### 3. Set userdata.js file
In your project folder, navigate to web/ and add the usernames for each bot#.json file. The username is the email address used for the config. Line 3 Example:
`var users = ["emailusedbot1@gmail.com","emailusedbot2@gmail.com"];`
## Procedure
Set up and create the master bot bash:
```
cd ~
nano bot1.sh
```
Once in the editor write the following:
```
#!/bin/bash

# Prepare Master Bot (Main)
cd YOUR/PROJECT/FOLDER/PokemonGo-Bot && virtualenv . && source bin/activate
nano configs/bot1.json
# Exit config and boot Nginx Server
gnome-terminal -e 'bash -c "cd ~/YOUR/PROJECT/FOLDER/PokemonGo-Bot/web/ && python -m SimpleHTTPServer"'
# Open and launch LocalHost in browser
xdg-open http://localhost:8000/
# For every bot launch in new terminal
# For every bot you plan to run you will need to create a separate bash
gnome-terminal -e 'bash -c "cd ~/ && ./bot2.sh"'
# Start main bot
python pokecli.py -cf ./configs/bot1.json
```
Now set up and create each child bot bash for each bot you plan to run:
```
cd ~
nano bot2.sh
```
Once in the editor write the following:
```
#!/bin/bash

# Prepare Child Bot (2)
cd YOUR/PROJECT/FOLDER/PokemonGo-Bot && virtualenv . && source bin/activate
nano configs/bot2.json
# Exit config and start
python pokecli.py -cf ./configs/bot2.json
```
For every bot#.sh file you just created set permissions:
```
cd ~
ls
chmod 700 bot#.sh
```
Always from a new terminal start your master bot bash:
```
cd ~
./bot1.sh
```
## Helpful hints and known issues
- ctrl-c : Using ctrl-c will exit PokemonGo Bot, if it is not the master bot bash the terminal will auto close. Press and hold ctrl-c to exit the server terminal.
- if the session is interrupted it is best to close out of all bot terminals and create to to start again
