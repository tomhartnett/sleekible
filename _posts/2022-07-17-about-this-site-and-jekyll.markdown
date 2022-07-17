---
layout: post
title: "About this Site and Jekyll"
date: 2022-07-17 00:00:00 -0500
categories:
---

Today I launched this site, which was created with the Jekyll static site generator. There was already a site a the same URL, but it was a hand-coded static html site. For several months now, I've been interested in moving to a static site generator.

Reasons for using a static site generator:
- In the past, I did more web development, but it's been a long time since I described myself as a "web developer".
- I do need a place to host pages related to my iOS apps, and also the idea of writing blog posts about those apps—to annouce major releases and/or just discuss app development—is appealing.
- I liked the idea of a solution that let's me write simple markdown and then commit & push to publish pages as html & css.

I chose Jekyll specifically because of its built-in support for GitHub Pages. For now I am keeping it simple. I used the default theme, minima. I did only minimal overriding of what you get out-of-the-box, mostly to remove things I'm not currently using.

I did encounter some minor troubles getting Jekyll to work locally on my M1 MacBook. It seems not all of the dependencies of the `github-pages` (and other?) gem build successfully on arm64. I overcame this by installing everything in a Terminal running i386 arch (Rosetta). Despite the challenges, it was fun getting Jekyll up & running. It was a nice break from my more typical iOS development.

GitHub's support for Jekyll was very smooth. After pushing a `gh-pages` branch the first time, GitHub automatically set up the repo as a GitHub Pages site and ran the build process. Also, moving my custom domain from the older html site's repo to the new repo worked quickly and on the first attempt. 

For future site improvements, I might explore different themes. For example, it would be nice if the site toggled between light & dark modes depending on the system settings. I hope to continue add posts periodically. Most likely, they'll be related to my iOS apps, but I may throw in other technical posts sometimes.
