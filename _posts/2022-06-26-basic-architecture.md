---
layout: post
title: Basic Architecture
date: 2022-06-26 20:45 +0100
---

![Robot and Brain](/assets/posts/2022-06-26-basic-architecture/the-social-robot-arch-1.svg)

I intend to publish on a two-week cadence, but I let life get away from me a bit over the last few weeks and so this post is very late. Sorry! I will do better in future.

There are other existing chatbots that are worth discussing, but in order to keep things interesting I'm going to intersperse the other "state of the art" posts with more technical posts about The Social Robot. Today we're going to look at some very basic architectural choices.

NAO, Alpha 2 & K9 are robots of very little brain. NAO has a dual core Intel Atom, K9 2 has Raspberry Pi 3s and I'm not sure what Alpha2's processor is, but I doubt I'll be running any deep learning algorithms on it.

So, starting with the obvious, our high-level picture looks a bit like this - we need to offload processing to something not on the robot. That might be  on local hardware or something running in the cloud.

That in turn invites more questions:

- is the picture really this simple? In other words, should there be more entities in this picture?
- what information needs to be passed between the robot and the "brain"
- what protocol should we use to send this information

Let's take these questions in order...

## What might be missing from our picture?

Some services might be quite hard for us to run ourselves. One example might be speech to text - we're going to need that if we're going to use voice to communicate with our robot and keyword spotting is unlikely to be enough. So let's say we decide to use Google, AWS, or Azure services to convert audio to text for us. Well, that's easy now our picture looks like this.

![Robot and Brain](/assets/posts/2022-06-26-basic-architecture/the-social-robot-arch-2.svg)

But wait, that means we're streaming audio from the robot to the "brain" and then the "brain" is streaming it to something else. That's going to introduce lag. Maybe the picture should look like this instead?

![Robot and Brain](/assets/posts/2022-06-26-basic-architecture/the-social-robot-arch-3.svg)

But now we've made things more complex on the robot. It has to know about multiple systems and we've leaked implementation details outside of the "brain". Also these cloud services aren't free we can't just stream audio continuously from the robot unless we have money to burn. That means we need something to detect if someone is speaking or otherwise gate the audio stream (for example by only streaming audio if the robot detects someone is looking at it). Streaming audio from the robot means that any audio "gate" is going to have to live on the robot too and there is only a limited amount of compute the robot will support.

For now let's aim for simplicity and worry about lag when/if we have to

![Robot and Brain](/assets/posts/2022-06-26-basic-architecture/the-social-robot-arch-4.svg)

Audio is likely to be the most latency sensitive media. For now let's assume that all media streamed from robot (for example video) is streamed first to the brain and then only elsewhere if part of the brain decides that's required.

## What's in our robot-brain communication?

If the only communication between the robot and the brain was audio then we'd have re-invented the smart speaker. The robot needs to sense it's environment so the brain also needs visual information, as well as input from any other sensors on the robot (tactile, distance sensors etc) as well as position information and joint angles (proprioception). The robot also needs to express itself so we also need to send motor commands to the robot as well as audio if the robot does not include a speech synthesizer or text commands if it has built-in text to speech (as NAO does for example).

![Robot and Brain](/assets/posts/2022-06-26-basic-architecture/the-social-robot-arch-5.svg)

## What protocol do we use for robot-brain communication?

ROS (Robot Operating System) is a platform designed to support robot applications. According to the [official site](https://www.ros.org/), ROS is "a set of software libraries and tools that help you build robot applications".  I've only dabbled a little with ROS, and my understanding is that it basically provides the following:

- pub/sub and client/server messaging frameworks
- drivers for many existing robots and hardware components
- implementations of important algorithms such as SLAM (Simultaneous Location And Mapping)
- simulation and visualisation tools

It sounds really cool, but I'm not going to use it. At least not yet.  This is what I care about concerning the transport between robot and brain:

- latency - if spoken interaction with the robot is going to seem natural then I want to keep the latency of audio streamed between the robot and the speech-to-text engine as low as possible
- privacy - the connection needs to be protected with reliable industry standard encryption
- standard protocols and tooling - both to make implementation easier and also for this to be a useful example of building an application with modern tooling

I'll use ROS where it makes sense to use it, but for the initial implementation I'm going to try [gRPC](https://grpc.io) for the body/brain transport:

- it supports multiple authentication methods
- it provides a secure transport via SSL/TLS
- it has a bi-directional streaming  mode so I can use it for body -> brain messages as well as brain -> body messages
- it supports any languages I might wish to use for the project

There are other messaging solutions I could have used, such as [ZeroMQ](https://zeromq.org/), but for now at least, gRPC appears to provide most of what I want with least effort.

## Conclusion

So at a very high and hand-wavy level we have something like this.

![Robot and Brain](/assets/posts/2022-06-26-basic-architecture/the-social-robot-arch-6.svg)

In the brain there will be a number of specialised services - these will likely be running in docker containers and eventually we'll need some kind of orchestrator such as docker swarm or kubernetes. On the robot we might be able to run docker (eg on Raspberry Pi) but mostly likely not (NAO, Alpha2) and so we may end up with one or a small number of executables that host multiple components. On NAO this will be a combination of native apps and python and on Alpha 2 an android app.

## Acknowledgements

- Diagrams drawn with [Excalidraw](https://excalidraw.com/)
- Robots Excalidraw library by  [@Kaligule](https://schauderbasis.de)
- Cloud Excalidraw library by  [@rfranzke](https://twitter.com/rafaelfranzke)
