---
layout: post
title: "Snaplists Version 2022.5 Released"
date: 2022-07-21 00:00:00 -0500
categories:
---

Today version 2022.5 of Snaplists was released in the App Store. This is mostly a bug-fix release, and mostly just one bug that is being fixed. I don't know when the bug was introduced (or spontaneously came into being with an iOS update 🤔). Perhaps it was present since 1.0 of the app and I never noticed. 

This was a particularly nasty bug:
while editing text in almost any text field in the app, if you were trying to insert/change characters somewhere within the text, the cursor would "jump" to the end of the text when you made a change. So, it was very frustrating to make changes at the beginning or in the middle of some text.

The issue was related to how the text was being set on the `UITextField` in `updateUIView(_:context:)` method of a `UIViewRepresentable` implementation. 

_(I wrap a `UITextField` mostly to allow me to change the `returnKeyType` to `.done`)._

Here was the code:
```
func updateUIView(_ uiView: UITextField, context: Context) {
    uiView.attributedText = NSAttributedString(string: text, attributes: textAttributes)
}
```

So, every time this method was invoked, the text was being set on the text field and the cursor was losing its position and "jumping" to the end. Luckily, the fix was easy. I just save the cursor location first, then set the text, then restore the cursor position:
```
func updateUIView(_ uiView: UITextField, context: Context) {
    let selectedRange = uiView.selectedTextRange
    uiView.attributedText = NSAttributedString(string: text, attributes: textAttributes)
    uiView.selectedTextRange = selectedRange
}
```

I was clued into this issue & fix by [this Stack Overflow post](https://stackoverflow.com/questions/65270192/cursor-always-jumps-to-the-end-of-the-uiviewrepresentable-textview-when-a-newlin).

Aside from this issue, I also made a tweak to the layout of the app's large widget. I reduced the number of lists/items that can be visible on the widget to avoid clipping content.

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/snaplists-simple-lists-app/id1527429580)
