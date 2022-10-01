---
layout: post
title: "ScrollViewReader Crashing on iOS 16"
date: 2022-10-01 00:00:00 -0500
categories:
---

Recently, while testing a new feature in the Snaplists app, it crashed on me. It crashed after entering a few items on the list screen. I re-opened the app, entered a few more items, and the app again crashed. At first I assumed I had introduced the crash in this development version of the app, but unfortunately the crash was reproduceable in the shipping version of the app as well.

The crash appears to be due to an issue with SwiftUI's `ScrollViewReader` and its `scrollTo` method on iOS 16. I found [others complaining of the same issue](https://developer.apple.com/forums/thread/712510) in the Apple developer forums. I was using `scrollTo` to programmatically scroll the list of items as the user enters items. This kept the text field for entering new items above the keyboard.

I could find no way to make the scrollTo method not crash, so I stopped using it and ScrollViewReader altogether. Instead, I used the new `@FocusState` property wrapper available since iOS 15 to programmatically keep the focus on the text field as the user is entering items. This way of doing it works well enough though it is a little wonky in that upon entering an item, the keyboard dismisses, then reappears.

![animated gif](/assets/entering-items-with-focusstate.gif)

Another issue I noticed around this same time was with the toolbar on the list screen. Previously, [I wrote about the "fake toolbar" workaround](/2022/09/11/fake-toolbar-on-ios16.html) I used to overcome a SwiftUI issue where the toolbar had disappeared from the Snaplists app. Unfortunately, this workaround was not as good as I'd hoped because the fake toolbar would lift up with the keyboard. I tried various changes to the view hierarchy in combination with `ignoresSafeArea(.keyboard)` to allow the keyboard to appear in front of the toolbar, but I couldn't get it to work just right. I removed the toolbar and replaced it with a "more" button at the top-right of the screen.

![screenshot of toolbar issue](/assets/replaced-fake-toolbar.png)

Finally, the actual feature I was actually trying to implement when discovering these bugs was the addition of colors to the app. Now, a list can be assigned a color. The color is shown next to the name of the list, and the user can choose from several colors.

![screenshot of list colors](/assets/list-color-options.png)

This feature required a "lightweight" migration of the app's Core Data model. Each list's color option is stored as a new attribute on the list entity.

Version 2022.9 of Snaplists, with these bug fixes and new color options, is now available in the App Store.

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/snaplists-simple-lists-app/id1527429580)