---
layout: post
title: Hello World
date: 2022-03-20 18:01 +0000
canonical_url: 'https://www.thesocialrobot.org/posts/hello-world/'
---

![The Robots](/assets/posts/2022-03-20-hello-world/the-robots.jpg)

I have to admit I'm feeling a little nervous as if I'm about to go on stage and make a fool of myself by sounding hopelessly naive or optimistic. I've had this project in mind for a while and it's easy to feel good about it while it's just daydreaming, but it's time to actually do something and I'm not sure how well I'll be able to do it.

I find social robotics particularly interesting. Robots are, obviously, cool by themselves; but the key to social robotics is how robots interact with people. They are, in effect, mobile, embodied interfaces between humans and computer systems. Think of the ships' avatars in [Iain M Banks' culture universe](https://en.wikipedia.org/wiki/Culture_series). There are some great robotic platforms out there, but I don't think they are fulfilling their potential. I've seen some great applications built using social robots - my favourite being the use of the NAO robot to help schools teach autistic children.

But something is missing.

So I've set myself a goal: create a truly social robotic application that acts as a companion to people.

What would this look like? Let's start with a non-goal first. I'm not trying to build AGI (Artificial General Intelligence)! I'm neither smart enough or well-funded enough to believe I can do something that well funded labs full of very smart people have not yet been able to achieve.

There are chatbot applications that some people, including me, have found helpful; such as [woebot](https://woebothealth.com/), [Mitsuku](http://www.mitsuku.com) and [replika](https://replika.ai/). However, these are just chatbots they exist on your phone or in a web browser, not in your world. Would it be possible to make something more helpful if it could actually perceive and interact with the physical world? I don't know but finding out sounds like fun!

One concern that people have raised with platforms like Replika is that people can get very attached to the chatbots, but that at any time the company that runs Replika could decide to shut the service down leaving people with nothing. So, next goal, this needs to be something that people could realistically run themselves either on their own hardware or on a cloud setup under their control that doesn't cost a fortune to run.

Another concern about general chatbots is that an unscrupulous company could use them for covert advertising or to promote products and services not necessarily in their owners best interests. This requires, that people maintain control over the software and hardware they are depending on.

It's not going to be any fun if everyone's robot acts in exactly the same way so the robots need to be able to develop their own personalities.

Taking my dog as an inspiration, one of the things that makes having him around so much fun is that as well as his own personality he has his own motivations and life too. Just because I'm in the mood to play with doesn't mean he wants to play. He'll do things that he wants to do, explore the garden, sleep, chew something, come to me wanting to be fed or petted. In other words he has his own inner life, he's not a passive object waiting for me to cajole him into interacting with me.

Finally, we need to talk about the hardware, the robots themselves. I've going to make as much of the project as general as possible but there still needs to be real hardware running the software. There are several robots I would like the software to run on (anyone got a spare Pepper, they'd like to donate 🙂 ) but I need to start with robots that are available to me:

- **NAO** - Eighteen years old and still the best (IMHO) humanoid robot platform available to mortals. Sadly, after buying Aldebaran Robotics, Softbank appeared to do everything they could to alienate the vibrant NAO developer community. There are supposedly 13,000 NAOs out there and many universities probably have one or two. I'm lucky to have a NAO v5 and this is going to be the first platform I target.
- **Alpha 2** - A humanoid robot by UBTECH resulting from a kickstarter campaign. It showed great promise until UBTECH decided to pursue an alexa-integrated version of Alpha and stopped supporting the online store and SDK for Alpha2. The SDK contains some annoying features such as requiring an app to be available via the now-defunct store to use text-to-speech. I hope to be able to workaround these issues and Alpha2 will, if I can get it working, be an example android client.
- **K9** - A home-brew robot made from a radio-controlled toy, a couple of Raspberry PI 3s, a servo controller, motor controller and audio input cards. This will act as an example of a low-cost DIY robot.

Even if you don't find this application compelling, The Social Robot, may still be interesting as

1. an example both of deploying a complete system using machine learning and
2. of applying machine learning at the edge (ie in this case on the robot) as well as in the cloud.

Next steps: Before we figure out how we're going to build an embodied chatbot / companion we should take a look at existing chatbots, see how well they work and what we can learn from them.
