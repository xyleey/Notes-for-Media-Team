<h1 align="center"><img src="https://github.com/TeamSII/TeamSII.github.io/blob/master/image/NOTES%20copyright.png" width="500" height="400" alt="YESUNG"></h1>

These notes are positively tested as available on the platform of Windows10(x64). They works in both system versions of 1709 and 1803, either HOME or Pro,N or NT. 

Please keep your CLU consistent with the version mentioned in the notes or, apply it in your special demand.

## Index
- [Operation environment](#operation-environment)
- [Tools](#tools)
  - [aria2](#aria2)
  - ['Y's](#ys)
  - [ffmpeg](#ffmpeg)
  - [caterpillar-hls](#caterpillar-hls)
  - [KVM48](#kvm48)

## Operation environment

1.安装[`python3.6.5`](https://www.python.org/downloads/)

2.查看版本`python --version`

3.基于python的库的下载与更新: 

  - `pip list` # 列出所有已安装的库

  - `pip list –outdated` # 列出旧版本的库(即可更新版本)
                            
  - `pip install –-upgrade xxx xxx xxx etc.` # 下载并更新xxx,xxx,xxx,等

#可用[`chocolatey`](https://chocolatey.org/) 进行python/ffmpeg/aria2等的安装

 `choco install -y python ffmpeg aria2 (--upgrade)`
 
 Chocolatey.org 也有 youtube-dl/you-get 的 approved packages,但不确定版本是否最新

## Tools

### aria2

[Aria2](https://aria2.github.io/), a funtional technique to download under multiple threads.

Install `choco install aria2` or download the release
  
Upgrade `choco upgrade aria2` or download the new release

Version `aria2c -v` or `aria2c --version`

Usage   `aria2c -h` or see examples on https://aria2.github.io/

### 'Y's

Install & Upgrade `pip install --upgrade youtube-dl you-get ykdl`

`youtube-dl` 可用于下载Youtube等网页中的视频,也能够解析m3u8地址进行下载
- Usage: 
  - `youtube-dl url`           下载当前视频最高版本
  - `youtube-dl -F url`        查看当前视频不同清晰度对应-f数据
  - `youtube-dl -f 137+78 url` 下载特定视频137和音频78并合成
  - `youtube-dl -f xx -g url`  查找xx对应清晰度的源地址链接

`you-get` 可用于下载斗鱼等网站的视频，同时具有查看当前视频信息和寻找源地址的功能
- Usage: 
   - `you-get url`    直接下载当前网页中的内容，适用斗鱼直播录制
   - `you-get -l url` 同时下载b站多p视频
   - `you-get -i url` 查看当前网页中可下载的视频信息
   - `you-get -u url` 查找当前页面视频的源地址，适用b站及斗鱼直播录制 
        
`ykdl` 功能和用法与上面两个类似，但设计上更加能够应付网络状况较差的直播录制，较少用
 - Usage:
   - `ykdl -i url` 只显示视频信息而不进行下载
   - `ykdl -l url` 将所给的视频url作为播放列表进行下载

### ffmpeg
 
  [`ffmpeg`](http://ffmpeg.org/download.html): 从官网下载ffmpeg for windows后，配置path即可。  
  
  Install `choco install ffmpeg` or configure the downloading package
  
  Upgrade `choco upgrade ffmpeg` or reconfigure the downloading package
  
  Version `ffmpeg -version`

- 下载和录制 (下述url均为actual url即视频源地址)
  
  `ffmpeg -i url -c copy output.ts`    下载视频或直播为视频文件，其文件名为output，封装格式为.ts
  
  `ffmpeg -i url -c copy -y output.ts` 下载视频或直播为视频文件，其文件名为output，封装格式为.ts, 如果文件重复则强制覆盖原有文件
  
- 本地转码
  
  `ffmpeg "file.ts" "file.flv"`
  
- 视频切割 (第一个时间表示剪切起始位置，第二个时间表示output的时长（注意不是剪切结束位置）)
  
  - 基础切割：`ffmpeg -ss 01:31:29 -i input.mp4 -c copy -t 00:13:50 output.mp4`
  
  - 精准切割：`ffmpeg -ss 00:04:18 -t 00:04:34 -accurate_seek -i input -c copy -avoid_negative_ts 1 output`
  
  Ref: http://trac.ffmpeg.org/wiki/Seeking

- 视频无损合并
  
  https://trac.ffmpeg.org/wiki/Concatenate 网站里的 'Concat demuxer' 部分.
  
  - create "concat.txt" with [file inputname.ts/flv/etc] by column.
  
  - `ffmpeg -f concat -i "concat.txt" -c copy "output.ts"`
  
  or you may simply locate to your destination directory, and then run
  
  `ffmpeg -i concat:"file1.ts|file2.ts|file3.ts|file4.ts|file.. .ts" -c copy output.ts`

- 抽取视频/音频流
  
  `ffmpeg -i video -c:v copy -an output`
  
  `ffmpeg -i video -c:a copy -vn output`
  
- 已有视频画面上添加额外的图片/视频
  
  http://ffmpeg.org/ffmpeg-filters.html#overlay-1
  
  `ffmpeg -i input -i logo -filter_complex 'overlay=10:main_h-overlay_h-10' output`
  
  e.g.在狼人杀录屏文件1.mp4(886x1920)中贴近左边界加入一个在[发送弹幕按钮]上方适当位置的水印2.png(300x100)，适用时长为全部(默认)
  
  `ffmpeg -i "1.mp4" -i "2.png" -filter_complex "overlay=10:1635" pix8.mp4`

- 旋转画面

  `ffmpeg -i input -vf "transpose=?" output`

   For transpose=?, 
   0 = 90 Counter Clockwise and Vertical Flip
   1 = 90 Clockwise
   2 = 90 Counter Clockwise
   3 = 90 Clockwise and Vertical Flip

   e.g. I need to turn the canvas of a video 180°, then I should use "transpose=2" twice: 
        `ffmpeg -i input -vf "transpose=2,transpose=2" output`
        
- 推流且在画面添加图片

  Example:
  `ffmpeg -i "20191129SNH48TeamSII六周年庆公演.mp4" -i test.png -filter_complex "overlay=10:25" -vcodec libx264 -pix_fmt yuv420p -acodec aac -b:v 1500k -b:a 320k -r 30 -f flv "$URL+$KEY"`
  
  该例已推流测试成功并且按照想要的位置将图片覆盖在视频上，从而达到在source画面上加logo的目的。
  
  但上述命令重，图片本身尺寸、放置的位置相关命令仍需专业人士优化。推流视频属性的参数等需要按照自己的需求进行详细设定，或使用preset。
  
  *此次测试以b站直播为例，推直播流时`-f flv "$URL+$KEY"`中目标地址格式为：“你的rtmp地址+你的直播码”，+表示二者直接连接，中间没有任何字符
        
- Two-Pass     
     
  Use [`Two-Pass`](https://trac.ffmpeg.org/wiki/Encode/H.264#twopass) if you are targeting a specific output file size, and if output quality from frame to frame is of less importance. For instance, if you need to resize a high-bitrate video to a specified bitrate level whilst you must take the peak bit rate into account, you may use:
  
  `ffmpeg -i input.mp4 -c:v libx264 -b:v 20M -maxrate 21M -bufsize 8M -pass 1 -f mp4 NUL && ffmpeg -i input.mp4 -c:v libx264 -b:v 20M -maxrate 21M -bufsize 8M -pass 2 -y output.mp4`
  
  or basically
  
  `ffmpeg -i input.mp4 -c:v libx264 -b:v 20M -maxrate 21M -bufsize 8M -pass 1 -f mp4 NUL && ^`
  
  `ffmpeg -i input.mp4 -c:v libx264 -b:v 20M -maxrate 21M -bufsize 8M -pass 2 -y output.mp4`
  
where you are able to limit all the instantaneous bitrates under 21,000 kbps and obtain a 20,000 kbps average bitrate as well.

### caterpillar-hls 

  [`caterpillar-hls`](https://github.com/zmwangx/caterpillar) 适用于下载hls下的VODs, i.e.`url.m3u8`

  Install `pip install --upgrade caterpillar-hls`
  
  In case of downloading Koudai 48 VODs, it may report `Error: Non-monotonous DTS in output stream ` while use ffmpeg alone.
  
  Caterpillar will be able to work out such errors in most of cases.
  
  Ultimate features
  
- Concat method
     ```console
     -m {concat_demuxer,concat_protocol,0,1} 不同的concat method，默认使用concat_demuxer，或-m 1切换concat_protocol
       口袋录播文件 -> 默认即可
       公演录播文件 -> 有时以默认模式下载会出现例如 "Application provided duration: 7980637472 / timestamp: 7994129672 is out of range for mov/mp4 format" 的错误，而使用concat_protocol就不会出现这样的问题
     ```
- Batch mode (20180605 update)

     `caterpillar manifest.txt` where the `manifest.txt` contains
     
     ```console
     https://example.com/hls/1.m3u8	1.mp4
     https://example.com/hls/2.m3u8	2.mp4
     https://example.com/hls/3.m3u8	3.mp4
     ```
     
%% Debug
- As you install the newest version of caterpillar-hls, you may see there is one sentence implying a request mismatch
  ```console
  c:\python36\lib\site-packages\requests\__init__.py:80: RequestsDependencyWarning: urllib3 (1.23) or chardet (3.0.4) doesn't match a supported version!
  RequestsDependencyWarning)
  ```
- Run `pip install -U requests certifi chardet idna urllib3`, then we have (in my case)

  ```console
  Collecting pipdeptree
  Downloading https://files.pythonhosted.org/packages/f2/25/cf80fcf4885ab46619f0b7922c1a648c5f1c4186320dc624754fe0130c7a/pipdeptree-0.12.1-py3-none-any.whl
  Collecting pip>=6.0.0 (from pipdeptree)
  Downloading https://files.pythonhosted.org/packages/0f/74/ecd13431bcc456ed390b44c8a6e917c1820365cbebcb6a8974d1cd045ab4/pip-10.0.1-py2.py3-none-any.whl (1.3MB)
    100% |████████████████████████████████| 1.3MB 743kB/s
  Installing collected packages: pip, pipdeptree
    Found existing installation: pip 9.0.3
      Uninstalling pip-9.0.3:
        Successfully uninstalled pip-9.0.3
  Exception:
  Traceback (most recent call last):
    File "c:\python36\lib\shutil.py", line 387, in _rmtree_unsafe 
     os.unlink(fullname)
  PermissionError: [WinError 5] Access is denied: 'C:\\Users\\Yesung\\AppData\\Local\\Temp\\pip-_3qewcai-uninstall\\python36\\scripts\\pip.exe'

  During handling of the above exception, another exception occurred:

  Traceback (most recent call last):
    File "c:\python36\lib\site-packages\pip\basecommand.py", line 215, in main
    File "c:\python36\lib\site-packages\pip\commands\install.py", line 342, in run
    File "c:\python36\lib\site-packages\pip\req\req_set.py", line 795, in install
    File "c:\python36\lib\site-packages\pip\req\req_install.py", line 767, in commit_uninstall
    File "c:\python36\lib\site-packages\pip\req\req_uninstall.py", line 142, in commit
    File "c:\python36\lib\site-packages\pip\_vendor\retrying.py", line 49, in wrapped_f
      return Retrying(*dargs, **dkw).call(f, *args, **kw)
    File "c:\python36\lib\site-packages\pip\_vendor\retrying.py", line 212, in call
      raise attempt.get()
    File "c:\python36\lib\site-packages\pip\_vendor\retrying.py", line 247, in get
      six.reraise(self.value[0], self.value[1], self.value[2])
    File "c:\python36\lib\site-packages\pip\_vendor\six.py", line 686, in reraise
    File "c:\python36\lib\site-packages\pip\_vendor\retrying.py", line 200, in call
      attempt = Attempt(fn(*args, **kwargs), attempt_number, False)
    File "c:\python36\lib\site-packages\pip\utils\__init__.py", line 102, in rmtree
    File "c:\python36\lib\shutil.py", line 494, in rmtree
      return _rmtree_unsafe(path, onerror)
    File "c:\python36\lib\shutil.py", line 384, in _rmtree_unsafe
      _rmtree_unsafe(fullname, onerror)
    File "c:\python36\lib\shutil.py", line 384, in _rmtree_unsafe
      _rmtree_unsafe(fullname, onerror)
    File "c:\python36\lib\shutil.py", line 389, in _rmtree_unsafe
      onerror(os.unlink, fullname, sys.exc_info())
    File "c:\python36\lib\site-packages\pip\utils\__init__.py", line 114, in rmtree_errorhandler
  PermissionError: [WinError 5] Access is denied: 'C:\\Users\\Yesung\\AppData\\Local\\Temp\\pip-_3qewcai-  uninstall\\python36\\scripts\\pip.exe'
  ```
This is because of the incompatible version of urllib3.
- Run `pip install -U pipdeptree && pipdeptree -p requests` as administrator. We can see the warning

  ```console
  Warning!!! Possibly conflicting dependencies found:
  * requests==2.18.4
   - urllib3 [required: >=1.21.1,<1.23, installed: 1.23]
  ------------------------------------------------------------------------
  requests==2.18.4
    - certifi [required: >=2017.4.17, installed: 2018.4.16]
    - chardet [required: >=3.0.2,<3.1.0, installed: 3.0.4]
    - idna [required: <2.7,>=2.5, installed: 2.6]
    - urllib3 [required: >=1.21.1,<1.23, installed: 1.23]
  ```
That's it ! We must have, the urllib3 1.22 for instance.
- Run `pip uninstall -y urllib3 && pip install "urllib3==1.22"`
- Done ! Now you could try `caterpillar -h` to see the options

### KVM48
[KVM48](https://github.com/SNH48Live/KVM48), the Koudai48 VOD Manager. It is capable of downloading all streaming VODs of a set of monitored members in a specified date range. It collaborates with aria2 and caterpillar

Install `pip install KVM48`
Usage   `kvm48 -h`

On windows, you can find the path of `config.yml` printed in the output of `kvm48 -h`. 

Edit this configuration file first before you use it.

- New process mode for url.m3u8 (20180605 update)
  This update allows kvm48 to distinguish all `url.m3u8`. It picks them up and prints them into `m3u8.txt` as it consists to caterpillar's batch mode requirements. Then we run `caterpillar m3u8.txt` to download those VODs concurrently.

<p align="center">
------------------------------------------------------------------ End ------------------------------------------------------------------
</p>
<p align="left">
*Notes for Win10_x64 V2.8.md Update 20191230
</p>  
   

