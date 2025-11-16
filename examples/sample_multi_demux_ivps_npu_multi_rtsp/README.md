## Overview
  This example reads multiple RTSP/MP4 inputs and outputs multiple RTSP streams after running decoding and inference.

## Environment setup
For RTSP server and streaming configuration see: [rtsp-simple-server](../../docs/rtsp.md)

## Quick start
1) Run the following command to read multiple RTSP inputs, decode, run inference, and output multiple RTSP streams:
```
./sample_multi_demux_ivps_npu_multi_rtsp -f rtsp://192.168.31.1:5554/test -f rtsp://192.168.31.1:5554/test2 -p config/yolov5s.json -r 25
```
2) Run the following command to read multiple MP4/H264 files, decode, run inference, and output multiple RTSP streams:
```
./sample_multi_demux_ivps_npu_multi_rtsp -f -f test.mp4 -f test2.mp4 -p config/yolov5s.json -r 25
```