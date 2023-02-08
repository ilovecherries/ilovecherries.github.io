---
layout: post
title:  "State of Linux for Oculus VR"
date:   2023-01-11 09:41:40 -0500
categories: vr
---
The one thing that permanently prevents me from having used Linux full-time in the past year is the usecase of VR. I used to have Arch and then EndeavourOS installed on my desktop for a very long time, and I would customize it a lot and basically try to make everything work under there.

One of the biggest reasons for me using Linux, actually, was the fact that Docker consumed so much less memory and was so much more performant than under Windows, I was fine with using Windows for most development things since there was WSL and I worked with that but I started to like get into devcontainers with VSCode and trying to make it work between my desktop and my Macbook like seemlessly without having to worry about dumb package management. (I could've probably used Nix in this case or asdf but this is hindsight now, I only use my Macbook for working on programming things now)

Anyway, I decided to take a gap year from my CS program and I had a *lot* of money left over from doing my internship, so I decided to buy a Quest 2 when it was only like $300 since I wanted VR for a long time. I reinstalled Windows like a week before it arrived and set up a lot of things in anticipation, since I was ready to like live in VR or something and do all my programming in it and like live in a cool virtual environment and use Oculus Link hooked up to my computer like a Pro User. (this doesn't work, VR is not good enough yet lol)

I was an idiot, though, and I actually had no idea how to use SteamVR from the Oculus interface (you actually just... literally start SteamVR... idiot me) so I got Virtual Desktop from the store and it was really good, but I didn't have a good wi-fi card at the time so I would get high latency when playing games like Beat Saber, so I started looking around for an alternative wired solution.

Enter [ALVR](https://alvr-org.github.io/), it's the wired solution I was looking for, and I find out that it actually supports Linux? I don't really pay any mind to it honestly, I just tried to get the wired VR working and like THINKING I got it working but I actually didn't... I did after a while though but I just started playing Beat Saber on my Quest natively and then used VD for things like VRChat.

Every few months, I do the following process to check up on the state of using VR with the Quest 2 on Linux:
1. Install [Fedora](https://getfedora.org/) or [EndeavourOS](https://endeavouros.com/) on my desktop with all the necessary drivers setup.
2. Install [ALVR](https://alvr-org.github.io/) both by getting the portable installation and building from source.
3. Spend an hour trying to make it work and looking on their Github and Discord to see if there's anything new I missed.
4. Give up and reinstall Windows after coming across some kind of dissapointment.

There was a time where I actually got it working perfectly, everything was streaming correctly, all the sound was working, not a lot of latency, but of course it was just an issue of the framerate was still just so much worse than what I had on Windows. I've tried installing custom kernels like [linux-tkg](https://github.com/Frogging-Family/linux-tkg), I've tried installing my NVIDIA drivers in several ways, different graphical environments, it just seems nothing will help.

It was so dissapointing after it feels like I've spent hours looking at logs trying to resolve dynamic linking issues, looking in their Discord server to see if anyone had the same issues I did, just an absolute mess of trying to amass all of this information trying to make it work properly. It's just not worth it when I can install Windows (I use [AtlasOS FACEIT Edition](https://atlasos.net/)) and get better performance by default and I don't have to fuck around nearly as much. Maybe it's the fact I have an NVIDIA card, maybe I'm missing something glaringly obvious, but I really don't use my desktop which I've basically decided is a gaming console enough to justify using Linux. I have my Macbook that I use for actually doing things on and, when I conciously want to game, I turn on my desktop and use that.