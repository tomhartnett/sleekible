---
layout: post
title: "From ClockKit to WidgetKit"
date: 2024-02-04 00:00:00 -0600
categories:
---

Starting in watchOS 9 in 2022, Apple deprecated ClockKit, the framework for creating complications for the watch face. Snaplists has had complications since it launched in 2021. The app has static complications that provide a quick way to launch the app from a watch face (similar to Reminders, Messages, Shortcuts, and other complications provided by Apple). Instead of ClockKit, the new way to create complications is with WidgetKit framework, which has been around for a few years for creating home screen widgets on iOS. With the release of version 2024.2 of the Snaplists app, I finally migrated the complications to WidgetKit.

A nice benefit, albeit a small one, of using WidgetKit for complications is that I finally was able to provide the same dark gray background as Apple's complications when seen in a tinted presentation. Previously, I could not get this to work with ClockKit. I could show the dark gray background on a full color watch face, but not tinted. It is WidgetKit's `AccessoryWidgetBackground` that provides the gray background.

A comparison of the ClockKit complications (left) vs the WidgetKit complications (right) on an Infograph face with orange tint color:

![Screenshot of watch faces showing complications](/assets/clockkit-to-widgetkit/old-vs-new.png)

I also took this opportunity to implement a rectangular complication, which is used on the Modular and other watch faces. I had avoided creating this complication in the past because it's a lot of space to fill for a simple complication that only launches the app. However, watchOS 10 introduced the Smart Stack feature, which is a list of widgets revealed when you swipe up on the watch face (or scroll up on the crown). If you implement the rectangular complication, using WidgetKit's `accessoryRectangular` family, then that same widget is available as a complication on some watch faces or as a widget in the Smart Stack. As a watchOS 10 user myself, I've found the Smart Stack allows me to use a more minimalist watch face with few complications yet still have quick access to widgets and the info they provide. So I definitely wanted to add Smart Stack support to the app. I kept it simple with another static complication featuring the app name and a "tap to open" label (similar to other apps' rectangular complications).

Screenshots of the new rectangular complication in various presentations as the Smart Stack (bottom right):

![Screenshots of the rectangular complication](/assets/clockkit-to-widgetkit/four-by-four.png)

I also switched the image asset used in the complications from PNG to SVG. With WidgetKit, there is a limit on the size of the image, in pixels used in the complication. For the `accessoryCircular` complication, this limit is 120x120 pixels. This resulted in a fuzzy image on the X-Large watch face, which also uses `accessoryCircular` but displays the complication much larger than other watch faces. With an SVG, I was able to use the same asset across watch faces.

Putting it all together, here is the SwiftUI view for the `accessoryCircular` complication. The code uses the `AccessoryWidgetBackground` to get the gray background and the SVG image from the asset catalog. The `widgetLabel` modifier shows the text on certain watch faces that make use of it, such as Infograph.

```
struct CircularView: View {
    var body: some View {
        ZStack {
            AccessoryWidgetBackground()

            Image("Complication")
                .renderingMode(.template)
                .resizable()
                .padding(.horizontal, 10)
                .padding(.vertical, 12)
                .widgetAccentable()
        }
        .widgetLabel {
            Text("Snaplists")
        }
    }
}
```

In addition to the new complications, I also made some changes to the layout of app's home screen and the list details screen to make it "more wachOS 10". I replaced the deprecated `NavigationView` the app was still using with a `NavigationStack`, and I used `ToolbarItem` to move the buttons for add new list, new item, and list options to the top and bottom bars.

Before and after screenshots of the app's home screen (left) and list details view (right):

![Screenshots of the app before and after](/assets/clockkit-to-widgetkit/app-old-vs-new.png)

Version 2024.2 of Snaplists, with its WidgetKit-powered complications, is now available in the App Store.

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/snaplists-simple-lists-app/id1527429580)
