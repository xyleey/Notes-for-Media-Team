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
  - [KVM48](#kvm48)
- [Extra Comments](#extra-comments)
  - [Download youku videos](#download-youku-videos) (gist)
  - [Linux command tips](#linux-command-tips) 
 
## Ubuntu for Windows 10

[Install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

1. Open PowerShell as Administrator and run

  `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`

2. Restart your computer when prompted.

3. Open the Microsoft Store and choose your favorite Linux distribution. I recommand the newest version `Ubuntu 18.04`.

* Default folder for Ubuntu in Win10 in my case:    
`C:\Users\User\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs\home\User
	 history | more`
* The Canonical regularly releases a new version of Ubuntu every 6 months. Hence you may see a package name follows by the version title, e.g. (18.04), which allows you to match your work needs.

## Tools

### aria2
[Aria2](https://aria2.github.io/), a funtional technique to download under multiple threads.

```console
$ sudo apt install -y aria2
$ aria2c -v   or   $ aria2c --version
```
Usage `$ aria2c -h` or see examples on https://aria2.github.io/

### ffmpeg

In case of supporting the caterpillar in Ubuntu, you need to pick a suitable way to install [`ffmpeg`](http://ffmpeg.org/download.html). 

Guidance see: https://github.com/zmwangx/caterpillar/wiki/Installation-Guide-for-Novices#ffmpeg
You may refer [official package information](https://packages.ubuntu.com/search?keywords=ffmpeg&searchon=names&exact=1&suite=all&section=all) as well as you choose the correct method.

- Artful (17.10) or later
The FFmpeg in the official repository should be good enough.
```console
$ sudo apt update
$ sudo apt install ffmpeg
```

- Zesty (17.04)
Unfortunately, the FFmpeg in the official repository (3.2.4 at the moment) does not appear to work with caterpillar. However, Zesty reaches end of life in January 2018, so you should upgrade to Artful anyway.

- LTS: Trusty (14.04) or Xenial (16.04)
You may use the reputable `jonathonf/ffmpeg-3` PPA to install the latest FFmpeg on Trusty or Xenial.

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
!! Remind: Install properly ffmpeg in accordance with your Ubuntu version. 
           For Ubuntu (18.04) you just run `sudo apt install ffmpeg` as the official repository or you may [install audio2video](https://github.com/TeamSII/Notes-for-Media-Team/blob/master/audio2video.md#recommanded-installation) instead.

### Python 

Python 3.6.0+ depends on Ubuntu release, subsequently to install 

- [Artful (17.10) or later](https://github.com/zmwangx/caterpillar/wiki/Installation-Guide-for-Novices#artful-1710-or-later-1)
  Artful or later comes with Python 3.6+ right in the official repository.
  
  ```console
  $ sudo apt update
  $ sudo apt install python3 python3-pip+
  $ apt-cache show python3 | grep -i version  #(optional)
  ```	
  
- [Zesty (17.04) or earlier with pyenv](https://github.com/zmwangx/caterpillar/wiki/Installation-Guide-for-Novices#zesty-1704-or-earlier-with-pyenv)

```console
$ sudo apt update 
Follow the instruction on the pyenv wiki to install prerequisite packages with apt;

$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev
  
You must have git:
$ sudo apt install -y git 
  
Check out pyenv where you want it installed. (as default $HOME/.pyenv)
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
  
Define environment variable PYENV_ROOT
$ nano ~/.bashrc
 # Append the following lines:
   export PYENV_ROOT="$HOME/.pyenv"
   export PATH="$PYENV_ROOT/bin:$PATH"
	
   eval "$(pyenv init -)"
  Ctrl+X to exit and press Y as "save & exit", [Enter]
  
Restart your shell so the path changes take effect
$ exec bash -l

See those lines you add at the bottom of .bashrc (just to ensure the Appendix)
$ tail -5 .bashrc

List all the things you can install (optional)
$ pyenv install -l 

Wait for several seconds to load the list
```

Now we can use pyenv to install python
```console
Install python3.6.4
$ pyenv install 3.6.4
# You can find the latest stable Python release number at https://www.python.org/downloads/, which is not necessarily 3.6.4, apparently.
  
Choose the Python Version
$ nano ~/.pyenv/version

Type two lines at start:
   3.6.4
   system  
  
Restart the shell 
$ exec bash -l
  
Now you can verify your Python version
$ python3 --version
```

### caterpillar-hls
[`caterpillar-hls`](https://github.com/zmwangx/caterpillar) 适用于下载hls下的VODs, i.e.`url.m3u8`
```console
After you install a correct version of python (python 3.6.5 in this case), now you can install caterpillar

$ pip3 install caterpillar-hls
  
You need to add $HOME/.local/bin to your $PATH to use caterpillar
$ echo "export PATH=$PATH:$HOME/.local/bin"
```	
- Batch mode (20180605 update)

     `caterpillar manifest.txt` where the `manifest.txt` contains
     
     ```console
     https://example.com/hls/1.m3u8	1.mp4
     https://example.com/hls/2.m3u8	2.mp4
     https://example.com/hls/3.m3u8	3.mp4

### KVM48
[KVM48](https://github.com/SNH48Live/KVM48), the Koudai48 VOD Manager. It is capable of downloading all streaming VODs of a set of monitored members in a specified date range. It collaborates with aria2 and caterpillar

Install if you've installed python3-pip, `pip3 install KVM48`

        if you've installed python with pyenv,  `pip install KVM48
	`
Usage   `kvm48 -h`

You need to edit YAML configuration file before you can use kvm48 `$ vi config.yml`.

The path would be `$HOME/.config/kvm48/config.yml`

- New process mode for url.m3u8 (20180606 update)
  This update allows kvm48 to distinguish all `url.m3u8`. It picks them up and prints them into `m3u8.txt` as it consists to caterpillar's batch mode requirements. Then we run `caterpillar m3u8.txt` to download those VODs concurrently.

## Extra Comments

### Download Youku Videos

Follow the instruction of features of `appinfo`, we are able to download a video from youku with the highest quality.

Here I just simply describe the usage. If you are interested with the raw code, please click [here](https://gist.github.com/zmwangx/79f44ea27915a921b9b06e60043a9468#file-process-youku-appinfo-jsonp) switch to access the gist page.

- First you need to endore you've got all tools it needs:
  python3 (see above)
  GNU parallel (`sudo apt install -y moreutils parallel`)
  ffmpeg (see above)
  
- Open Youku video page with web inspector open, search for appinfo, click on the request, and "copy as cURL" 

- Now 

```console
In a Linux terminal, run the following command from a fresh directory:
$ curl ... | python3 -c "$(curl -s https://gist.githubusercontent.com/zmwangx/79f44ea27915a921b9b06e60043a9468/raw/process-youku-appinfo-jsonp)"
```

where `curl ...` presents the command you copied in the previous step.

#For instance, I want to download the video from https://v.youku.com/v_show/id_XMzY1NDkxODYyNA==.html?spm=a2h0k.11417342.soresults.dtitle

```console
$ curl 'https://acs.youku.com/h5/mtop.youku.play.ups.appinfo.get/1.1/?jsv=2.4.11&appKey=24679788&t=1529036729918&sign=60ee0fb2fddec185c8ff8f17f950529d&api=mtop.youku.play.ups.appinfo.get&v=1.1&timeout=20000&YKPid=20160317PLF000211&YKLoginRequest=true&type=jsonp&dataType=jsonp&callback=mtopjsonp2&data=%7B%22steal_params%22%3A%22%7B%5C%22ccode%5C%22%3A%5C%220502%5C%22%2C%5C%22client_ip%5C%22%3A%5C%22192.168.1.1%5C%22%2C%5C%22utid%5C%22%3A%5C%22lKSNE%2BlibmkCAR%2FN6%2BnV%2FZkl%5C%22%2C%5C%22client_ts%5C%22%3A1529036729%2C%5C%22version%5C%22%3A%5C%220.5.52%5C%22%2C%5C%22ckey%5C%22%3A%5C%22109%23WIIa7mbBaps%2FECSkpjekCVTUxnvERMZcCqXyeso5bI8SM1EsSjUnhCbGaSADaCk4%2FkmUYmrGaAIMGdv5zGp2d815tUQ77NAnhxYzDBOxKn%2Fy56EdlcDpgCCazWDjEz8gOE4O4spYBoPaBwwBlcDSggxL1t9Hb%2FfnLBiEz5fqL4nMaxCU25%2BiMDrUS7GKwojHteN3IFpaP6G%2F581cHiKqyj9TejSUWMed8CqKBp5ii7ChcS8E72uxvo%2Byt%2BPaViSuki6B5E0DG4A2VEaq8Bc82Ibzy69akDLiP1ER7ZmRUw1bR8tyGrFL5olstgUURByGNUSbhtVBqHmSUe4%2FHXzT1%2BPMYTGR8kF6UPG2utMQNopGmQ7xhCa0pAvs%2FK2uBrAkdkXaD2UbL0EceAQ1TyN9F2lMETaeOR0Vo6pZWljCSiiXTfMDw1VqsUAuOieuJvMKfCx1gtpqGwe74IEE7KW903ZF50oKgeBnubq2hC2NgsbZF%2FZg%2F4luwm92RhZXXs8rCL7UYjIp7Vs5mDZsp%2F7navQj4xiC0Nl2zoXcq5U%2FPc7uTphFMcuqiHKFL%2FDHCmKO9CoFU2gZYapZsF7Zo%2FQfM56JGGFwLxKBnZxIfvtY8zAonmDb59BP%2B3N8%5C%22%7D%22%2C%22biz_params%22%3A%22%7B%5C%22vid%5C%22%3A%5C%22XMzY1NDkxODYyNA%3D%3D%5C%22%2C%5C%22current_showid%5C%22%3A%5C%22null%5C%22%7D%22%2C%22ad_params%22%3A%22%7B%5C%22site%5C%22%3A1%2C%5C%22wintype%5C%22%3A%5C%22interior%5C%22%2C%5C%22p%5C%22%3A1%2C%5C%22fu%5C%22%3A0%2C%5C%22vs%5C%22%3A%5C%221.0%5C%22%2C%5C%22rst%5C%22%3A%5C%22mp4%5C%22%2C%5C%22dq%5C%22%3A%5C%22hd2%5C%22%2C%5C%22os%5C%22%3A%5C%22win%5C%22%2C%5C%22osv%5C%22%3A%5C%22%5C%22%2C%5C%22d%5C%22%3A%5C%220%5C%22%2C%5C%22bt%5C%22%3A%5C%22pc%5C%22%2C%5C%22aw%5C%22%3A%5C%22w%5C%22%2C%5C%22needbf%5C%22%3A1%2C%5C%22atm%5C%22%3A%5C%22%5C%22%7D%22%7D' -H 'Accept-Encoding: gzip, deflate, br' -H 'Accept-Language: zh-CN,zh;q=0.9,en-GB;q=0.8,en;q=0.7' -H 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.87 Safari/537.36' -H 'Accept: */*' -H 'Referer: https://v.youku.com/v_show/id_XMzY1NDkxODYyNA==.html?spm=a2h0k.11417342.soresults.dtitle' -H 'Cookie: __ysuid=1527000156228oti; cna=lKSNE+libmkCAR/N6+nV/Zkl; juid=01cf8n7kn2put; __aysid=1528990115134OLI; __aryft=1528992305; __ayft=1529021753757; __ayscnt=1; yseid=1529021755271zC3uH3; yseidcount=3; ycid=0; __arycid=dz-1-00; __arcms=dz-1-00; P_ck_ctl=D1ECFB4B028D9A5A648A13D7A4F67005; referhost=http%3A%2F%2Fso.youku.com; _m_h5_tk=ca7aa941a67ff5f980b2e8758e43b79a_1529037472546; _m_h5_tk_enc=261ba415c461c81044efe9fead3ae61f; isg=BCYmjSrfQZJUKhU3yfiORlk7d5yiLqejG-X0wRDNuskqk8OtVZe60Qwq758fPWLZ; seid=01cg0r1jud2sdg; __ayvstp=102; __aysvstp=143; __arpvid=1529036726009b47f9H-1529036726022; __aypstp=26; __ayspstp=54; seidtimeout=1529038527086; ypvid=1529036729514TrsITN; ysestep=22; yseidtimeout=1529043929515; ystep=41' -H 'Connection: keep-alive' --compressed | python3 -c "$(curl -s https://gist.githubusercontent.com/zmwangx/79f44ea27915a921b9b06e60043a9468/raw/process-youku-appinfo-jsonp)"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 10925    0 10925    0     0   6179      0 --:--:--  0:00:01 --:--:--  6239
Title: 电影鸭【曾志伟】【1080p】【粤语中字】
Available streams:
0: mp4hd3v2 1920x1080
1: 3gphd 480x270
2: mp4hd2v2 1280x720
3: mp4sd 640x360
4: mp4hd 960x540
Saving segment URLs of mp4hd3v2 (1920x1080) to urls.txt...
Downloading segments with GNU Parallel...
Academic tradition requires you to cite works you base your article on.
When using programs that use GNU Parallel to process data for publication
please cite:

  O. Tange (2011): GNU Parallel - The Command-Line Power Tool,
  ;login: The USENIX Magazine, February 2011:42-47.

This helps funding further development; AND IT WON'T COST YOU A CENT.
If you pay 10000 EUR you should feel free to use GNU Parallel without citing.

To silence this citation notice: run 'parallel --citation'.

100% 38:0=0s http://103.38.56.152/6981DB0C8AAC3979C74F794502/03000C26255B1BA0764857042B1E904645FDAF-24A7-413E-A85E-4FC1C513BB17.mp4?ccode=0502&duration=103&expire=18000
Merging downloaded segments with FFmpeg...
ffmpeg version 3.4.2-2 Copyright (c) 2000-2018 the FFmpeg developers
  built with gcc 7 (Ubuntu 7.3.0-16ubuntu2)
  configuration: --prefix=/usr --extra-version=2 --toolchain=hardened --libdir=/usr/lib/x86_64-linux-gnu --incdir=/usr/include/x86_64-linux-gnu --enable-gpl --disable-stripping --enable-avresample --enable-avisynth --enable-gnutls --enable-ladspa --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libmp3lame --enable-libmysofa --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librubberband --enable-librsvg --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libssh --enable-libtheora --enable-libtwolame --enable-libvorbis --enable-libvpx --enable-libwavpack --enable-libwebp --enable-libx265 --enable-libxml2 --enable-libxvid --enable-libzmq --enable-libzvbi --enable-omx --enable-openal --enable-opengl --enable-sdl2 --enable-libdc1394 --enable-libdrm --enable-libiec61883 --enable-chromaprint --enable-frei0r --enable-libopencv --enable-libx264 --enable-shared
  libavutil      55. 78.100 / 55. 78.100
  libavcodec     57.107.100 / 57.107.100
  libavformat    57. 83.100 / 57. 83.100
  libavdevice    57. 10.100 / 57. 10.100
  libavfilter     6.107.100 /  6.107.100
  libavresample   3.  7.  0 /  3.  7.  0
  libswscale      4.  8.100 /  4.  8.100
  libswresample   2.  9.100 /  2.  9.100
  libpostproc    54.  7.100 / 54.  7.100
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc0ffac0] Auto-inserting h264_mp4toannexb bitstream filter
Input #0, concat, from '/mnt/f/i/tmp15hr17yr':
  Duration: N/A, start: -0.046440, bitrate: 2529 kb/s
    Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), yuv420p(tv, bt709), 1920x1080 [SAR 1:1 DAR 16:9], 2401 kb/s, 23 fps, 23 tbr, 11776 tbn, 46 tbc
    Metadata:
      handler_name    : VideoHandler
    Stream #0:1(und): Audio: aac (LC) (mp4a / 0x6134706D), 44100 Hz, stereo, fltp, 128 kb/s
    Metadata:
      handler_name    : SoundHandler
Output #0, mp4, to '电影鸭【曾志伟】【1080p】【粤语中字】.mp4':
  Metadata:
    encoder         : Lavf57.83.100
    Stream #0:0(und): Video: h264 (High) (avc1 / 0x31637661), yuv420p(tv, bt709), 1920x1080 [SAR 1:1 DAR 16:9], q=2-31, 2401 kb/s, 23 fps, 23 tbr, 11776 tbn, 11776 tbc
    Metadata:
      handler_name    : VideoHandler
    Stream #0:1(und): Audio: aac (LC) (mp4a / 0x6134706D), 44100 Hz, stereo, fltp, 128 kb/s
    Metadata:
      handler_name    : SoundHandler
Stream mapping:
  Stream #0:0 -> #0:0 (copy)
  Stream #0:1 -> #0:1 (copy)
Press [q] to stop, [?] for help
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc0ffac0] Auto-inserting h264_mp4toannexb bitstream filter
    Last message repeated 1 times
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter8x
    Last message repeated 2 times
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter822x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter645x
    Last message repeated 2 times
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter678x
    Last message repeated 2 times
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter694x
    Last message repeated 2 times
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter689x
    Last message repeated 1 times
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter683x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter642x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter604x
    Last message repeated 1 times
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter576x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter554x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter527x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter501x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter469x
    Last message repeated 1 times
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter445x
    Last message repeated 1 times
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter438x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter31x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter425x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter421x
    Last message repeated 1 times
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter419x
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x7fffbc837080] Auto-inserting h264_mp4toannexb bitstream filter415x
[mp4 @ 0x7fffbc1464a0] Starting second pass: moving the moov atom to the beginning of the file
frame=117407 fps=3967 q=-1.0 Lsize= 1461303kB time=01:25:08.08 bitrate=2343.5kbits/s speed= 173x
video:1378700kB audio:79228kB subtitle:0kB other streams:0kB global headers:0kB muxing overhead: 0.231473%
Merged into "电影鸭【曾志伟】【1080p】【粤语中字】.mp4".
```

### Linux command tips 
- Switch in cmd.exe to bash `\...\directory path> bash`     (regard as login)

  Switch from bash to cmd: ctrl+D                           (regard as logout)
  
- Change directory `cd /mnt/.../path`

- Return to the default directory `$ cd ~`

- Return to previous directory `$ cd ..`

- Create new folder/file `$ mkdir /mnt/.../foldername` `$ /mnt/.../filename.xxx`

- Mover a file into specific folder `$ mv filename.xxx /mnt/.../path`  

- Text editors
   - `nano text`
   - `vi text`
- sudo apt list --upgradable     # see all upgradable packages' information

  sudo apt list --upgradable -a  # see all upgradable packages' version information
  
  sudo apt update   # check upgrade but not upgrade
  
  sudo apt upgrade  # upgrade installed packages
  
  
<p align="center">
------------------------------------------------------------------ End ------------------------------------------------------------------
</p>
<p align="left">
*Notes for Notes for Ubuntu V2.1.md Update 20180608
</p>  
   
