## Overview
  This example takes RTSP/MP4 input and outputs to both screen display and an RTSP media server, demonstrating an RTSP-in/RTSP-out pipeline.

## Flowchart
![](../../docs/sample_demux_ivps_joint_rtsp_vo.png)


## Quick start
1) Prepare an H264 binary file or an MP4 with H264 video that you want to stream; the example below uses `test.h264`.

2) Download the appropriate `rtsp-simple-server` executable from the releases: https://github.com/aler9/rtsp-simple-server/releases/tag/v0.21.0 and run it in the background. Note: the default RTSP server port is 8554; you may need to update `rtsp-simple-server.yml` to avoid port conflicts (we use 5554 in this example).

3) Install ffmpeg and stream test.h264 using the following command
```
ffmpeg -re -stream_loop -1 -i test.h264 -rtsp_transport tcp -c copy -f rtsp rtsp://localhost:5554/test
```

4) Assume the host IP where `rtsp-simple-server` runs is `192.168.31.1` and the AiXinPai device is on the same LAN.

5) Run the following command to create the RTSP pull-decode-infer pipeline:
```
./sample_demux_ivps_joint_rtsp_vo -f rtsp://192.168.31.1:5554/test -p config/yolov5s.json
```