# A simple way to run freqtrade bots on a Raspberry PI

## Requirements:
- Raspberry PI (or similar arm boards like Orange PI, RockPI, etc.) with installed Raspbian OS (or some compatible Debian distros) with enabled SSH
- Bash shell on your PC/Mac (for Windows user you can use `Git BASH` terminal)
- Git on your PC/Mac

## Software installation:
! From now on, let's say that you have a PI running at `192.168.0.10`, and your username is `pi`

- Clone this repo and get inside the project root:
```
git clone https://github.com/tl-nguyen/freqtrade-with-docker-and-pi.git
cd freqtrade-with-docker-and-pi
```

- Install docker, pull the freqtrade image from dockerhub and move the ./freqtrade folder into PI home directory using remote_install.sh script
```
./remote_install.sh pi@192.168.0.10
```

## Add a new strategy bot:
- Add strategy class file, json config file into `freqtrade-with-docker-and-pi/freqtrade/strategies` folder.

- Rename the strategy class file, json config file with the same name (the extensions stay the same). For example: `bbrsi.py`, `bbrsi.json`

- Move/Update the strategy files into the PI using `remote_update.sh` script. It's important to use the name of the files (in this case `bbrsi`) as the strategy name. This command will move your strategy files to the PI, if the strategy is running, this command also restart it with the updated files
```
./remote_update.sh pi@192.168.0.10 bbrsi
```

## Start the strategy bot
- To start the strategy bot you have to execute the `remote_start.sh` script

! Let's say that BBRSI is the class name of your strategy (The class defined in the `bbrsi.py` file)
```
./remote_start.sh pi@192.168.0.10 bbrsi BBRSI
```

- You can start the bot with additional freqtrade params. For Example: --dynamic-whitelist
```
./remote_start.sh pi@192.168.0.10 bbrsi BBRSI "--dynamic-whitelist 100"
```

## Stop the strategy bot
- Use `remote_stop.sh` script to stop and remove the bot
```
./remote_stop.sh pi@192.168.0.10 bbrsi
```

## Strategy logs
- Use `remote_logs.sh` script to see the strategy logs in real time
```
./remote_logs.sh pi@192.168.0.10 bbrsi
```

## Check which strategies are running
- Use `remote_ps.sh` script to see which strategies are running right now
```
./remote_ps.sh pi@192.168.0.10
```

## Run multiple strategies
You can run multiple strategies by repeating all of the steps from `Add a new strategy bot` and `Start the strategy bot` sections. With Raspberry PI 3 on Raspbian Lite, you can run up to 4 strategies simultanously

You have to create a telegram bot for each of your strategy if you want to use telegram to manage your trades.



