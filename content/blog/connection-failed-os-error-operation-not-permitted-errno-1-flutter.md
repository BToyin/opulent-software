---
title: "Connection failed (OS Error: Operation not permitted, errno = 1) - Flutter"
date: 2024-12-17T20:10:00.000Z
author: Toyin Ajani
image: /images/blog/6616841b2c4e721c6bc11748_628e3175b148307220159bb0_build-20a-20flutter-20macos-20app.jpeg
type: blog
---
If you have come across this exact error in flutter when attempting to display an image in your flutter app (macOS desktop) then do not fret because the resolution for this is actually very simple. It is described as the most upvoted answer on stack-overflow here: 

<https://stackoverflow.com/questions/65458903/socketexception-connection-failed-os-error-operation-not-permitted-errno-1>

However, if you’d like a more step-by-step guide then you can also follow this.

This is the error you will be encountering, likely with a different address:

![MacOS Desktop error - images for flutter](/images/blog/screenshot-2024-12-17-at-08.47.02.png)

As per the stack-overflow answer:

> macOS needs you to request a specific entitlement in order to access the network. 

To do that, first open Xcode by running this command in a termial: `open macos/Runner.xcworkspace`

which should open XCode for you: 

![XCode dashboard](/images/blog/screenshot-2024-12-17-at-08.50.07.png)

Now inside **Release.entitlements** Click on the “+” to add a new row. 

* Paste in com.apple.security.network.client as the key
* Click enter and this should auto-change to “Outgoing Network Connections” 
* Then change the Value to YES (can just type true and it’ll change automatically) 

![Adding entitlement to XCode](/images/blog/screenshot-2024-12-17-at-08.55.48.png)

Do the same thing in **DebugProfile.entitlements**

Now just restart your flutter app and the image should display with no problems.

Hopefully that helps because I was struggling for a little bit finding the resolution.

Peace and Blessings
