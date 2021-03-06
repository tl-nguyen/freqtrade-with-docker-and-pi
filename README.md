# A simple way to run freqtrade bots on a Raspberry PI

## Requirements:
- Raspberry PI (or similar arm boards like Orange PI, RockPI, etc.) with installed Raspbian OS (or some compatible Debian distros) with enabled SSH
- Bash shell on your PC/Mac (for Windows user you can use `Git BASH` terminal)
- Git on your PC/Mac

## Software installation:
! From now on, let's say that you have a PI running at `192.168.0.10`, and your username is `pi`

- Clone this repo and get inside the project root:
```
git clone https://github.com/tl-nguyen/freqtrade-pi.git
cd freqtrade-pi
```

- Modify the `config.yml` file. Add username and hostname of your RPI:
```
username: pi
hostname: 192.168.0.10
```

- Use `help` command to learn more about the fp.sh script:
```
./fp.sh help
```

- Use `install` command to install docker, pull the freqtrade image from dockerhub and move the ./freqtrade folder into RPI home directory:
```
./fp.sh install
```

- Generate SSH keys so you won't be asked when sending commands to your RPI:
```
ssh-keygen; ssh-copy-id pi@192.168.0.10
```
Press enter a few times when be asked to enter location or passphrase (or enter them if you know what are they about), the keys will be created by default in ~/.ssh/ directory. After that you will be asked to enter your RPI password in order for the public key to be placed there. When it is done, you can ssh to your RPI directly without the password

## Add a new strategy bot:
- Add strategy class file, json config file into `freqtrade-with-docker-and-pi/freqtrade/strategies` folder.

- Rename the strategy class file, json config file with the same name (the extensions stay the same). For example: `bbrsi.py`, `bbrsi.json`

- Use `update` command to Move/Update the strategy files into the PI. It's important to use the name of the files (in this case `bbrsi`) as the strategy name. This command will move your strategy files to the PI, if the strategy is running, this command also restart it with the updated files:
```
./fp.sh update -s bbrsi
```

## Start the strategy bot
- If BBRSI is the class name of your strategy (The class defined in the `bbrsi.py` file)

- Modify `config.yml`. Add your strategy file name and strategy class name associated with it in the strategies section:
```
strategies:
    bbrsi: BBRSI
```

- Use `start` command to start the strategy:
```
./fp.sh start -s bbrsi
```

- You can start the bot with additional freqtrade params. For Example: --dynamic-whitelist
```
./fp.sh start -s bbrsi -p "--dynamic-whitelist 100"
```

## Stop the strategy bot
- Use `stop` command to stop and remove the bot:
```
./fp.sh stop -s bbrsi
```

## Strategy logs
- Use `logs` command to see the strategy logs in real time:
```
./fp.sh logs -s bbrsi
```

## Check which strategies are running
- Use `ps` command to see which strategies are running right now:
```
./fp.sh ps
```

## Run multiple strategies
You can run multiple strategies by repeating all of the steps from `Add a new strategy bot` and `Start the strategy bot` sections. With Raspberry PI 3 on Raspbian Lite, you can run up to 4 strategies simultanously

You have to create a telegram bot for each of your strategy if you want to use telegram to manage your trades.



