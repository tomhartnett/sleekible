---
layout: post
title: "Fake Toolbar on iOS 16"
date: 2022-09-11 00:00:00 -0500
categories:
---

_**Update 1 Oct 2022:** the workaround described in this post was not as great as I'd hoped. I have since replaced this workaround with another workaround, as described in [this blog post](/2022/10/01/scrollviewreader-crashing-on-ios16.html)._

---
<br/>
Since the first beta version of Xcode 14 was released in early June, I have been aware of an issue with the [Snaplists app](/apps/snaplists/) on iOS 16. On the list detail screen, an **Actions** button is shown at the bottom. This button is actually a SwiftUI `Menu` placed inside a `ToolbarItem` (in a `toolbar`) using `.bottomBar` placement. 

Here's a screenshot of the Actions button and the menu of list actions that is shown when it's tapped.

![screenshot of toolbar](/assets/ios-15-real-toolbar.jpg)

On iOS 16, the bottom toolbar is missing! Here is a screenshot from the iOS 16 simulator showing the missing toolbar.

![screenshot of missing toolbar](/assets/ios-16-missing-toolbar.jpg)

So I was aware of the issue since early June, but I did nothing all summer to try and fix it because I was hoping it was a bug in the iOS 16 beta and would be fixed for me by Apple. As each new beta version of Xcode 14 was released, I used it to build the app and re-test, but the issue was never resolved. I did try the new [toolbar(_ visibility: Visibility...)](https://developer.apple.com/documentation/swiftui/view/toolbar(_:for:)) view modifier which from the name alone seems like it might help, but it did not. Finally, the Xcode 14 RC was released on September 7 (the day of the Apple event announcing new iPhones and Apple Watch). The issue was still not resolved, so I knew then that I needed to do something to fix it before iOS 16 shipped on Sept 12.

I did file a feedback (FB11461006) on Sept 7, but I don't expect to hear of a fix any time soon. Snaplists uses the SwiftUI `NavigationView`, which has been deprecated with iOS 16. I created a simple test project for my feedback using a NavigationView. The issue is easily reproducible with just two views where one uses a NavigationView and NavigationLink to show the other view, which has a toolbar. I also created another test app using the new `NavigationStack` (instead of a NavigationView) on iOS 16 and it displayed the toolbar as expected.

_This is speculation on my part, but perhaps the toolbar doesn't work well with NavigationView on iOS 16 and Apple just isn't going to fix it?_

I decided to employ a workaround (sometimes called a "hack") to resolve the issue. I removed the `toolbar` and replaced it with an `HStack`. This "fake toolbar", as I call it, works well enough in my opinion on both iOS 15 and iOS 16.  [Here's a link to the PR](https://github.com/tomhartnett/Snaplists/pull/18) if you want to see the change in more detail.

The real toolbar (left) next to the fake toolbar (right). _The workaround also looks fine on iOS 16 (not pictured)._

![screenshot of toolbars](/assets/ios-15-toolbars-comparison.jpg)

Long-term, it's clear to me that I need to migrate to the new `NavigationSplitView` that is available with iOS 16+. In fact, in the process of implementing the workaround described in this blog post, I did spend some time to experiment with this newer view. However, I want to continue to support iOS 15 for now, and also don't want to rush a change like that. I was somewhat short on time this last weekend before iOS 16 ships, so I decided to use the "fake toolbar" as a quick fix for now.

_Note: there is also the new `NavigationStack` that would work on iPhone, but I need a split view on iPad. The NavigationSplitView will collapse down to a single view on iPhone, so I think that's the way to go._

Version 2022.8 of Snaplists, with this fix for the missing toolbar on iOS 16, is now available in the App Store.

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/snaplists-simple-lists-app/id1527429580)