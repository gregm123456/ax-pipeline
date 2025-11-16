## Overview
  This example reads multiple RTSP/MP4 inputs and outputs HDMI displays after running decoding and inference.


## Quick start
1) Run the following command to read multiple RTSP streams, decode, run inference, and display results on HDMI:
```
./sample_multi_demux_ivps_npu_hdmi_vo -f rtsp://192.168.31.1:5554/test -f rtsp://192.168.31.1:5554/test2 -p config/yolov5s.json
```
2) Run the following command to read multiple MP4/H264 files, decode, run inference, and display results on HDMI:
```
./sample_multi_demux_ivps_npu_hdmi_vo -f test.mp4 -f test2.mp4 -p config/yolov5s.json
```