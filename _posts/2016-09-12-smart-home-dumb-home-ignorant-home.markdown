---
title: Smart Home, Dumb Home, Ignorant Home
date: 2016-09-12 10:35:00 -05:00
permalink: "/posts/ignorance"
categories:
- smart-home
tags:
- connected home
- IoT
- home automation
- automated ignorance
- terminology
layout: post-sidebar
author_name: Brandon Satrom
author_url: "/author/brandon"
author_avatar: brandon
show_avatar: true
read_time: 15
feature_image: feature-blueprint
show_related_posts: true
square_related: recommend-blueprint
---

> Stupidity has a certain charm. Ignorance does not. - Frank Zappa

In an increasingly connected world, much of our attention seems to be drawn to the realm of “intelligence.” Not so much in humanity, but in the increasing intelligence of “things” around us. “Smart Homes,” “Smart Cities,” “Smart Cars,” “Smart Buildings,” and “Smart watches” are all terms that have seen increased usage over the last decade, and a sharp increase over the last few years. Our vision of the future is to make everything around us "smart," ostensibly in an effort to make ourselves smarter.

<script type="text/javascript" src="https://ssl.gstatic.com/trends_nrtr/680_RC25/embed_loader.js"></script> <script type="text/javascript"> trends.embed.renderExploreWidget("TIMESERIES", {"comparisonItem":[{"keyword":"smart city","geo":"","time":"today 5-y"},{"keyword":"smart home","geo":"","time":"today 5-y"},{"keyword":"smart car","geo":"","time":"today 5-y"},{"keyword":"smart building","geo":"","time":"today 5-y"}],"category":0,"property":""}, {}); 
</script> 


In the midst of all of this attention, there seems to be less critical thought about what imbues “intelligence” on these things in the first place. The idea that a “smart thing” is a normal thing with a set of sensors (and possibly actuators) that can tell you about the thing, read the environment around the thing, and potentially control the thing is generally accepted. The thing in question is invariably connected to a network, meaning that it can communicate with the outside world, and the outside world with it.

Putting it another way, *sensing plus connectivity is "smart"*.

On the flip side, to be a “dumb thing” is to possess none of this ability to sense, communicate and control beyond the thing itself. A “dumb thing” is a closed system, useful only in analog and forever the scorn of its owners, who’d rather not get up and walk across the room to turn on the coffee pot. 

Putting it another way *the absence of connectivity, sensing or not, is "dumb"*

And while this distinction between smart and dumb, connected and disconnected things works for broad journalistic narrative, it masks the non-binary reality of the so called “Internet of Things.” It also papers over the increasing unease people--especially non-technical consumers--feel about acquiring devices that provide an open door to the world around them.

This is especially true in the realm of the “smart home,” where a never-ending litany of connected devices are finding their way into the homes of many, from lightbulbs to thermostats; coffee pots to refrigerators; garage door openers to sprinkler systems. According to the narrative, these things are inherently good for consumers because they are "smart."

But beneath this binary narrative, a more sinister truth lurks: many of these devices are something worse than dumb: they are ignorant. And ignorance is dangerous.

## The "Ignorant Home," Defined

The idea of "ignorant devices" and "ignorant homes" isn't commonly used that I've seen, but should be part of the narrative around smart home technologies. By "ignorance," I don't mean that a device cannot sense or isn't connected. Rather, an ignorant device is one that is:

A) Unaware of the connected home around it and/or unable to communicate with that home;
B) Unable to function when its communication mechanisms fail;
C) Unable to defend itself against external attacks;

If the device or devices in your home exhibit any of these qualities, it would be better for them to be just plain dumb. To be sure, the devices in these categories border on dangerous.

### The Unaware Device

Imagine your home has a slew of the latest smart devices. You have a Nest thermostat, two Nest cameras, a Ring video doorbell, a smattering of Hue lightbulbs, an August door lock and some WeMo plugs in your kitchen so you can start the coffee maker and  auto-toast an English Muffin each morning. This is a fine setup, and one that many folks on the bleeding edge of smart homes have, until it comes time to actually do something with those devices.

First and foremost, controlling the devices above—which are peddled by five different manufacturers—requires five different mobile apps, all of which have their own User Experience, their own workflows and their own ways of orchestrating devices. Some of these apps, like the Hue app, are so convoluted in how they manage scenes that the simple act of adding another device from the same manufacturer to an existing scene or workflow is painful. Try adding new Hue bulbs to an existing scene if you want to feel the pain yourself.

And if you want to create scenes that tie multiple devices together? Maybe you want to say “Goodnight, sweet home” and have your living room lights turned off, the front door dead-bolted and the entryway Nest cam activated? Good luck. If you’ve managed to grab devices for which the manufactures have entered into some kind of partnership agreement, you might discover a light integration, but you’re unlikely to find this among all your devices. What’s more, even where integrations do exist, you’re stuck awkwardly using them via another manufacturer’s app.

The devices that fall into this category are ignorant because they are unaware that they are not alone in the smart home. They have no concept of other devices and other applications, only that they themselves exist, they are connected and that they perform some kind of function. It’s digital solipsism at its best. 

Even worse, many of these devices are often ignorant that they are *in* a home to begin with. While these devices are on your local subnet, they communicate almost exclusively with a public cloud maintained by their manufacturer. Most of these devices provide no way to interface with them on the local network; it’s cloud or bust. Even when you’re controlling the device from your phone, you’re doing so through the public cloud and not your local network. And if you’re controlling a lightbulb in your home, but you have to go through the cloud to do it, is the bulb really on your network, or is your home just an extension of the public cloud?

### The Non-Functional Device

The second type of ignorance stems from the role that the cloud plays in the lives of many of these devices. Many connected devices have a cloud component to them, from door locks to thermostats and beyond. Benefits of this connection include:

- The vendor can make it easier for you to get the device running in your home without having a central smart home controller;
- The vendor can push updates to the device with little to no intervention from the consumer;
- The consumer can control the device when they are away from home.

These are all great benefits that have allowed smart home devices to proliferate faster than related technologies of the past (like X10), but they also come with a few serious, related drawbacks to being cloud connected:

- If you can reach into your home and control the device from the cloud, in theory, anyone else can, too.
- If the device is cloud-dependent, it may stop functioning when your home network or the vendor’s cloud goes down.
- The vendor can reach into your home and render the device non-functional.

We’ll touch on the first drawback in the next section, but for now let’s cover the last two.

Cloud connectedness is a virtue, cloud dependency a vice. The benefits of a cloud connection quickly turn sour when a device actually must have a cloud connection to function or when that connection can easily render the device useless. Consider the [Ring](http://ring.com) doorbell. I have one of these devices next to my front door, as well as a Ring Chime in my entryway. The purpose of the chime is to replicate the function of an old-school doorbell chime, which goes off inside the house when the outside button is pushed. 

It’s a great concept, in theory, except for the fact that the Ring Doorbell and the Ring Chime in my home don’t communicate directly with each other, rather through Ring’s central, managed cloud. If my home network drops or the cloud goes down, not only do I *not* get notifications on my phone, but the Chime is completely non-functional.

The example of the Ring doorbell illustrates the issue with cloud dependency, but the use case is little more than an minor annoyance. This case, when applied to other, more critical smart home devices, however, can lead to more severe consequences.

Take the Nest Thermostat, for instance, a device considered to be a vanguard of the smart home era. The Nest is a clever, beautiful little device, but is also very cloud dependent. In just January of 2016, Nest [pushed an update](https://thestack.com/iot/2016/01/14/nest-thermostat-bug-leaves-owners-without-heating/) to its thermostats that included a bug which rendered the devices (and home thermostats) completely non-functional. In January. In a large part of the midwest United States. 

Think about it, because of the central role of the cloud in the life of this devices, thousands of people were left without heat in the dead of Winter. It’s one thing to miss the FedEx driver when he drops off a package. Quite another to not be able to control your perfectly functional furnace when it’s ten below outside.

Another, more seditious reality lies in the second point, and frankly is a subject for further explanation in a full post, but I’ll touch on it here. That is the cloud allowing the device vendor to render your device non-functional. One recent example of this occurred earlier this year with the Revolv smart home hub. Revolv was acquired by Nest in 2014 and was considered to be a very capable smart home controller, a device which is meant to solve the problem of disconnected, ignorant devices by installing a piece of hardware that unifies those devices and centrally manages your home.

Revolv was a solid device in the space and while not wildly successful, it did show promise. That is, until [Google shut it down unceremoniously](https://thestack.com/iot/2016/01/14/nest-thermostat-bug-leaves-owners-without-heating/) in March of this year. What was even more notable, however, was not that Google shut down the service. These things happen all the time, especially at Google. The notable tidbit here is that, in the process of shutting down the Revolv, they rendered each and every purchased Revolv device totally and 100% non-functional. They couldn’t even be used to turn on a light bulb once Google shut things down. The same cloud dependency that allowed the Revolv scale and easily manage homes was used to reach into the homes of each and every owner and render their homes instantly dumber.

<blockquote class="twitter-tweet" data-partner="tweetdeck"><p lang="en" dir="ltr">Holy fucking shit. <a href="https://twitter.com/nest">@nest</a> smoke alarm malfunctions in the middle of the night, can&#39;t be silenced. No reason. Now no smoke alarm.</p>&mdash; Ken MacInnis (@kcm) <a href="https://twitter.com/kcm/status/765449955389517824">August 16, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

The cloud is a great boon to the smart home, but it comes with tradeoffs (and risks, which we’ll discuss next). To really step out of the realm of ignorance into enlightenment, smart device vendors *must* ensure that being “cloud connected” doesn’t turn into “cloud dependent.”

### The Defenseless Device

While being unaware and cloud-dependence range from the annoying to the downright frustrating, there is another aspect to ignorance that is altogether more dangerous. That is, when devices that can make your home less safe, and even be used against you.

This is somewhat an extension of the last type of device, but, in this case, is about devices that—through their cloud connection—can be taken over by malicious 3rd parties for a seditious purpose. By even allowing these devices into your home, you’re creating an attack vector that can literally provide malicious actors with the keys to your home.

One notable example of this type of exploit was [unveiled earlier this year](http://mashable.com/2016/05/02/smartthings-hack/#cL.Ys9SDDPqQ) by security researchers at the University of Michigan. Using the popular [Samsung SmartThings](https://www.smartthings.com/) system—another smart home controller designed to coordinate your home’s various devices—these individuals were able to install an app in the SmartThings marketplace that logged pin code changes for smart door locks and sent those codes via SMS to the attacker. With these codes, an attacker can install his or her own secret codes and waltz right into your home, anytime. This is possible because SmartThings has an exploit that allow over-privileged apps—that is, apps that claim they are doing something simple, like checking the battery status on a device, but which do more than the end-user allows for—to be installed from the SmartThings marketplace. Unlike the heavily curated marketplaces that exist for mobile devices today, many of the home automation marketplaces are so nascent and unproven as to be markedly unsafe.

Another example [popped up](http://motherboard.vice.com/read/internet-of-things-ransomware-smart-thermostat) in August of this year, and is just as frightening. In this case, security researchers were able to take control of a connected thermostat (they didn’t say which brand) and install a piece of ransomware on the device. For those not familiar with the term, ransomware is the term for a piece of malicious software that essentially “locks up” your device until you pay the attacker to remove the lock. Historically, this has been used to render personal computers unusable. In this case, researchers were able to run an exploit on a connected thermostat in a similar fashion, displaying a “pay or else” message and then rendering the thermostat inoperable.

This is serious and should sound frightening. Imagine someone taking over your thermostat and setting the temperature to 99 degrees in August and insisting you pay money to be able to cool your own home again.

The fact is, the “smart home” space is still young, and while “home automation” has been around for a while, it’s never had this volume of interest, and has never been so connected to the outside world. Vendors in this space are still figuring out how to secure their devices, and thus, your home, but they are doing so as they rush to take part in the in-progress smart home land grab. This is all well and good, but safety and security are not areas for extensive field testing. It is our responsibility as consumers to not only pay critical attention to the security features of our devices, but to push the vendors we buy from to make security a priority, not a nice to have feature.

## Avoiding the Ignorant Home

At this point, you’re probably reading this and thinking *why would I ever buy even a connected lightbulb again?* But in spite of the critical nature of this post, I am actually quite bullish on the smart home. My concern stems from the fact that we’re sitting at a critical inflection point where smart home technologies will *never* reach mass adoption if the purveyors of those devices don’t get their collective acts together and purge ignorance from their ecosystems.

As consumers and end-users of these devices, the best way that we can ensure that device vendors focus their attention on creating safe and secure devices is to push them to do so via our buying behavior. Avoid devices that have a history of known-issues, as well as devices that require a cloud connection to function. If you don’t know, do some research, and ask online, especially if you’re shopping via Amazon, where questions can be a powerful influencer for others considering a device. The bottom line is this: only you have the safety of your home in mind, so be aggressive and look for devices that co-exist well on your network, that integrate with others and that are inherently well-protected.

Beyond the practical things you and I can do to keep our homes safe and smart, device vendors can move themselves towards building more safe and secure (and intelligent) ecosystems by taking a few lessons from nature.

## The Smart Home and *Natural* Intelligence

In the real world, that is the dumb, disconnected natural world around us, "intelligence" means survival, and the vast majority of creatures, including humans, have learned to survive through *community* and *security.* Creatures unaware of external threats, or unable to defend themselves, quickly die out, as do those that live "apart from the pack" and attempt to survive alone.

As the number of connected devices in our homes and around us proliferates--growing into the trillions in the coming years--we'd be wise to view these devices as part of an electronic ecology, instead of merely bits of connected silicon in our environment. With this as our guidance, we should expect that *communal*, *adaptable* and *secure* devices would rise to the top and stand as the best devices for our homes. I’ll share more about what I think this means in next post in this series.

As we build our own ecologies and ecosystems in our homes, buildings and cities, we'd be wise to apply the same principles: For devices to be truly intelligent--and for our homes to inherit the same qualities--they must be communal, they must be able to adapt and they must be secure. Anything else isn't just dumb, it's dangerous.