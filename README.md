# deepstream-test1
this is deepstream-test1, more please refer to [nvidia deepstream](https://developer.nvidia.com/deepstream-sdk)

# workflow
filesrc（a single H.264 stream） → decode（v412） → nvstreammux → nvinfer (primary detector) → nvdsosd → renderer

# Prequisites

Please follow instructions in the apps/sample_apps/deepstream-app/README on how
to install the prequisites for Deepstream SDK, the DeepStream SDK itself and the
apps.

You must have the following development packages installed
```
   GStreamer-1.0
   GStreamer-1.0 Base Plugins
   GStreamer-1.0 gstrtspserver
   X11 client-side library
```

To install these packages, execute the following command:
```
   sudo apt-get install libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev \
   libgstrtspserver-1.0-dev libx11-dev
```
Compilation Steps:
```
  $ cd apps/deepstream-test1/
  $ make
  $ ./deepstream-test1-app <h264_elementary_stream> #./deepstream-test1-app sample_720p.h264
```

# detailed infomation

This document shall describe about the sample deepstream-test1 application.

It is meant for simple demonstration of how to use the various DeepStream SDK
elements in the pipeline and extract meaningful insights from a video stream.

This sample creates instance of "nvinfer" element. Instance of
the "nvinfer" uses TensorRT API to execute inferencing on a model. Using a
correct configuration for a nvinfer element instance is therefore very
important as considerable behaviors of the instance are parameterized
through these configs.

For reference, here are the config files used for this sample :
1. The 4-class detector (referred to as pgie in this sample) uses
    dstest1_pgie_config.txt

In this sample, we first create one instance of "nvinfer", referred as the pgie.
This is our 4 class detector and it detects for "Vehicle , RoadSign, TwoWheeler,
Person".
nvinfer element attach some MetaData to the buffer. By attaching
the probe function at the end of the pipeline, one can extract meaningful
information from this inference. Please refer the "osd_sink_pad_buffer_probe"
function in the sample code. For details on the Metadata format, refer to the
file "gstnvdsmeta.h"
