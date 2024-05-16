# 监控视频自动合并工具

这是一个基于 .net core / C# 的监控视频自动合并工具，基于 **ffmpeg** 进行视频无损合并。

原始创意是技术交流群里一个老色批的个人需求，因为现在的监控（如小米监控摄像头）支持把视频全都自动上传到NAS中，但是上传的视频都是分段的，数量众多且难以维护，因此他在寻求这样一个工具，可以自动将所有的分段视频按照月份进行合并成一个视频。

在尝试了用批处理、Shell脚本以及AI写程序之后，他折腾了不少时间还是没成功完成这样的需求，因此这样一个简单的工具就出现了。

## 支持的视频文件名格式

目前适配的是小米摄像头生成的监控视频片段，支持以下两种文件名类型的视频片段：

- `20240320192213_20240320193309.mp4`
- `43M19S_1705916599.mp4`

## 支持的功能

- 自动搜索MP4视频文件，支持按时间自动分组/排序
- 支持按月合并或按日合并
- 支持自定义输出文件名
- 支持两种监控录像文件名格式
- 如果已部分合并过，支持追加合并
- 支持自动重命名或删除已合并文件
- 视频&音频无损合并，自动检测音频不兼容并转码

## 用法

从源码编译出可执行文件后运行，或您可以选择从[博客](https://blog.iccfish.com/?p=5591)中下载已经预编译的可执行文件。

命令行格式：

**Windows**
```
.\VideoRecordingJoiner.exe <选项> 要合并的视频目录或文件，可以支持多个
```

**Linux**
```
./VideoRecordingJoiner <选项> 要合并的视频目录或文件，可以支持多个
```

> 软件基于 `ffmpeg` ，因此您的系统中需要已安装相关包，或当前软件目录下可找到它的可执行文件。

支持的命令行参数：
```
  -o    <路径>  指定输出目录; 如果不指定，将默认输出当前运行目录下
  -d            指定合并后删除源文件，如果不指定此选项，则会修改源文件名（防止重复合并）
  -f    <格式>  输出文件名格式，不包含扩展名，支持 yyyy、MM、dd占位符，如 "yyyy-MM\dd"
  -t    <类型>  输出文件类型，可选 mkv 或 mp4，默认为 mkv，建议使用 mkv
  -gm           按月合并（默认为按日合并）

注意：
  - 按月合并模式下，默认输出文件名模板为 【yyyy-MM】，可以使用 -f 选项覆盖
  - 按日合并模式下，默认输出文件名模板为 【yyyy-MM\dd】，可以使用 -f 选项覆盖
```

输出目录可手动指定，如果不指定，则默认在当前目录下。

源目录将会自动搜索子目录。

输出格式建议用 mkv，因为mp4已经发现部分情况下音频编码会存在不兼容的情况，需要进行格式转换，导致速度降低。