---
layout: post
title: "Snaplists Watch App Updates"
date: 2023-04-16 00:00:00 -0500
categories:
---

My original motivation for making the Snaplists app (I started working on it in late summer, 2020) was to create a simple list app for the Apple Watch. I wanted a way to easily access a shopping list from my watch while in the store. I didn't like repeatedly retrieving my phone from pocket and unlocking it. This desire, combined with the ease of syncing Core Data with the relatively new `NSPersistentCloudKitContainer`[^1], led to Snaplists: a very basic to-do list app for iOS and watchOS.

But while the overall app was originally created out of desire for a convenient watch app, the watch app's features have been lagging behind the iOS app. Recently, in the iOS app, I added the ability to assign a color to a list for categorization purposes. The watch app did display this color, but there was no way to change it on the watch. Also, the iOS app has a menu on the list screen with options for marking all items complete or incomplete, and there are options to delete all items or all completed items. These features were missing from the watch.

A week ago I started working on these features in the watch app. It all came together relatively quickly and easily. I just added a "More" button to the list screen using the ubiquitous `ellipsis.circle` SF symbol that I also use in the iOS app to indicate "more actions available here". This More button opens a new screen with the mark all/delete all buttons. There's also a link to another screen where the user can change the list's name and color. The menu options and copy are consistent with the iOS app.

![Screenshots of the watch app's new features](/assets/snaplists-2023-1-watch-app.png)

Under the hood I also cleaned up the project organization a bit. From the earlier days of watchOS development, I still had separate _WatchApp_ and _WatchApp Extension_ groups in the project navigator (and underlying folders on the file system). These separate folders are related to how watchOS apps had separate targets for the watch app and a watch app extension. In the recent past, Xcode started offering to consolidate these targets into one, but it still left behind the separate folders and some redundant files. There were two asset catalogs and two Info.plist files. From what I could tell, only one of the Info.plist files was in use. Both assets catalogs were in use with one having the AppIcon images and the other watch complication images. I cleaned all this up so that the watch app now has only one Info.plist, one asset catalog, and all the files are in the same group/folder named just _WatchApp_. This of course inflated the number of files changed in the PR I opened to merge this into the main branch, but the majority of the file "changes" were really just moves into the WatchApp folder.

Before & after of Xcode's project navigator showing the cleanup:
![Screenshot of Xcode project navigator](/assets/snaplists-2023-1-project-navigator.png)

Version 2023.1, with the new list-editing options in the watch app, is now available in the App Store.

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/snaplists-simple-lists-app/id1527429580)

---

[^1]: Several years ago, I implemented sync via CloudKit for another Core Data app I was developing (but never published to the App Store). It was somewhat challenging to get the syncing to work right and I'm not sure it was perfect. `NSPersistentCloudKitContainer` handles the sync for you, and you just have to deal with Core Data. So I was very excited about it when it was announced at WWDC19.
