---
layout: post
title: "Snaplists Drag and Drop and More"
date: 2023-06-02 00:00:00 -0500
categories:
---

With the latest update of Snaplists, version 2023.2, I finally required iOS 16 and watchOS 9. I did this so I could use the `Transferable` protocol in SwiftUI to implement drag and drop. Of course it would have been possible to implement drag and drop using the older `NSItemProvider` APIs and keep iOS 15 support, but implementing drag and drop is really easy with SwiftUI and Transferable. Also, with iOS 17 to be announced in a few days on June 5, I felt the time was right to require iOS 16.

The basic steps were:
1. Create an Exported Type in the info.plist
2. Make the type you want to drag and drop conform to `Transferable`
3. Add the `.draggable` and `.dropDestination` modifiers to your views in SwiftUI

The WWDC video [Meet Transferable](https://developer.apple.com/wwdc22/10062) covers the topic.

A demo of drag and drop on iPad:

![Demo of drag and drop on iPad](/assets/ipad-drag-and-drop.gif)

Another optimization I added in 2023.2 was `TextFieldLink` in the watchOS app. [TextFieldLink](https://developer.apple.com/documentation/swiftui/textfieldlink) can be rendered like a button and when tapped will go directly into text input mode on the watch. This is useful because it saves the user a step when adding a new item to a list in the watch app. The user taps on the Add button, and the app immediately prompts for the item's name. Previously, the app was presenting a screen with an item name `TextField` and the user then had to tap into that TextField. `TextFieldLink` is available in watchOS 9.

A Series 5 on watchOS 8 showing the extra step required to enter text vs the Series 8 on watchOS 9 with `TextFieldLink`:

![Demo of TextFieldLink on Apple Watch](/assets/textfieldlink-demo.gif)

Another feature I added to the Snaplists watch app in 2023.2 is the ability to edit an item's text after creating it. There was no way to do this in the watch app previously. To accomplish this, I added a long-press gesture. This gesture will open a screen with a text field where the item text can be updated. But, a challenge was that tapping on the item should still mark it as complete, so we need both a tap gesture and a long-press gesture on the item. My first attempt at this did not work. One gesture would replace the other. I solved this with SwiftUI's `.highPriorityGesture` & `.simultaneousGesture` modifiers.

```
HStack { ... } // Contains the item view
.highPriorityGesture(
    TapGesture()
        .onEnded { _ in
            // Handle the tap
        }
)
.simultaneousGesture(
    LongPressGesture()
        .onEnded { _ in
            // Handle the long press
        }
)
```

The watch app handling both a tap and a long-press gesture:

![The watch app handling both a tap and a long-press gesture](/assets/tap-long-press-demo.gif)

Now that the app requires iOS 16 and watchOS 9, I can think about other new SwiftUI features to adopt. I have had `NavigationStack` (for the watch) and `NavigationSplitView` (for iPhone/iPad) on my TODO list for a while. Plus, I'm now getting a deprecation warning related to the `NavigationView` the app has been using since launch. But now's not the time to start on that. I'll be watching WWDC next week to see what's new first before beginning any new development efforts 🙂

Version 2023.2, with drag and drop on iOS and improved item entry in the watch app, is now available in the App Store.

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/snaplists-simple-lists-app/id1527429580)
