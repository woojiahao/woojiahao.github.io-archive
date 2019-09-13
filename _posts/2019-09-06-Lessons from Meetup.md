---
published: true
title: Lessons from Meetup (and Docker)
---

Recently, I got into Docker and used [Meetup](https://github.com/woojiahao/meetup) as a testing grounds for improving my knowledge on Docker. Meetup is a Discord bot written with Kotlin that provides updates on programming events happening in Singapore. The idea was first proposed by a member of the SG Computer Science server.

While there was another programming group (Kopi.JS) with a similar tool in [Slakoth](https://github.com/cheeaun/slakoth), it was for Slack and as such did not support Discord. So I did what any developer would do and replicated the application for Discord. 

## Why Meetup.com API?!
One of the pain points that surfaced was with the Meetup.com events API. Originally, I had planned to use the service to find all the tech events within Singapore, but it all failed when they had decided to promote their API to use OAuth and with that, they made the endpoint I was interested in premium only and as such, I no longer had access to that feature.

## Thank you Engineers.SG
When looking through Slakoth's source code, I realised that it relied on this API from Engineers.SG, a site where all programming events in Singapore are managed and hosted to. So I reached out to the developer of the API to see if it was open for other parties to use and thankfully it was!

## Learning Docker
Ah Docker, what a simple yet brilliant tool that improves the lives of the developers you grace with your presence. Aside from a short workshop I took a year ago hosted by Michael Cheng (one of the main forces behind Engineers.SG), I avoided Docker. My aversion of the tool stemmed from a bad experience during the workshop where I was unable to get Docker worker on my Windows machine and it ended up introducing an irrational fear of the tool as "hard to set up and use". 

Conceptually, Docker also felt daunting - being introduced to concepts like mounting volumes and the underlying system of Docker was not the best introduction to Docker.

### What changed? 
Well, over the recent semester break, I wanted to step out of my comfort zone and experience what Docker was like in the context of a real world project. To get over the first hurdle which was the fact that Docker was not supported on Windows Home, I decided to install Manjaro linux and go full linux as my daily driver. I will make a post about the other reasons why I had switched, but to sum it up, my school had cut support for Windows licenses and I was interested in using Linux beyond just the basic bash commands. Docker was the cherry on the cake since it was much easier to get started on Linux.

That resolved the first issue - "hardware" support. Now to tackle my irrational fear of Docker. I took the simplest approach I could think of and just dove right into the "Get started" guide curated by the Docker team and followed each lesson religiously. I was bent on understanding this system that had stumped me for so long.

As I practiced the example pieces provided by the Docker team, Docker felt less challenging and more interesting. The potential it held began to intrigue me and I fell in love with it's core concept - **Build, Share and Run Any App, Anywhere.** Soon, I was vying for projects to do to get my hands dirty with Docker. 

This was where Meetup came in.

## Ooo shiny!
I started Meetup as a side project for a computer science server for Singaporean developers back in March/April. Its goal was to be able to search for and post updates on programming events that were happening here in Singapore. As mentioned previously, I had plans for Meetup to use the, well, the Meetup.com API to retrieve all of the local events happening. However, these dreams were cut short and eventually, with my internship rearing its head, I was unable to complete Meetup to include the features that I wanted for it.

Fast forward 4 months and 1 completed internship later, I finally got the time to work on personal projects and seeing as I was also learning Docker, I had decided to make use of Meetup as a way to start out with Docker. It was perfect! It matched all of my criteria - simple, server-hosted and all for me! I wanted a testing ground that I had full control over and still added value to myself (and others in this case). 

### Starting out
However, before starting to completely integrate a Discord bot with Docker, I had to first test whether I could get a Docker working with a Discord bot. So I decided to make a sample project to test it out. I decided to properly comment each component of this bot so that I would have a point of reference if I ever needed to have a reference point. 

The test project was a resounding success and I even got a star out of it (you can find the repository [here](https://github.com/woojiahao/discord-docker))! And so we proceed with our venture into the world of Docker with integrating it with Meetup. 

### Creating the `Dockerfile`
Writing the `Dockerfile` for Meetup was relatively simple seeing how the test project was setup to mimic the environment of Meetup. 

### Integrating with Heroku
An aspect of Meetup is its time-sensitive nature. This has caused many problems during my initial development as I could never get the daily posting feature to work properly. 