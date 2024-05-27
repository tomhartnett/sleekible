---
layout: post
title:  "Removing apps from the App Store"
date:   2024-05-27 00:00:00 -0500
categories:
---

Today I removed my two apps from sale in the App Store. This means they will no longer appear in the App Store and new users will not be able to download them. I did this because I've lost interest in working on both these apps and I don't feel like maintaining them any longer. I think it's better to remove them now while they both still work vs. later when one or both have issues.

To remove the apps, I first drafted this blog post. Then I updated other posts on the blog to remove any App Store links and instead link to this post. Then I went into App Store Connect, removed the apps from sale, and then published the blog updates.

[SearchMoji](/apps/searchmoji) was actually a useful app when I first launched it in 2016. At the time, the system keyboard on iOS did not yet have emoji-search. You had to scroll through the hundreds of emojis to find the one you were looking for. I originally had the idea of creating a 3rd-party keyboard with emoji-search. 3rd party keyboards were new with iOS 8 at that time, and I started the project with that as the goal. However, creating a 3rd-party keyboard turned out to be harder than I expected[^1], so I just created an app that let you search for emojis by keyword and copy them to the clipboard. Eventually, I did ship a keyboard, but later removed it after emoji-search was added to the system keyboard.

I'm not sure I can remember the original reason I started working on [Snaplists](/apps/snaplists/), my list-making/todo app. There was definitely a desire to try out the new NSPersistentCloudKitContainer, which handles syncing Core Data via iCloud for you. This was introduced at WWDC 2019, which I attended in person. I had previously implemented sync with Core Data and CloudKit in an unpublished app, and it was not easy to do. Another motivation for the app was wearing masks in public at the beginning of the pandemic. I wanted a simple shopping list app that I could access on my Apple Watch and avoid the need to unlock my iPhone with Face ID (which did not work with a mask on). I started working on Snaplists in 2020 and launched it in early 2021. I enjoyed working on this app as it spanned watchOS, iOS, and iPadOS. However, these days I tend to just use Apple Notes and Reminders, which are both very good.

---

[^1]: _When you add a keyboard extension to your Xcode project, you just get a blank view with a single button to switch to the next keyboard. As the developer, it's on you to implement everything: the keys, animations when you tap on them, updating the input, etc. Also, the system keyboard has a lot of "smarts" to handle typos, suggest words, move the cursor around, etc. You get none of that for free! It's very challenging to create a "good" keyboard._
