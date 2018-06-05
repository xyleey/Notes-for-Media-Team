<h1 align="center"><img src="https://github.com/TeamSII/TeamSII.github.io/blob/master/image/NOTES%20copyright.png" width="500" height="400" alt="YESUNG"></h1>

These notes are positively tested as available on the platform of Ubuntu, a subsystem simulates linux environment.

Please keep your CLU consistent with the version mentioned in the notes or, apply it in your special demand.

## Index
- [Ubuntu for Windows 10](#ubuntu-for-Windows-10)
- [Tools](#tools)
  - [aria2](#aria2)
  - [ffmpeg](#ffmpeg)
  - [Python](#python)
  - [caterpillar-hls](#caterpillar-hls)
  - [kvm48](#kvm48)
  - ['Y's](#ys)
- [Linux command tips](#linux-command-tips) 
 
## Ubuntu for Windows 10

[Install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

1. Open PowerShell as Administrator and run

`Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`

2. Restart your computer when prompted.

3. Open the Microsoft Store and choose your favorite Linux distribution. I recommand the newest version `Ubuntu 18.04`.

## Tools

### aria2
[Aria2](https://aria2.github.io/), a funtional technique to download under multiple threads.

```console
$ sudo apt install -y aria2
$ aria2c -v   or   $ aria2c --version
```
Usage `$ aria2c -h` or see examples on https://aria2.github.io/

### ffmpeg
[`ffmpeg`](http://ffmpeg.org/download.html) -> use `jonathonf/ffmpeg-3`.
```console
$ sudo add-apt-repository ppa:jonathonf/ffmpeg-3
$ sudo apt-get update
$ sudo apt-get install -y ffmpeg
$ ffmpeg -version
```
#In case of `add-apt-repository: command not found`

```console
$ sudo apt install software-properties-common
```

### Python 
- Python 3.6.0+ depends on Ubuntu release
  ```console
  $ sudo apt update
  $ sudo apt install python3 python3-pip+
  $ apt-cache show python3 | grep -i version
  ```	
  *** Maybe you should download python 3.6.4 by pyenv (see [Install Python 3.6+ in Ubuntu by pyenv])
------------------------------------------------------------------------------------------------------------------------------------

#Install Python 3.6+ in Ubuntu by pyenv

Guide: https://github.com/zmwangx/caterpillar/wiki/Installation-Guide-for-Novices#zesty-1704-or-earlier-with-pyenv

* $ sudo apt update 

* Follow these instructions on the pyenv wiki to install prerequisite packages with apt;
  $ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev
  
* You must have git:
  $ sudo apt install -y git 
  
* Check out pyenv where you want it installed. (as default $HOME/.pyenv)
  $ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
  
* Define environment variable PYENV_ROOT
  $ nano ~/.bashrc
  # Append the following lines:
    export PYENV_ROOT="$HOME/.pyenv"
    export PATH="$PYENV_ROOT/bin:$PATH"
	
    eval "$(pyenv init -)"
  Ctrl+X to exit and press Y as "save & exit", [Enter]
  
* Restart your shell so the path changes take effect
  $ exec bash -l 

* See those lines you add at the bottom of 。bashrc (just to ensure the Appendix)
  $ tail -5 .bashrc

* List all the things you can install (optional)
  $ pyenv install -l 
  wait for several seconds to load the list
  
* Install python3.6.4
  $ pyenv install 3.6.4
  # You can find the latest stable Python release number at https://www.python.org/downloads/, which is not necessarily 3.6.4, apparently.
  
* Choose the Python Version
  $ nano ~/.pyenv/version
  Type two lines at start:
  3.6.4
  system  
  
* Restart the shell 
  $ exec bash -l
  
* Now you can verify your Python version
  $ python3 --version
  
------------------------------------------------------------------------------------------------------------------------------------
	
- caterpillar
  $ pip3 install caterpillar-hls
    #You need to add $HOME/.local/bin to your $PATH to use caterpillar
  $ echo "export PATH=$PATH:$HOME/.local/bin"
	 
------------------------------------------------------------------------------------------------------------------------------------
	 
#KVM48 https://github.com/SNH48Live/KVM48#readme

Install  
  $ pip install KVM48
 
  $ kvm48 --help
usage: kvm48 [-h] [-f FROM] [-t TO] [-s SPAN] [--dry] [--config CONFIG]
             [--version] [--debug]

KVM48, the Koudai48 VOD Manager.

KVM48 downloads all streaming VODs of monitored members in a specified
date range. Monitored members and other options are configured through
the YAML configuration file

  $HOME/.config/kvm48/config.yml

or through a different configuration file specified with the --config
option.

The date range is determined as follows. All date and time are processed
in China Standard Time (UTC+08:00). --from and --to are specified in the
YYYY-MM-DD or MM-DD format.

- If both --from and --to are specified, use those;

- If --to is specified and --from is not, determine the date span
  through --span and the `span' config option (the former takes
  priority), then let the date range be `span' number of days
  (inclusive) ending in the --to date.

  For instance, if --to is 2018-02-18 and span is 7, then the date range
  is 2018-02-12 to 2018-02-18.

- If --from is specified and --to is not, then

  - If --span is explicitly specified on the command line, let the date
    range be `span' number of days starting from the --from date.

  - Otherwise, let the date range be the --from date to today (in
    UTC+08:00).

- If neither --from nor --to is specified, use today (in UTC+08:00) as
  the to date, and determine span in the same way as above.

KVM48 uses aria2 as the downloader. Certain aria2c options, e.g.,
--max-connection-per-server=16, are enforced within kvm48; most options
should be configured directly in the aria2 config file.

optional arguments:
  -h, --help            show this help message and exit
  -f FROM, --from FROM  starting day of date range
  -t TO, --to TO        ending day of date range
  -s SPAN, --span SPAN  number of days in date range
  --dry                 print URL & filename combos but do not download
  --config CONFIG       use this config file instead of the default
  --version             show program's version number and exit
  --debug


### kvm48
[KVM48](https://github.com/SNH48Live/KVM48), the Koudai48 VOD Manager. It is capable of downloading all streaming VODs of a set of monitored members in a specified date range. It collaborates with aria2 and caterpillar

Usage `kvm48 -h`

On windows, you can find the path of `config.yml` printed in the output of `kvm48 -h`. 

Edit this configuration file first before you use it.

- New process mode for url.m3u8 (20180605 update)
  This update allows kvm48 to distinguish all `url.m3u8`. It picks them up and prints them into `m3u8.txt` as it consists to caterpillar's batch mode requirements. Then we run `caterpillar m3u8.txt` to download those VODs concurrently.

## Linux command tips 
Tip: Return to the default directory: cd ~
     Default folder for Ubuntu in Win10 in my case: 
	         C:\Users\Yesung\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs\home\Yesung
	 history | more


<p align="center">
------------------------------------------------------------------ End ------------------------------------------------------------------
</p>
<p align="left">
*Notes for Win10_x64 V2.5.md Update 20180605
</p>  
   
