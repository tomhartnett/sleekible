---
layout: post
title: "FFmpeg Wrapper"
date: 2025-02-22 00:00:00 -0600
categories:
---

As an iOS developer, I take a lot of screenshots and videos of the iOS Simulator at work. I often want to share the videos with others via Slack, Teams, Jira, etc, but the file size of a video screen recording can get very large. For several years now, I have routinely used the excellent [FFmpeg](https://ffmpeg.org) command-line tool to reduce the size of video files before sharing with others. Another benefit is that it will convert the macOS Quicktime video format (.mov) to .mp4, which is easier to play on Windows. FFmpeg is great for this task, but I sometimes can't remember the command line arguments to use and have to look them up again. 

An idea I've had for a while was to create a macOS app that would allow me to drag and drop a file onto it. The app would then run FFmpeg with the desired arguments to process the file. Building this "wrapper" for FFmpeg was a fun little weekend project. I named the app VDrop, short for "video drop".

![Screenshot of VDrop](/assets/vdrop-app-screenshot.png)

![Demo video of VDrop](/assets/vdrop-demo.gif)

The app has a single window. If you close that window, it terminates the app. There is a green rounded rect to drag and drop the file. Dropping it there will launch FFmpeg. By default, it will convert a .mov file to .mp4 and keep the same filename. If the destination filename already exists, it will append a number to it e.g. "some-file 1.mp4".

There are a couple options for the file to be output. These affect the arguments passed to FFmpeg.

- The **Animated GIF** checkbox will convert the video to an animated GIF. I typically use the .gif format for use on the web (like in the video above).
- The **Downscale width** checkbox allows you to enter an explicit width for the output video. Screen recordings taken in the iOS Simulator have a very large resolution. For example, a video from the iPhone 16 Pro simulator is 1206 × 2622. It's not necessary to downscale the video to get a significantly reduced file size, but I sometimes do it depending on how I plan to use the video.

The app is built with SwiftUI. I made the repo public and it's available here: [https://github.com/tomhartnett/VDrop](https://github.com/tomhartnett/VDrop)
