# Source Build (AX620E)

There are currently two build paths for ax-samples:

- **Local build**: Build directly on the development board if it contains a full Linux environment with gcc, cmake, and other necessary dev tools.
- **Cross-build**: Build on an x86 PC using the aarch64 cross-toolchain and then deploy the binaries to the target device.

## 1 Local build

AX630C and AX620Q devices typically do not include a full Linux development environment, so local builds are not covered here.

## 2 Cross-build

### 2.1 Download source

Clone the repository and enter the `ax-pipeline` root directory:

```shell
git clone https://github.com/AXERA-TECH/ax-pipeline.git
cd ax-pipeline
```

### 2.2 Prepare submodules

Please contact FAE to obtain the AX620E BSP package. Extract the SDK to the `ax-pipeline` directory and name it `ax620e_bsp_sdk`.

```shell
git submodule update --init
cd ax620e_bsp_sdk
wget https://github.com/ZHEQIUSHUI/assets/releases/download/ax650/drm.zip
mkdir third-party
unzip drm.zip -d third-party
cd ..
```

### 2.3 Create 3rdparty and download OpenCV

```shell
mkdir 3rdparty
cd 3rdparty
wget https://github.com/ZHEQIUSHUI/assets/releases/download/ax650/libopencv-4.5.5-aarch64.zip
unzip libopencv-4.5.5-aarch64.zip
cd ..
```

### 2.4 Build environment

- cmake >= 3.13
- Make sure your cross-toolchain is configured. If not, use the following example:

```shell
wget https://developer.arm.com/-/media/Files/downloads/gnu-a/9.2-2019.12/binrel/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu.tar.xz
tar -xvf gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu.tar.xz
export PATH=$PATH:$PWD/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/bin/
```

### 2.5 Build source

```shell
cd ..
mkdir build
cd build
cmake -DAXERA_TARGET_CHIP=AX620e -DBSP_MSP_DIR=$PWD/../ax620e_bsp_sdk/msp/out -DOpenCV_DIR=$PWD/../3rdparty/libopencv-4.5.5-aarch64/lib/cmake/opencv4 -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE=../toolchains/aarch64-none-linux-gnu.toolchain.cmake -DCMAKE_INSTALL_PREFIX=install ..
make $(expr `nproc` - 1)
make install
```

After the build completes, the generated binaries are in `ax-pipeline/build/install/bin/`:

```shell
ax-pipeline/build$ tree install
install
├── bin
│   ├── config
│   │   ├── dinov2.json
│   │   ├── scrfd.json
│   │   ├── yolov5_seg.json
│   │   ├── yolov5s_650.json
│   │   ├── yolov5s_face.json
│   │   ├── yolov6.json
│   │   ├── yolov7.json
│   │   ├── yolov7_face.json
│   │   ├── yolov8_pose.json
│   │   └── yolox.json
│   ├── sample_demux_ivps_joint_hdmi_vo
│   ├── sample_demux_ivps_joint_rtsp
│   ├── sample_demux_ivps_joint_rtsp_hdmi_vo
│   ├── sample_multi_demux_ivps_joint_hdmi_vo
│   ├── sample_multi_demux_ivps_joint_multi_rtsp
│   └── sample_multi_demux_ivps_joint_multi_rtsp_hdmi_vo
```
