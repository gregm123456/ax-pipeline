## Overview
  This example shows how to build a media pipeline that accepts RTSP/MP4 input and performs decoding, inference, and RTSP output.

## Flowchart
![](../../docs/sample_demux_ivps_joint_rtsp.png)

## Environment setup
For RTSP server and streaming configuration see: [rtsp-simple-server](../../docs/rtsp.md)

## Quick start
1) Run the following command to demux, decode, and infer on an mp4/h264 file and output results via RTSP stream.
```
./sample_demux_ivps_npu_rtsp -f test.mp4 -l 1 -p config/yolov5s.json -r 25
```
2) Run the following command to pull, decode, and infer from an RTSP input and output results via RTSP stream.
```
./sample_demux_ivps_npu_rtsp -f rtsp://192.168.31.1:8554/test -l 1 -p config/yolov5s.json -r 25
```
