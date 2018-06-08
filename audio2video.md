
# audio2video
[audio2video](https://github.com/fanaticscripter/multimedia-scripts) can combine an image and an audio into a video.
This program bases on the [loop](https://github.com/fanaticscripter/multimedia-scripts/blob/master/audio2video) feature from ffmpeg.

## Recommanded installation
   
   ```console
    $ sudo add-apt-repository ppa:fanaticscripter/multimedia
    $ sudo apt-get update
    $ sudo apt install multimedia-scripts
    $ audio2video -h
    ```
    
- [`https://launchpad.net/~fanaticscripter/+archive/ubuntu/multimedia`](https://launchpad.net/~fanaticscripter/+archive/ubuntu/multimedia)  
- [`https://github.com/fanaticscripter/multimedia-scripts`](https://github.com/fanaticscripter/multimedia-scripts) 
  
## Update 20180605

  * Add multimedia-scripts bionic i.e. bionic (18.04)
    Therefore, while you install audio2video successfully, you have ffmpeg 3.4.2-2 too.
 
## Possible case

Primarily the developer has already released the update build, which could avoid the appearance of the following situation:

```console
$ sudo add-apt-repository ppa:fanaticscripter/multimedia
 Small utilities for multimedia processing.
 More info: https://launchpad.net/~fanaticscripter/+archive/ubuntu/multimedia
Press [ENTER] to continue or Ctrl-c to cancel adding it.

Hit:1 http://archive.ubuntu.com/ubuntu bionic InRelease
Get:2 http://archive.ubuntu.com/ubuntu bionic-updates InRelease [83.2 kB]
Get:3 http://archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]
Get:4 http://security.ubuntu.com/ubuntu bionic-security InRelease [83.2 kB]
Get:5 http://ppa.launchpad.net/fanaticscripter/multimedia/ubuntu bionic InRelease [15.9 kB]
Ign:6 http://ppa.launchpad.net/jonathonf/ffmpeg-3/ubuntu bionic InRelease
Err:7 http://ppa.launchpad.net/jonathonf/ffmpeg-3/ubuntu bionic Release
  404  Not Found [IP: 91.189.95.83 80]
Get:8 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages [113 kB]
Get:9 http://archive.ubuntu.com/ubuntu bionic-updates/main Translation-en [43.9 kB]
Get:10 http://archive.ubuntu.com/ubuntu bionic-updates/universe amd64 Packages [68.4 kB]
Get:11 http://archive.ubuntu.com/ubuntu bionic-updates/universe Translation-en [30.6 kB]
Get:12 http://archive.ubuntu.com/ubuntu bionic-backports/universe amd64 Packages [2684 B]
Get:13 http://archive.ubuntu.com/ubuntu bionic-backports/universe Translation-en [1136 B]
Get:14 http://security.ubuntu.com/ubuntu bionic-security/main amd64 Packages [74.1 kB]
Get:15 http://security.ubuntu.com/ubuntu bionic-security/main Translation-en [27.1 kB]
Get:16 http://security.ubuntu.com/ubuntu bionic-security/universe amd64 Packages [19.9 kB]
Get:17 http://security.ubuntu.com/ubuntu bionic-security/universe Translation-en [10.2 kB]
Get:18 http://ppa.launchpad.net/fanaticscripter/multimedia/ubuntu bionic/main amd64 Packages [468 B]
Get:19 http://ppa.launchpad.net/fanaticscripter/multimedia/ubuntu bionic/main Translation-en [184 B]
Reading package lists... Done
E: The repository 'http://ppa.launchpad.net/jonathonf/ffmpeg-3/ubuntu bionic Release' does not have a Release file.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.

$ sudo add-apt-repository --remove ppa:jonathonf/ffmpeg-3
 Backport and/or package of FFMPEG 3 and key libraries

***

Cumulative downloads at 2017-12-20: 68,504 (based on ffmpeg)

Total downloads of 3.4: 24,113
Total downloads of 3.3.2: 2,814

***

Find PPAs useful? Consider a donation to those you use.
 More info: https://launchpad.net/~jonathonf/+archive/ubuntu/ffmpeg-3
Press [ENTER] to continue or Ctrl-c to cancel removing it.

$ sudo add-apt-repository ppa:fanaticscripter/multimedia
 Small utilities for multimedia processing.
 More info: https://launchpad.net/~fanaticscripter/+archive/ubuntu/multimedia
Press [ENTER] to continue or Ctrl-c to cancel adding it.

Get:1 http://security.ubuntu.com/ubuntu bionic-security InRelease [83.2 kB]
Hit:2 http://archive.ubuntu.com/ubuntu bionic InRelease
Get:3 http://archive.ubuntu.com/ubuntu bionic-updates InRelease [83.2 kB]
Get:4 http://archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]
Hit:5 http://ppa.launchpad.net/fanaticscripter/multimedia/ubuntu bionic InRelease
Get:6 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 Packages [113 kB]
Get:7 http://archive.ubuntu.com/ubuntu bionic-updates/universe amd64 Packages [68.4 kB]
Fetched 423 kB in 2s (200 kB/s)
Reading package lists... Done

$ sudo apt-get update
Hit:1 http://ppa.launchpad.net/fanaticscripter/multimedia/ubuntu bionic InRelease
Hit:3 http://archive.ubuntu.com/ubuntu bionic-updates InRelease
Get:4 http://archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]
Get:5 http://security.ubuntu.com/ubuntu bionic-security InRelease [83.2 kB]
Fetched 158 kB in 2s (74.2 kB/s)
Reading package lists... Done

$ sudo apt install multimedia-scripts
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  zsh zsh-common
Suggested packages:
  zsh-doc
The following NEW packages will be installed:
  multimedia-scripts zsh zsh-common
0 upgraded, 3 newly installed, 0 to remove and 41 not upgraded.
Need to get 4068 kB of archives.
After this operation, 15.2 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://archive.ubuntu.com/ubuntu bionic/main amd64 zsh-common all 5.4.2-3ubuntu3 [3376 kB]
Get:2 http://ppa.launchpad.net/fanaticscripter/multimedia/ubuntu bionic/main amd64 multimedia-scripts all 0.1-0bionic1 [2368 B]
Get:3 http://archive.ubuntu.com/ubuntu bionic/main amd64 zsh amd64 5.4.2-3ubuntu3 [689 kB]
Fetched 4068 kB in 1s (5242 kB/s)
Selecting previously unselected package zsh-common.
(Reading database ... 43598 files and directories currently installed.)
Preparing to unpack .../zsh-common_5.4.2-3ubuntu3_all.deb ...
Unpacking zsh-common (5.4.2-3ubuntu3) ...
Selecting previously unselected package zsh.
Preparing to unpack .../zsh_5.4.2-3ubuntu3_amd64.deb ...
Unpacking zsh (5.4.2-3ubuntu3) ...
Selecting previously unselected package multimedia-scripts.
Preparing to unpack .../multimedia-scripts_0.1-0bionic1_all.deb ...
Unpacking multimedia-scripts (0.1-0bionic1) ...
Setting up zsh-common (5.4.2-3ubuntu3) ...
Processing triggers for man-db (2.8.3-2) ...
Setting up zsh (5.4.2-3ubuntu3) ...
Setting up multimedia-scripts (0.1-0bionic1) ...

$ audio2video -h
Usage: audio2video <image> <bgm> <output>
