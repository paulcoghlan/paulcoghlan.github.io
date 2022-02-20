---
title: "Add Audio Delay to MacOS OBS"
date: 2022-02-20T14:38:12Z
draft: false
tags:
    - hacks
    - zoom
    - audiovideo
    - obs
---

Unsynchronised audio and video is annoying and fatiguing on long Zoom calls.  Unfortunately using the OBS
Virtual Camera feature will add latency to your video feed, so you need add an appropriate audio delay.

There are two established MacOS tools for adding an audio loopback ([Loopback](https://rogueamoeba.com/loopback/) and
[Soundflower](https://github.com/mattingalls/Soundflower)) but they both require the installation of a MacOS kernel extension -
which I wasn't able to do because of the Mobile Device Manager app installed by my employer.

[BlackHole](https://github.com/ExistentialAudio/BlackHole) is open source and fortunately doesn't require a kernel extension to be installed.

I was able to use BlackHole with OBS to add an audio delay as follows:

1. Install BlackHole (`brew install blackhole-2ch`)
2. Set `BlackHole 2ch` to be the `Monitoring Device` in audio Settings:
{{< figure src="/2022/monitor.png" >}}
3. Click on the "Advanced Audio Properties" cog to add a delay to your audio device:
{{< figure src="/2022/cog.png" style="width: 320px;" >}}
4. Set `Audio Monitoring` to `Monitor Only (mute output)` and alter `Sync Offset` to set the audio delay
{{< figure src="/2022/delay.png" >}}
5. Use `BlackHole 2ch` as the input audio device in your video call software, e.g. for Zoom:
{{< figure src="/2022/microphone.png" >}}

You can join a test video call on another device and use something like a metronome app to figure out the necessary audio delay.  I used [Tempus](https://apps.apple.com/ee/app/tempus-a-touch-screen-metronome/id955594935) on my iPhone to flash the screen/make a sound at a regular interval.