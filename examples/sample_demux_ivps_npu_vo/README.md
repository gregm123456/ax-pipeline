## Overview
  This example shows how to build a pipeline that accepts RTSP/MP4 input and splits two decoded streams for display and inference.

## Flowchart
![](../../docs/sample_demux_ivps_joint_vo.png)

## Quick start
```bash
Usage:./sample_demux_ivps_joint_vo -h for help

        -p: model config file path
        -f: mp4 file(just only support h264 format)
        -l: loop play video (loop playback)
```

```
./sample_demux_ivps_joint_vo -f xxx.mp4 -l 1 -p config/yolov5s.json
```
