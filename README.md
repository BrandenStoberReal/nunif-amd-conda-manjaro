## AMD ROCm 6.1 Instructions w/ Anaconda3 on Arch Linux

### 1. Install Anaconda3
https://www.anaconda.com/download/success

```
wget https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh
chmod +x Anaconda3-2024.10-1-Linux-x86_64.sh
./Anaconda3-2024.10-1-Linux-x86_64.sh
```

### 2. Clone repo
I personally use the 'dev' branch but 'master' should work fine aswell.

```
git clone https://github.com/nagadomi/nunif.git
cd nunif
```

If you want to use the `dev` branch, execute the following command.
```
git clone https://github.com/nagadomi/nunif.git -b dev
```
or
```
git fetch --all
git checkout -b dev origin/dev
```

### 3. Create conda env
Python 3.12 was confirmed to work by me, but any compatible version is fine.
```
cd nunif
conda create -n waifu2x -c conda-forge python=3.12
```

### 4. Install pytorch conda dependencies
These conda packages resolve Python issues with linking libraries at runtime.
```
conda activate waifu2x
conda install -c conda-forge gxx_linux-64 pillow
```

### 5. Install waifu2x dependencies
```
pip3 install -r requirements-torch-rocm.txt
pip3 install -r requirements.txt
pip3 install wxpython
```

### 6. Install models
```
python3 -m waifu2x.download_models
python -m iw3.download_models
```

### 7. Run Waifu2x under gfx 1030 HIP runtime
NOTE: Even if you have an AMD card above gfx1030, we are forcing the 1030 runtime due to PyTorch ROCm issues. Any GPU lower than gfx1030 chipset is untested and likely won't work.

This resolves the "HIP: Invalid device function" error.
```
export HSA_OVERRIDE_GFX_VERSION=10.3.0 && python3 -m waifu2x.gui
```

## waifu2x

[waifu2x/README.md](./waifu2x/README.md)

waifu2x: Image Super-Resolution for Anime-Style Art. Also it supports photo models (GAN based models)

The repository contains waifu2x pytorch implementation and pretrained models, started with porting the original [waifu2x](https://github.com/nagadomi/waifu2x).

The demo application can be found at
- https://waifu2x.udp.jp/ (Cloud version)
- https://unlimited.waifu2x.net/ (In-Browser version).

## iw3

[iw3/README.md](./iw3/README.md)

I want to watch any 2D video as 3D video on my VR device, so I developed this very personal tool.

iw3 provides the ability to convert any 2D image/video into side-by-side 3D image/video.

### iw3-desktop

[iw3/docs/desktop.md](./iw3/docs/desktop.md)

iw3.desktop is a tool that converts your PC desktop screen into 3D and streaming over WiFi.

You can watch any image and video/live displayed on your PC as 3D in realtime.

## stilizer

[stlizer/README.md](./stlizer/README.md)

stlizer is a fast conservative video stabilizer.

## cliqa

[cliqa/README.md](./cliqa/README.md)

`cliqa` provides low-vision image quality scores that are more consistent across different images.

It is useful for filtering low-quality images with a threshold value when creating image datasets.

Currently, the following two models are supported.

- JPEGQuality: Predicts JPEG Quality from image content
- GrainNoiseLeve: Predicts Noise Level related to photograph and PSNR degraded by that noise

CLI tools are also available to filter out low quality images using these results.

## Install

### Installer for Windows users

- [nunif windows package](windows_package/docs/README.md)
- [nunif windows package (日本語)](windows_package/docs/README_ja.md)

### For developers

#### Dependencies

- Python 3 (Works with Python 3.10 or later, developed with 3.10)
- [PyTorch](https://pytorch.org/get-started/locally/)
- See requirements.txt

We usually support the latest version. If there are bugs or compatibility issues, we will specify the version.

- [INSTALL-ubuntu](INSTALL-ubuntu.md)
- [INSTALL-windows](INSTALL-windows.md)
- [INSTALL-macos](INSTALL-macos.md)


### License Notes

Note that if you distribute binary builds, it is possible that it will be GPL.

This is due to PyAV(av) wheel package containing the GPL version of ffmpeg library.
You can build PyAV with the LGPL version of ffmpeg library.

If you load this repository with torch.hub.load for waifu2x Python API etc, this problem does not exist because PyAV is not a dependent package.
