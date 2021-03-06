OpenPose Calibration Module and Demo
=============================================

## Contents
1. [Introduction](#introduction)
2. [Installing the Calibration Module](#installing-the-calibration-module)
3. [Quick Start](#quick-start)



## Introduction
This experimental module performs camera calibration (distortion, intrinsic, and extrinsic camera parameter extraction). It computes and saves the intrinsics parameters of the input images. It is built on top of OpenCV, but aimed to simplify the process for people with no calibration or computer vision background at all (or for lazy people like myself).



## Installing the OpenPose 3-D Reconstruction Module
Install OpenPose following [doc/installation.md](./installation.md#3d-reconstruction-module). In addition, you must enable the `WITH_EIGEN` flag when running CMake. You can either set it to:
    1. `APT_GET` if you download it with `sudo apt-get install libeigen3-dev` (Ubuntu only).
    2. `BUILD` if you want to let OpenPose download it and keep it inside the OpenPose folder (no installation nor extra steps required).



## Quick Start
Note: This example will assume that the target are 3 Flir/Point Grey cameras, but it can be generalized to any camera model.

1. Distortion and intrinsic parameter calibration:
    1. Run OpenPose and save images for your desired camera. Use a grid (chessboard) pattern and move around all the image area. I.e., cover several distances, and within each distance, cover all parts of the image view (all corners and center). You should get at least 100-150 images for a good calibration. Note: we recommend saving the images in PNG format (default behavior) in order to improve calibration quality.
        1. Webcam calibration: `./build/examples/openpose/openpose.bin --num_gpu 0 --write_images {intrinsic_images_folder_path}`.
        2. Flir camera calibration: Add the flags `--flir_camera --flir_camera_index 0` (or the desired flir camera index) to the webcam command.
        3. Calibration from video sequence: Add the flag `--video {video_path}` to the webcam command.
        4. Any other camera brand: Simply save your images in {intrinsic_images_folder_path}, file names are not relevant.
    2. Get familiar with the calibration parameters used in point 3 (i.e., `grid_square_size_mm`, `grid_number_inner_corners`, etc.) by running:
```
./build/examples/calibration/calibration.bin --help
```
    3. Extract and save the intrinsic parameters:
```
./build/examples/calibration/calibration.bin --mode 1 --grid_square_size_mm 40.0 --grid_number_inner_corners "9x5" --camera_serial_number 18079958 --intrinsics_image_dir {intrinsic_images_folder_path}
```
    4. In this case, the intrinsic parameters would have been generated as {intrinsic_images_folder_path}/18079958.xml.
    5. Run steps 1-4 for each one of your cameras.

2. Extrinsic parameter calibration:
    1. We are still implementing this part. Documentation will be available after completing it.



## Using a Different Camera Brand
You can use any camera brand, check [doc/3d_reconstruction_demo.md#using-a-different-camera-brand](./3d_reconstruction_demo.md#using-a-different-camera-brand).
