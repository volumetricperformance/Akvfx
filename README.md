Volumetric Performance Toolbox
=====

> How can artists create new live performances during the time of COVID-19? Volumetric Performance Toolbox empowers creators to perform from their own living spaces for a virtual audience. Movement artist Valencia James performed publicly with the Toolbox for the first time on September 24, 2020 in the Mozilla Hubs virtual social space. The project is a collaboration with Valencia James as part of Eyebeam’s Rapid Response for a Better Digital Future fellowship.

More information on the Volumetric Performance Toolbox here:

https://valenciajames.com/volumetric-performance/
https://www.glowbox.io/work/volumetric-performance/

This is a fork of the AKFVX project, heavily modified to stream either a RGB+D texture to ffmpeg -or- a pointcloud seen to Spout.


Akvfx
=====

**Akvfx** is a Unity plugin that captures color/depth data from an [Azure
Kinect] device and converts them into attribute maps (textures) handy for using
with [Visual Effect Graph].

[Azure Kinect]: https://azure.com/kinect
[Visual Effect Graph]: https://unity.com/visual-effect-graph

System Requirements
-------------------

- Windows 10
- Unity 2019.4
- Spout
- Azure Kinect DK

See also the [System Requirements] page of Azure Kinect DK. Note that Akvfx
doesn't support Linux at the moment.

[System Requirements]:
    https://docs.microsoft.com/en-us/azure/kinect-dk/system-requirements

[Spout]
https://leadedge.github.io/
	
[Fork specific notes]

This fork is an experiment in live streaming the VFX to an RMTP end point.
- Output to a virtual webcam using SpoutCam
- Output rgb channel and depth channel (as HSV) for reconstruction in threejs on the "other end"

FFMPEG:
- View Unity Output
 ./ffplay -f dshow -video_size 640x960 -vf "format=yuv420p" -i video="SpoutCam"
 
- Stream output to rtmp, node media server
 ./ffmpeg -f dshow -video_size 640x960 -i video="Unity Video Capture" -c:v libx264 -preset veryfast -b:v 1984k -maxrate 1984k -bufsize 3968k -vf "format=yuv420p" -g 60 -f flv rtmp://localhost:1935/live/test
 
- Stream a debug video
  ./ffmpeg -stream_loop -1 -i debug.mp4 -c:v libx264 -preset veryfast -b:v 1984k -maxrate 1984k -bufsize 3968k -vf "format=yuv420p" -g 60 -f flv rtmp://localhost:1935/live/test
 
- Play a stream from local node media server
 ./ffplay http://localhost:8090/live/test/index.m3u8
