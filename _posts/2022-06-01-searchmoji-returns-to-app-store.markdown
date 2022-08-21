---
layout: post
title:  "SearchMoji Returns to the App Store"
date:   2022-06-01 00:00:00 -0500
categories:
---

[SearchMoji](/apps/searchmoji/) is once again in **Ready for Sale** status in the App Store. I removed the app from sale last year. Honestly, working on the app no longer appealed to me and I decided it was time to retire it. However, since then, I found myself periodically opening the app on my own phone (I still had it installed), and the thought of it not being available any more bothered me. 

After some time, I decided to bring it back, but under some conditions:
- I would remove the ads and the Google AdMob SDK from the app.
  - The ads provided basically no revenue and this was the last 3rd party SDK in the app.
  - Additionally, as it wasn't earning anything, I decided I would prefer the more customer friendly **Data not collected** privacy label in the App Store.
- I had to improve the emoji-database-build process to reduce the amount of manual work needed to add the new emojis each year.
  - Every year when new emojis are added to iOS, I have to update the app to include the new emojis and their names and keywords in the app's database.
  - I've tried to make this process as streamlined as possible but it still involved some manual curation of metadata.

I was able to accomplish the above goals. The app no longer shows ads and does not contain any 3rd party code. I also removed the IAP to remove ads from the app. I replaced it with a "tip jar" IAP. I have yet to see anyone actually give a tip 🤷🏻‍♂️
  
I reworked the database-build process to use some publicly-available data files from the Unicode Consortium. A command line executable I wrote parses these files and builds the Core Data / SQLite database. There is no manual entry of any data. It is yet to be tested, but the process should require just "dropping" in the new files later this year when new emojis are added to iOS 🤞

With these changes, I hope to keep the app updated for the foreseeable future. Since emoji-search was added to the system keyboard in iOS 14, the need for this app has definitely decreased. However, I still occasionally find myself wanting to open up an emoji to see a larger view, read the title, etc. The app is and will probably remain in support-mode / KTLO going forward, but I plan to keep it updated with new emoji and available in the App Store.

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/searchmoji-emoji-search-app/id1067703384)