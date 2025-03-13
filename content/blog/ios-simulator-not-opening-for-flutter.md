---
title: IOS Simulator not opening for Flutter
date: 2025-03-13T05:37:00.000Z
author: Toyin A.
image: /images/blog/maxresdefault.jpg
type: blog
---
If you have never run the IOS simulator on your mac before then you might be running into the same issue I had. Trying to open the IOS simulator through Flutter will just do nothing and the simulator never opens anything. Trying to create a new simulator similarly does not do anything and you possibly can open a new simulator but are unable to select an OS version when trying to do so. Restarting your machine doesn’t work either.

Very simply - The exact problem and solution are detailed below:

<https://stackoverflow.com/questions/77199500/ios-simulator-is-not-running>

[](https://stackoverflow.com/questions/77199500/ios-simulator-is-not-running)Just hope that having it here will allow more people to find and resolve the same issue. I will also paste the solution here:

If you are trying to start a new simulator and there are no options for OS Version, then it means that there are no simulator runtimes installed. Here are the steps I took:

1. Open Xcode.
2. On the menu bar, click on "Window"
3. Click "Devices and Simulators"
4. In the Devices and Simulators Window click on the "Simulators" tab. At this point, you won't see anything in the tab.
5. At the bottom left corner press the \`+\` icon. This will open a window to create a new simulator
6. in the window, click the dropdown menu next to "OS Version". Then, select the option "Download more simulator runtimes...".
7. From here, you can download which platforms you want to simulate (Which in my case is IOS).

Peace and Blessings

T
