---
layout: post
title: "SearchMoji Version 2022.3 Released"
date: 2022-07-26 00:00:00 -0500
categories:
---

SearchMoji version 2022.3 has been released in the App Store. This update brings some UI polish with several tweaks made to the app's Dark Mode support. These adjustments provide a more consistent experience across screens. Screens and table view cells now have consistent background colors. 

I also refactored (replaced really) a couple screens to get rid of their .xib implementations. One screen (the "More" screen) was just a table view controller without any custom table view cells, so I simply created an all-programmatic UI with `UITableViewController`. For another screen (the "About" screen), I started to do the same when I realized a SwiftUI view would work just fine. I actually took the SwiftUI `AboutView` from the [Snaplists app](/apps/snaplists/) and dropped it in SearchMoji. With minor tweaks to change the displayed app name and logo, the view worked great.

Just for fun, I added a "Featured Emoji" to the More screen. This screen displays options for submitting app feedback, viewing the privacy policy etc. After the app's [re-release to the App Store](/2022/06/01/searchmoji-returns-to-app-store.html) earlier this year, the screen had a large amount of empty space where the IAP to remove ads used to be (the app no longer has ads). The "Featured Emoji" feature displays a different emoji every hour. 

It was fun coming up with how to choose a different emoji each hour to display. Basically, I am calculating the number of hours elapsed since the beginning of the year. Using that number as an index to select an emoji from the list of all emojis (as returned by the app's search function). There are actually more hours in a year than the number of emojis returned by the search, so the featured emoji will actually wrap around and start over a few times per year.

![Featured Emoji screenshot](/assets/featured-emoji.jpeg)
