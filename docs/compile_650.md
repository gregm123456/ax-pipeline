# Source build (AX650N)

**Note**: Please ensure that `ax-pipeline`, `ax650n_bsp_sdk`, and `board_bsp` versions match (for example, the AiXinPai Pro BSP version is 3.6.2). Mismatched versions may cause unexpected errors.

There are two main build paths for ax-samples:

- **Local build**: Build on the development board if it contains a full Linux environment with gcc, cmake, etc.
- **Cross-build**: Build on an x86 PC using the appropriate cross-toolchain for aarch64.

## 1 本地编译(WIP)

### 1.1 Supported hardware boards

- AiXinPai Pro (based on AX650N)

### 1.2 Environment setup

Install the required developer tools on the development board:

```bash
apt update
apt install build-essential libopencv-dev cmake
```

### 1.3 Download source

Clone the repository and enter the `ax-pipeline` root directory:

```shell
git clone https://github.com/AXERA-TECH/ax-pipeline.git
cd ax-pipeline
```

### 1.4 Prepare submodules

```shell
git submodule update --init
wget https://github.com/ivanshi1108/assets/releases/download/3.6.2/ax650n_bsp_sdk.tar.gz
tar xzvf ax650n_bsp_sdk.tar.gz && rm ax650n_bsp_sdk.tar.gz
ln -s /soc/lib/ ax650n_bsp_sdk/msp/out/lib
```

### 1.5 Build source

```shell
mkdir build && cd build
cmake -DAXERA_TARGET_CHIP=AX650 -DBSP_MSP_DIR=$PWD/../ax650n_bsp_sdk/msp/out -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=install ..
make -j$(expr `nproc` - 2)
make install
```

After the build completes, the generated binaries are in `ax-pipeline/build/install/bin/`:

```shell
ax-pipeline/build$ tree install
install
|-- bin
|   |-- config
|   |   |-- clip.json
|   |   |-- crowdcount.json
|   |   |-- custom_model.json
|   |   |-- depth_anything.json
|   |   |-- dinov2.json
|   |   |-- dinov2_depth.json
|   |   |-- glpdepth.json
|   |   |-- owlvit.json
|   |   |-- ppyoloe.json
|   |   |-- scrfd.json
|   |   |-- scrfd_recognition.json
|   |   |-- yolo_nas.json
|   |   |-- yolov5_seg.json
|   |   |-- yolov5s.json
|   |   |-- yolov5s_face.json
|   |   |-- yolov5s_face_recognition.json
|   |   |-- yolov6.json
|   |   |-- yolov7.json
|   |   |-- yolov7_face.json
|   |   |-- yolov8.json
|   |   |-- yolov8_pose.json
|   |   |-- yolov8_seg.json
|   |   `-- yolox.json
|   |-- sample_demux_ivps_npu_hdmi_vo
|   |-- sample_demux_ivps_npu_rtsp
|   |-- sample_demux_ivps_npu_rtsp_hdmi_vo
|   |-- sample_multi_demux_ivps_npu_hdmi_vo
|   |-- sample_multi_demux_ivps_npu_multi_rtsp
|   `-- sample_multi_demux_ivps_npu_multi_rtsp_hdmi_vo
```

## 2 Cross-build

### 2.1 Download source

Clone the repository and enter the `ax-pipeline` root directory:

```shell
git clone https://github.com/AXERA-TECH/ax-pipeline.git
cd ax-pipeline
```

### 2.2 Prepare submodules

```shell
git submodule update --init
wget https://github.com/ivanshi1108/assets/releases/download/3.6.2/ax650n_bsp_sdk.tar.gz
tar xzvf ax650n_bsp_sdk.tar.gz && rm ax650n_bsp_sdk.tar.gz
cd ax650n_bsp_sdk/msp/out/
wget https://github.com/ivanshi1108/assets/releases/download/3.6.2/lib.tar.gz
tar xzvf lib.tar.gz && rm lib.tar.gz
cd ../../../
cd third-party
wget https://github.com/ivanshi1108/assets/releases/download/3.6.2/drm.tar.gz
tar xzvf drm.tar.gz && rm drm.tar.gz
wget https://github.com/ivanshi1108/assets/releases/download/3.6.2/libexif.tar.gz
tar xzvf libexif.tar.gz && rm libexif.tar.gz
cd ../
```

### 2.3 Create `3rdparty` and download OpenCV

```shell
mkdir 3rdparty && cd 3rdparty
wget https://github.com/ivanshi1108/assets/releases/download/3.6.2/opencv_4.5.5.tar.gz
tar xzvf opencv_4.5.5.tar.gz && rm opencv_4.5.5.tar.gz
cd ../
```

### 2.4 Build environment

- cmake >= 3.13
- Make sure you have the cross-toolchain configured; example command shown below

```shell
wget https://developer.arm.com/-/media/Files/downloads/gnu-a/9.2-2019.12/binrel/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu.tar.xz
tar -xvf gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu.tar.xz
export PATH=$PATH:$PWD/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/bin/
```

### 2.5 Build source

```shell
mkdir build && cd build
cmake -DAXERA_TARGET_CHIP=AX650 -DBSP_MSP_DIR=$PWD/../ax650n_bsp_sdk/msp/out -DOpenCV_DIR=$PWD/../3rdparty/opencv_4.5.5/lib/cmake/opencv4 -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE=../toolchains/aarch64-none-linux-gnu.toolchain.cmake -DCMAKE_INSTALL_PREFIX=install ..
make -j$(expr `nproc` - 2)
make install
```

After the build completes, the generated binaries are in `ax-pipeline/build/install/bin/`:

```shell
ax-pipeline/build$ tree install
install
|-- bin
|   |-- config
|   |   |-- clip.json
|   |   |-- crowdcount.json
|   |   |-- custom_model.json
|   |   |-- depth_anything.json
|   |   |-- dinov2.json
|   |   |-- dinov2_depth.json
|   |   |-- glpdepth.json
|   |   |-- owlvit.json
|   |   |-- ppyoloe.json
|   |   |-- scrfd.json
|   |   |-- scrfd_recognition.json
|   |   |-- yolo_nas.json
|   |   |-- yolov5_seg.json
|   |   |-- yolov5s.json
|   |   |-- yolov5s_face.json
|   |   |-- yolov5s_face_recognition.json
|   |   |-- yolov6.json
|   |   |-- yolov7.json
|   |   |-- yolov7_face.json
|   |   |-- yolov8.json
|   |   |-- yolov8_pose.json
|   |   |-- yolov8_seg.json
|   |   `-- yolox.json
|   |-- sample_demux_ivps_npu_hdmi_vo
|   |-- sample_demux_ivps_npu_rtsp
|   |-- sample_demux_ivps_npu_rtsp_hdmi_vo
|   |-- sample_multi_demux_ivps_npu_hdmi_vo
|   |-- sample_multi_demux_ivps_npu_multi_rtsp
|   `-- sample_multi_demux_ivps_npu_multi_rtsp_hdmi_vo
```
