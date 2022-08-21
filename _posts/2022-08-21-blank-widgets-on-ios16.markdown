---
layout: post
title: "Blank Widgets on iOS 16"
date: 2022-08-21 00:00:00 -0500
categories:
---

This weekend I noticed the [Snaplists app's](/apps/snaplists/) widgets do not display properly on the iOS 16 beta. I discovered this issue after installing the beta on my iPad. The widgets appear blank, without any content.

![screenshot of blank widgets](/assets/snaplists-2022.6-widgets-broken.jpg)

At first, I thought maybe it is an issue in the beta that is yet to be fixed. But a quick search on Google and Twitter did not reveal anyone else complaining of the same problem. I concluded there must be some fix I need to make in the app.

I opened the project with Xcode 14 and built and ran the app on the simulator. This worked fine without any compilation/build errors. Next, I added one of the app's widgets to the home screen of the simulator. It was indeed blank. Next, I selected the widget extension's scheme and started debugging on the simulator. The widget extension was crashing immediately.

In Xcode's debug console, I saw this message:

```
2022-08-20 12:57:07.041006-0500 SimplistsWidgetExtension[28646:717385] [CK] In order to use CloudKit, your process must 
have a com.apple.developer.icloud-services entitlement. The value of this entitlement must be an array that includes the 
string "CloudKit" or "CloudKit-Anonymous".
```

So it seems the widget extension does not have the iCloud (with CloudKit) capability added to it. The app uses Core Data backed by CloudKit using Apple's `NSPersistentCloudKitContainer` to sync lists across devices. And the widget extension needs all of that too. Not having this capability on the widget extension didn't seem to be a problem on iOS 14 & 15, but it seems it is on 16.

Looking over the **Signing & Capabilities** tab of the project's targets, it was true that the widget extension was missing the capability. So I added it and selected the app's iCloud container.

![Screenshot of iCloud capability in Xcode](/assets/xcode-icloud-capability.jpg)

_Note: you may notice the app's source project is called Simplists, not Snaplists. That was indeed the original name I wanted to use, but it was not available_ 😕

My first thought was to wait for the GM release of Xcode 14 to fix this issue, and get an update approved ahead of the iOS 16 release. However, I suspected this issue could be fixed now with Xcode 13 (by adding the iCloud capability to the widget extension) and have that version of the app "just work" on iOS 16 when it ships.

Of course, if you're going to build an app with Xcode 13, then how do you test it on iOS 16? One way is to build it with Xcode 13, upload a build to the App Store, and then install it on the iOS 16 device using TestFlight. I did eventually do that as I always do as part of my release process. But, before doing all that, I also was able to build with Xcode 13 and then simply drag & drop the app bundle on the iOS 16 simulator. This is a handy way to test on the new OS when the app was built with the previous version of Xcode, without going through App Store Connect & TestFlight.

![Screenshot of Derived Data folder and iOS simulator](/assets/derived-data-drag-and-drop-on-simulator.jpg)

I simply built the app with Xcode 13. Then quit Xcode. Then launch the iOS 16 Simulator. Then browse to the app bundle in the DerivedData folder. Then drag & drop the app onto the simulator. The app then installs on the simulator and you can test it.

To wrap up, the widget extension was crashing on iOS 16 because it was missing the iCloud entitlement. This caused the widgets to appear completely blank without any content. Debugging with Xcode 14 revealed the issue, but it was resolved in Xcode 13 by adding that capability to the widget extension target. Dragging and dropping the app bundle made it possible to test a build on the iOS 16 simulator without building it with Xcode 14. With the fix, the app and its widgets still work on iOS 15, and also on iOS 16, and the new app version can be shipped now so it is available on the first day that iOS 16 ships 😅

Version 2022.7 of Snaplists, with this fix for blank widgets on iOS 16, is now available in the App Store.

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/snaplists-simple-lists-app/id1527429580)