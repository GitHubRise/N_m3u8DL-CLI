# N_m3u8DL-CLI
一个m3u8下载器
## 基本用法
双击exe，然后输入m3u8链接或拖入m3u8文件或拖入本程序生成的json文件按下回车键。  
![录制](https://i.loli.net/2019/06/11/5cff1dbc4e32955427.gif)

## 高级用法
使用命令行参数
```
N_m3u8DL-CLI.exe <URL|JSON> [OPTIONS]  

    --workDir    Directory  设定程序工作目录
    --saveName   Filename   设定存储文件名(不包括后缀)
    --baseUrl    BaseUrl    设定Baseurl
    --headers    headers    设定请求头，格式 key:value 使用|分割不同的key&value
    --maxThreads Thread     设定程序的最大线程数(默认为32)
    --minThreads Thread     设定程序的最小线程数(默认为16)
    --retryCount Count      设定程序的重试次数(默认为15)
    --timeOut    Sec        设定程序网络请求的超时时间(单位为秒，默认为10秒)
    --muxSetJson File       使用外部json文件定义混流选项
    --enableDelAfterDone    开启下载后删除临时文件夹的功能
    --enableMuxFastStart    开启混流mp4的FastStart特性
    --enableBinaryMerge     开启二进制合并分片
    --enableParseOnly       开启仅解析模式(程序只进行到meta.json)
    --disableDateInfo       关闭混流中的日期写入

```
附录：一个典型的混流Json文件应该长这个样子：
```
    {
        "muxFormat": "mp4", 
        "fastStart": "True", 
        "poster": "文件路径，\需写为\\", 
        "audioName": "", 
        "title": "", 
        "copyright": "", 
        "comment": ""
    }
```
## SimpleG附加说明  
这是程序临时的用户界面程序  
![录制](https://i.loli.net/2019/06/11/5cff11b74dcba62351.gif)


在输入m3u8链接后，双击“名字”的输入框会尝试自动获取视频标题，目前仅支持腾讯、爱奇艺、优酷视频。

URL输入框可接受txt文件路径或文件夹拖入以进行批量下载：
txt文件格式为每行一个m3u8地址；
文件夹内存在若干m3u8文件。
## JS获取腾讯视频、优酷m3u8
腾讯视频
```
javascript:prompt(videoPlayer.getData()._videoData.title,Array.reverse(Array.from(videoPlayer.getData()._playlistData.stream))[0].m3u8_url);
```  
优酷
```
javascript:var url;var size=0;Array.from(videoPlayer.getData()._playlistData.stream).forEach(function(element,index,array){if(element.audio_lang==videoPlayer.getConfig().language&&element.size>size){url=element.m3u8_url;size=element.size}});/*nilaoda*/prompt(videoPlayer.getData()._videoData.title+"_"+videoPlayer.getConfig().language+"_"+(size/1024/1024).toFixed(2)+"MB",url);
``` 
爱奇艺
```
javascript:var info=playerObject._player._core._movieinfo.originalData.data.program.video;info.forEach(function(item,index){if(item._selected){var blob=new Blob([item.m3u8],{type:"text/plain"});var url=URL.createObjectURL(blob);var title=(document.title.indexOf('-')!=-1?document.title.substring(0,document.title.indexOf('-')):document.title.replace(/\s/,''))+"_"+item.scrsz+"_"+(item.code==2?"H264":"H265")+"_"+document.getElementsByClassName("iqp-time-dur")[0].innerText.replace(/:/,".")+"_"+(item.vsize/1024/1024).toFixed(2)+"MB.m3u8";/*nilaoda*/var aLink=document.createElement("a");aLink.href=url;aLink.download=title;aLink.style.display="none";var event;if(window.MouseEvent){event=new MouseEvent("click")}else{event=document.createEvent("MouseEvents");event.initMouseEvent("click",true,false,window,0,0,0,0,0,false,false,false,false,0,null)}aLink.dispatchEvent(event)}});
```
芒果(需要填写请求头`Referer:https://player.mgtv.com/mgtv_v6_player/PlayerCore.swf`)
```
javascript:prompt(MGTVPlayer.VIDEOINFO.title,MGTVPlayer.player.cms.sourceInfo.info);
```
