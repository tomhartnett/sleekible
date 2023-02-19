---
layout: post
title: "SearchMoji and New Emojis in iOS 16.4"
date: 2023-02-18 00:00:00 -0600
categories:
---

It did not escape my attention this past week that Apple released betas for iOS 16.4 and Xcode 14.3. However it was not immediately obvious to me that iOS 16.4 included new emojis until I saw a post about it on Mastodon. This immediately got my attention because one of my apps, [SearchMoji](/apps/searchmoji/), is emoji-focused. SearchMoji is an "emoji search app and dictionary"[^1], so I consider it important to keep the app up-to-date when new emojis are released.

I had nothing planned this weekend, so I downloaded the Xcode 14.3 beta and began looking at what it would take to get the new emojis into the app. SearchMoji has a Core Data database with all the emoji names, categories, and keyword metadata. Whenever new emojis are added to iOS, I have to update this database with the new emojis' metadata. 

![Screenshot of SearchMoji Core Data model designer in Xcode](/assets/searchmoji-core-data-model.png)

[As I wrote last year](/2022/06/01/searchmoji-returns-to-app-store), I put some effort into streamlining this metadata "ingestion" process. I found some emoji data files[^2] provided by the [Unicode Consortium](https://home.unicode.org/about-unicode/) and wrote a command-line executable in Swift to parse those files and create the Core Data database for me. 

This year was the first time I could test the ease with which I could add new emojis simply by updating those Unicode data files with new versions. I'm pleased to say the process was pretty smooth. I have a readme file in the command-line project that provides a list of the input files (provided by Unicode) and other info about running the program. The hardest parts were finding the updated versions of the input files and interpreting the command-line program's output (to determined if it worked or not!). But, the program ran without issues on the first attempt. I was then able to output a new Core Database, add it to the app, and test the app in the simulator. The whole process took less than an hour.

![Screenshot of SearchMoji showing new emojis](/assets/searchmoji-16.4-emojis.png)

Of course, iOS 16.4 hasn't shipped yet and Xcode 14.3 is still in beta, so I can't submit to the App Store yet. I'll have to wait a while before I can ship this update. In the meantime, perhaps I'll make a few more updates the app.

---

[^1]: _That text is in the actual title and subtitle of the [app in the App Store](https://apps.apple.com/mk/app/searchmoji-emoji-search-app/id1067703384)._
[^2]: _I'm currently using files from Unicode's [cldr-json repo on GitHub](https://github.com/unicode-org/cldr-json) and this [emoji-test.txt file](https://www.unicode.org/Public/emoji/15.0/emoji-test.txt) as inputs to the command-line program._