---
layout: post
title: "Tip Jar IAP"
date: 2024-01-22 00:00:00 -0600
categories:
---

Since its initial release in 2021, the Snaplists app has had an in-app purchase to unlock all app functionality. Without the IAP, the app limited users to only three lists with a max of ten items each. Adding a fourth list (or eleventh item) required a purchase to unlock. It was a non-consumable IAP meaning that the user only needed to purchase it once. In the latest release of the app, I removed this IAP and the code enforcing the free limits, so now anyone that installs the app gets the full functionality for free. This change will make it easier if I ever decide to stop developing the app and remove it from the store. No one will have just paid to unlock it.

I removed the Premium IAP, but I added a "tip jar" with small, medium, and large IAPs. In the US, these are $0.99, $1.99, and $3.99. Unlike the Premium IAP, these are consumable IAPs meaning the user can purchase them more than once. Just like in real life: you can tip someone more than once for the same service. I use iCloud key-value storage, `NSUbiquitousKeyValueStore`, to keep track of the user's lifetime tip amount and last tip date. I don't expect many users to give a tip, but it's nice to have the option if anyone wants it. 

For this feature, I mostly dropped existing code from the SearchMoji app into the project. I had previously developed this feature for that app. It required only minor changes to adapt it for Snaplists. These changes included some user-facing copy on the screen and the product identifiers used to retrieve the IAPs from Apple.

Screenshot of SearchMoji tip jar screen (left) and Snaplists (right):

![Screenshot of tip jar views from SearchMoji and Snaplists](/assets/tip-jars.png)

IAPs are reviewed by App Review kind of like app updates. The tip jar IAPs were approved without any issue. _This was also the case when I added them to SearchMoji._ However, the timing of the approvals was a little off. When you submit new IAPs for review in App Store Connect, that review process is separate from your app's review. The Snaplists app update using the tip IAPs was approved before the IAPs were approved. The IAPs were approved about a day later. I did not release the app update until the IAPs were approved. I assume if I had, then the tip jar screen would have simply failed to load the IAPs (a condition I do handle gracefully). But after the IAPs were approved, I released the app update and saw no issues with tip functionality. 

Another callout is that the IAP review process is much less communicative than an app update's review. There wasn't the series of emails/notifications for all the statuses (waiting for review, in review, approved) as there are when an app update goes through the process. Only a single email was sent when the IAPs were approved. However, I could see their status in App Store Connect and it did update as they went from waiting for review, to in review, to approved.

The approval email:

![Screenshot of email inbox showing IAP approved email](/assets/iap-email.png)

Version 2024.1 is now available in the App Store.

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/snaplists-simple-lists-app/id1527429580)
