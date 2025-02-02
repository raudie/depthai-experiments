## Background on the DepthAI Gen2 Pipeline Builder Examples

The Gen2 Pipeline Builder (luxonis/depthai#136) allows theoretically infinite permutations of pipelines to be built. 
So there is no way to do what we did with depthai_demo.py as in Gen1 where effectively all permutations of configs can be done via CLI options.

So for Gen2 it is necessary to show how the fundamental nodes can be used in easy, and simple ways. 

This way, the pipeline consisting of multiple nodes can be easily built for a given application by piecing together these simple/clear examples.

Allowing the power of these permutations, but with the speed of copy/paste plugging together of examples.

So the below examples hope to serve to show how to use each of these nodes, so that you can piece them together for your application.

## Install Dependencies

After having [installed development tools on your system](https://docs.luxonis.com/en/latest/pages/api/#installing-system-dependencies), and prior to running the examples below, install the dependencies for them with:

`python3 -m pip install -r requirements.txt`

This installs the depthai library version that corresponds to the examples, to ensure compatibility.  

## Known issues

As of now, these examples do not show additional capabilities of depth processing that depthai/OAK-D are capable of, such as subpixel, LR-check, and extended disparity, as there is an instability we are tracking down when using these additional features with the `manip` node and/or the `neural-network` node.  To try subpixel, LR-check, and extended disparity independently, see the Subpixel and LR-Check Disparity Depth example, [here](https://github.com/luxonis/depthai-experiments#gen2-subpixel-and-lr-check-disparity-depth-here).

## The examples:

Note that the encoding examples will save encoded video to your host storage.  Be more careful with these...  If you leave them running, you could fill up your storage on your host.

### [01_rgb_preview.py](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/01_rgb_preview.py)

To run, issue `python3 01_rgb_preview.py` after having installed the dependencies above.

This example shows how to set up a pipeline that outpus a small preview of the RGB camera, connects over XLink to transfer these to the host real-time, and displays the RGB frames on the host with OpenCV.

![image](https://user-images.githubusercontent.com/32992551/103940730-0ff23400-50eb-11eb-8620-aabf743089a6.png)

### [02_mono_preview.py](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/02_mono_preview.py)

This example shows how to set up a pipeline that outputs the left and right grayscale camera images, connects over XLink to transfer these to the host real-time, and displays both using OpenCV.

![image](https://user-images.githubusercontent.com/32992551/103947827-fa363c00-50f5-11eb-96eb-537912334c3a.png)

### [03_depth_preview.py](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/03_depth_preview.py)

This example shows how to set the SGBM (semi-global-matching) disparity-depth node, connects over XLink to transfer the results to the host real-time, and displays the depth map in OpenCV.

![image](https://user-images.githubusercontent.com/32992551/103947842-fe625980-50f5-11eb-9c99-58d903bba331.png)

### [04_rgb_encoding.py](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/04_rgb_encoding.py)

This example shows how to configure the depthai video encoder in h.265 format to encode the RGB camera input at 8MP/4K/2160p (3840x2160) at 30FPS (the maximum possible encoding resolultion possible for the encoder, higher frame-rates are possible at lower resolutions, like 1440p at 60FPS), and transfers the encoded video over XLINK to the host, saving it to disk as a video file.

Pressing Ctrl+C will stop the recording and then convert it using ffmpeg into an mp4 to make it playable.  Note that ffmpeg will need to be installed and runnable for the conversion to mp4 to succeed.

### [05_rgb_mono_encoding.py](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/05_rgb_mono_encoding.py)

This example shows how to set up the encoder node to encode the RGB camera and both grayscale cameras (of DepthAI/OAK-D) at the same time.  The RGB is set to 1920x1080 and the grayscale are set to 1280x720 each, all at 30FPS.  Each encoded video stream is transferred over XLINK and saved to a respective file.

Pressing Ctrl+C will stop the recording and then convert it using ffmpeg into an mp4 to make it playable.  Note that ffmpeg will need to be installed and runnable for the conversion to mp4 to succeed.

### [06_rgb_full_resolution_saver.py](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/06_rgb_full_resolution_saver.py)

This example does its best to save 3840x2160 .png files as fast at it can from the RGB sensor.  It serves as an example of recording high resolution to disk for the purposes of high-resolution ground-truth data.  We also recently added the options to save isp - YUV420p uncompressed frames, processed by ISP, and raw - BayerRG (R_Gr_Gb_B), as read from sensor, 10-bit packed.  See [here](https://github.com/luxonis/depthai-experiments/pull/29) for the pull request on this capability.

![16100554427418](https://user-images.githubusercontent.com/32992551/103947807-f0143d80-50f5-11eb-8604-c25f329306de.png)


### [07_mono_full_resolution_saver.py ](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/07_mono_full_resolution_saver.py)

This example shows how to save 1280x720p .png of the left grayscale camera to disk.  Left is defined as from the boards perspective.

![16100555388158](https://user-images.githubusercontent.com/32992551/103947946-28b41700-50f6-11eb-9688-530d0d64c1f4.png)

### [08_rgb_mobilenet.py ](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/08_rgb_mobilenet.py)

This example shows how to MobileNetv2SSD on the RGB input frame, and how to display both the RGB preview and the metadata results from the MobileNetv2SSD on the preview.

![image](https://user-images.githubusercontent.com/32992551/103948433-ff47bb00-50f6-11eb-9cb9-b959d2dce63c.png)

### [09_mono_mobilenet.py](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/09_mono_mobilenet.py)

This example shows how to run MobileNetv2SSD on the left grayscale camera and how to display the neural network results on a preview of the left camera stream.

![image](https://user-images.githubusercontent.com/32992551/103948468-0cfd4080-50f7-11eb-9ea8-beafce001265.png)

### [10_mono_depth_mobilenetssd.py](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/10_mono_depth_mobilenetssd.py)

This example shows how to run MobileNetv2SSD on the left grayscale camera in parallel with running the disparity depth results, displaying both the depth map and the left grayscale stream, with the bounding box from the neural network overlaid.

![image](https://user-images.githubusercontent.com/32992551/103948502-19819900-50f7-11eb-9524-dcd6d7e05bcf.png)

### [11_rgb_encoding_mono_mobilenet.py](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/11_rgb_encoding_mono_mobilenet.py)

This example shows how to encode the RGB stream in h.265 while in parallel running MobileNetv2SSD on the left grayscale camera.  The example saves the h.265-encoded RGB stream to disk, while in parallel displaying the bounding boxes from MobileNetv2SSD overlaid on the left camera stream.

Pressing Ctrl+C will stop the recording and then convert it using ffmpeg into an mp4 to make it playable.  Note that ffmpeg will need to be installed and runnable for the conversion to mp4 to succeed.

![image](https://user-images.githubusercontent.com/32992551/103948535-24d4c480-50f7-11eb-912f-f541970c553b.png)

### [12_rgb_encoding_mono_mobilenet_depth.py ](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/12_rgb_encoding_mono_mobilenet_depth.py)

This example shows how to run all of the following in parallel:
1. Encode the RGB camera in h.265 and save it to disk on the host.
2. Run MobileNetv2SSD on the left grayscale camera, displaying the left stream and the overlaid bounding box on the host.
3. Run the depth node, displaying depth on the host.

Pressing Ctrl+C will stop the recording and then convert it using ffmpeg into an mp4 to make it playable.  Note that ffmpeg will need to be installed and runnable for the conversion to mp4 to succeed.

![image](https://user-images.githubusercontent.com/32992551/103948572-31f1b380-50f7-11eb-9e60-7996d7487d7d.png)

### [13_encoding_max_limit.py](https://github.com/luxonis/depthai-experiments/blob/master/gen2_examples/13_encoding_max_limit.py)

This example show the maximum possible resolution + framerate for the case of encoding all 3 cameras on DepthAI/OAK-D.  It encodes in parallel the RGB camera at 4K/2160p (3840x2160) at 25 FPS, and each of the grayscale cameras at 1280x720 at 25 FPS

- left: 1280x720 at 25 FPS
- RGB: 3840x2160 at 25 FPS
- right: 1280x720 at 25 FPS

This example is actually slightly above the theoritical maximum combination of resolution and framerate for DepthAI/OAK-D's video encoder, which is a total max pixel limit of 3840x2160 at 30FPS, which is 248,832,000 pixels/second limit.  This example is 1280x720x2x25 + 3840x2160x25 = 253,440,000 pixels/second.

Pressing Ctrl+C will stop the recording and then convert it using ffmpeg into an mp4 to make it playable.  Note that ffmpeg will need to be installed and runnable for the conversion to mp4 to succeed.
