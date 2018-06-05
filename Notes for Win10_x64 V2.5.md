<h1 align="center"><img src="https://github.com/TeamSII/TeamSII.github.io/blob/master/image/NOTES%20copyright.png" width="500" height="400" alt="YESUNG"></h1>

These notes are positively tested as available on the platform of Windows10(x64). They works in both system versions of 1709 and 1803, either HOME or Pro,N or NT. 

Please keep your CLU consistent with the version mentioned in the notes or, apply it in your special demand.

## Index
- [Operation environment](#operation-environment)
- [General usage](#general-usage)
  - ['Y's](#ys)
  - [ffmpeg](#ffmpeg)
  - [caterpillar](#caterpillar)

## Operation environment

1.安装[`python3.6.5`](https://www.python.org/downloads/)

2.查看版本`python --version`

3.基于python的库的下载与更新: 

  - `pip list` # 列出所有已安装的库

  - `pip list –outdated` # 列出旧版本的库(即可更新版本)
                            
  - `pip install –-upgrade xxx xxx xxx etc.` # 下载并更新xxx,xxx,xxx,等
                            
5.'Y's: `youtube-dl`, `you-get`(&`lulu`), `ykdl`是基于python的三个命令行下载工具

   安装与更新: `pip install –-upgrade youtube-dl you-get ykdl lulu`
   
   * 定期运行保证三件套都 up to date
   

#可用[`chocolatey`](https://chocolatey.org/) 进行python/ffmpeg/aria2等的安装

 `choco install -y python ffmpeg aria2 (--upgrade)`
 
 Chocolatey.org 也有 youtube-dl/you-get 的 approved packages,但不确定版本是否最新

## General usage

### 'Y's

`youtube-dl` 可用于下载Youtube等网页中的视频,也能够解析m3u8地址进行下载
 Usage: 
  - `youtube-dl url`          下载当前视频最高版本
  - `youtube-dl -F url`       查看当前视频不同清晰度对应-f数据   
  - `youtube-dl -f xx -g url` 查找xx对应清晰度的源地址链接

`you-get` 可用于下载斗鱼、b站、优酷等网站的视频，同时具有查看当前视频信息和寻找源地址的功能
 Usage: 
   - `you-get url`    直接下载当前网页中的内容，适用斗鱼直播录制
   - `you-get -l url` 同时下载b站多p视频
   - `you-get -i url` 查看当前网页中可下载的视频信息
   - `you-get -u url` 查找当前页面视频的源地址，适用b站直播录制 
         
*`lulu` 实质为you-get的分支，下载b站视频能力比you-get稳定一些
  Usage:
   - `lulu weburl`
   - `lulu -h`     see all options

`ykdl` 功能和用法与上面两个类似，但设计上更加能够应付网络状况较差的直播录制，较少用
  Usage:
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


- 抽取视频/音频流
  
  `ffmpeg -i video -c:v copy -an output`
  
  `ffmpeg -i video -c:a copy -vn output`
  
  
- 已有视频画面上添加额外的图片/视频
  
  http://ffmpeg.org/ffmpeg-filters.html#overlay-1
  
  `ffmpeg -i input -i logo -filter_complex 'overlay=10:main_h-overlay_h-10' output`
  
  e.g.在狼人杀录屏文件1.mp4(886x1920)中贴近左边界加入一个在[发送弹幕按钮]上方适当位置的水印2.png(300x100)，适用时长为全部(默认)
  
  `ffmpeg -i "1.mp4" -i "2.png" -filter_complex "overlay=10:1635" pix8.mp4`

### caterpillar 

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
     caterpillar `manidest.txt` which contains:
     ```console
     https://example.com/hls/1.m3u8	1.mp4
     https://example.com/hls/2.m3u8	2.mp4
     https://example.com/hls/3.m3u8	3.mp4
     ```
   <p></p><p></p>  
   
*Notes for Win10_x64 V2.5.md Update 20180605