## Overview
  This example reads RTSP/MP4 input and performs inference, outputting results to HDMI display.

## Flowchart
![](../../docs/sample_demux_ivps_joint_hdmi_vo.png)

## Environment setup
For RTSP server and streaming configuration see: [rtsp-simple-server](../../docs/rtsp.md)

## Quick start
1) Run the following command to read an RTSP stream, decode, run inference, and output results to HDMI:
```
./sample_demux_ivps_npu_hdmi_vo -f rtsp://192.168.31.1:8554/test -p config/yolov5s.json
```

2) Run the following command to read an MP4/H264 file, decode, run inference, and output results to HDMI:
```
./sample_demux_ivps_npu_hdmi_vo -f test.h264 -p config/yolov5s.json
```

3) Run the following command to decode an MP4/H264 file and run multiple different models simultaneously, and display tiled output on the HDMI screen:
```
./sample_demux_ivps_npu_hdmi_vo -f test.h264 -p config/yolov5s.json -p config/yolov8_pose.json -p config/yolox.json
```