## Overview
  This example shows a pipeline that decodes RTSP/MP4 input, runs inference, and outputs results both on HDMI display and as an RTSP stream.

## Flowchart
![](../../docs/sample_demux_ivps_joint_rtsp_vo.png)

## Environment setup
For RTSP server and streaming configuration see: [rtsp-simple-server](../../docs/rtsp.md)

## Quick start
1) Run the following command to read RTSP streams, decode, run inference, and output results to both HDMI display and RTSP stream.
```
./sample_demux_ivps_npu_rtsp_hdmi_vo -f rtsp://192.168.31.1:8554/test -p config/yolov5s.json
```
2) Run the following command to read MP4/H264 files, decode, run inference, and output results to both HDMI display and RTSP stream.
```
./sample_demux_ivps_npu_rtsp_hdmi_vo -f rtsp://192.168.31.1:8554/test -p config/yolov5s.json
```