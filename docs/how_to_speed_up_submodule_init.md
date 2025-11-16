# How to speed up submodule downloads
Because the repository includes large submodules, some users experience slow downloads. This short guide shows a quick trick to speed up submodule downloads in environments with limited network access.

### Free download acceleration
1) Open https://ghproxy.com/

2) As shown on the website, add the prefix `https://ghproxy.com/` to Git URLs to speed up downloads.

For example:
```bash
git clone https://github.com/sipeed/axpi_bsp_sdk.git
```
Change the command to:
```bash
git clone https://ghproxy.com/https://github.com/sipeed/axpi_bsp_sdk.git
```

### Update ```.gitmodules```
1) Open the repository's `.gitmodules` file with a text editor.
```
[submodule "axpi_bsp_sdk"]
	path = axpi_bsp_sdk
	url = https://github.com/sipeed/axpi_bsp_sdk.git
[submodule "third-party/libv4l2cpp"]
	path = third-party/libv4l2cpp
	url = https://github.com/mpromonet/libv4l2cpp.git
[submodule "third-party/RTSP"]
	path = third-party/RTSP
	url = https://github.com/ZHEQIUSHUI/RTSP.git
```
2) Add `https://ghproxy.com/` before each Git URL in the `url` field. The result should look like:
```
[submodule "axpi_bsp_sdk"]
	path = axpi_bsp_sdk
	url = https://ghproxy.com/https://github.com/sipeed/axpi_bsp_sdk.git
[submodule "third-party/libv4l2cpp"]
	path = third-party/libv4l2cpp
	url = https://ghproxy.com/https://github.com/mpromonet/libv4l2cpp.git
[submodule "third-party/RTSP"]
	path = third-party/RTSP
	url = https://ghproxy.com/https://github.com/ZHEQIUSHUI/RTSP.git
```
