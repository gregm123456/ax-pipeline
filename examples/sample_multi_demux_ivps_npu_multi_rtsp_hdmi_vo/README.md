## Overview
  This example reads multiple RTSP/MP4 inputs and outputs to RTSP servers and HDMI displays, demonstrating an RTSP-in/RTSP-out pipeline with HDMI display.

## Environment setup
For RTSP server and streaming configuration see: [rtsp-simple-server](../../docs/rtsp.md)

## Quick start
1) Run the following command to read multiple RTSP streams, decode, infer, and output to HDMI and multiple RTSP streams.
```
./sample_multi_demux_ivps_npu_multi_rtsp_hdmi_vo -f rtsp://192.168.31.1:5554/test -f rtsp://192.168.31.1:5554/test2 -p config/yolov5s.json
```

2) Run the following command to demux, decode, infer on multiple mp4/h264 files, and output to HDMI and RTSP streams.
```
./sample_multi_demux_ivps_npu_multi_rtsp_hdmi_vo -f test.mp4 -f test2.mp4 -p config/yolov5s.json
```