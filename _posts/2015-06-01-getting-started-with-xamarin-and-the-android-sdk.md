---
title: Getting Started with Xamarin Studio and the Android SDK
layout: post
published: true
---

Let's face it. Xamarin is a pretty amazing company when you look at
it. The father of Xamarin, Miguel de Icaza had a vision when he saw
the first release of Microsoft .Net and the C# language. He said, (
and I'm paraphrasing) - "this is an amazing technology and it needs to
go open source and cross-platform so the whole world can
benefit". Well, maybe he didn't actually day "cross-platform" *per
se*, but certainly words to that effect.

One of the real stumbling blocks in moving from being a pure Windows
developer to a Cross-Platform dev is understanding the Android
SDK. When I first came across Xamarin Forms, I thought to myself, I
wonder how far I can get without having to delve into masses of
Android documentation and Hacker resources.

Don't get me wrong. The Xamarin documentation is amazing, and very
detailed. There just is a Lot of it. And I guess you could call me
Lazy, but I really just want to follow the Pareto Principle in
everything I do, and want to get 80% of the results with just 20% of
the effort. It's just more effective that way. I can always fill in
the rest over the next decade.

For me I want to know what's the least amount I need to learn about
Android internals and get to being a really good cross platform
dev. Simples!

So, what have I learned:

There were 3 major releases of the Android OS in recent times, all
named after deserts. In order of most recent first, they are:

5.0 Android Lollipop which uses API Library 21 and beyond.

4.4 Android KitKat which uses API Library 19 and beyond.

4.1 Android Jelly Bean which uses API Library 16 and above.

Versions before 4.1 are probably not relevant to you unless you are
thinking of targeting older devices, and you can probably work out the
rest of the equation yourself with a quick Google search.

So, really you have two numbers to remember: the minimum SDK version
you want to target, in my case, 16, and the current version you want
to target. At the time of writing the latest version of the API is 22,
so that's what I'm going to target. Note that your app can run on a
target platform newer than this, but the OS will not make any
assumptions about newer features of the OS that your app doesn't know
about. So, to recap:


	minSDKversion=16	
	targetSDKversion=22

## CPU and Emulation

Next, a quick note about CPU family & emulation.

When you start off with your first app, you really don't want to use a
real android device, because frankly the learning curve is enough
without looking at debugging hardware issues and bricked phones. So we
will start with a virtual device that runs a little virtual
machine. That way we can wipe the device at any time and start again
if we need to.

A few caveats: If you are already running Hyper-V, VMware, Virtual
Box, or some other virtual PC framework, then this is not going to
work. This is because you need to take advantage of a CPU
virtualization feature called VT-x to manage virtual devices, and it's
a single resource. If Hyper-V is using it, your android virtual device
can't. It's as simple as that. (Some motherboards require you to turn
on VT-x in the BIOS. stackoverflow is your friend here).

Similarly, you can't launch a virtual device from a virtual machine
because it's host is already using the virtualization bit feature.

Which makes laptops with about 8GB ideal for android development. Why?
If you have already purchased a laptop with only 8GB of ram you
probably have no plans to run hyper-v, because frankly 8GB is just not
enough to run dev apps and virtual machines.

If you are running a desktop or server like Windows Server 2012 R2,
you are more than likely already running hyper-v, or are thinking of
running your dev environment in a guest virtual machine. It won't work
either. Or rather, you can compile and build your app, but you can't
test it because you can't start a virtual machine.

Now all of the above is based on the premise that you are using
hardware emulation. In other words, when you virtual android device is
running, each machine code instruction that your virtual device
executes is passed to your physical CPU but in a sandbox, and so can
run at a speed that is reasonably close to the speed of a real device
(Within an order of magnitude at least). So to run a virtual android
device on an Intel or AMD architecture CPU, you need to choose an
Intel Atom based virtual device hardware because it is of the same
family of processors.

The alternative is software emulation, where the hardware CPU of your
target device is a totally different CPU family. An example would be a
virtual device running on an ARM architecture device. In this case
each instruction has to be translated from an ARM architecture
instruction to a set of equivalent Intel family instructions (x86 op
codes), sent to your physical CPU to operate on, then take the result
and translate this back.

This is painful slow, and while software emulation does work, you will
find yourself wishing it didn't every time you boot your virtual
device. Believe me, it's painful.

So the lesson here is not to use software emulation, at least not
starting off, and not unless you have to.

There is however one last obstacle you have to overcome in order to be
able to take advantage of hardware emulation. You need to download and
install a hardware abstraction layer device manager tool to perform
this hardware translation. Google "Intel Hardware Accelerated
Execution Manager". Search, download and install it now, as it will
save you a lot of time and hassle later.

Armed with all the above info, all that is left to do is to download
and install the Xamarin Studio Starter Edition to your laptop, and
when prompted just choose the defaults for everything. It has to go
off and install a whole range of SDKs and Tools, so best kick this off
and let it run for a couple hours and do something more productive
than watching a progress bar.

Once done, we need to start the Android SDK Manager and choose what
API Libraries we want to install, and the OS images and Extras we need
to get up and running. This is the focus of the next post.

Happy hunting.

P.S. Here are some handy references you will probably bookmark but
never read later because you don't have time.

http://developer.android.com/about/versions/android-5.0.html

http://developer.android.com/about/versions/android-4.4.html

http://developer.android.com/about/versions/android-4.1.html
