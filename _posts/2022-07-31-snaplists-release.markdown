---
layout: post
title: "Snaplists Drops Support for iOS 14"
date: 2022-07-31 00:00:00 -0500
categories:
---

Version 2022.6 of Snaplists, which is now available in the App Store, drops support for iOS 14 and watchOS 7. Mostly I dropped support for iOS 14 because I wanted to use the new [ButtonRole parameter](https://developer.apple.com/documentation/swiftui/buttonrole) available to SwiftUI in iOS 15 when creating a button. This parameter allows you to designate the button as `.destructive`. This will cause SwiftUI to render the button in red. I applied this role to all delete buttons in the app. 

Of course, I could have simply used an `if #available(iOS 15, *)` macro to test for iOS 15 and then use the role, but analytics in App Store Connect showed little to no iOS 14 usage in the past 90 days, so I decided to drop support. In fact, I already had a couple `#available` macros in the app used as workarounds to deal with some undesirable spacing issues with `ToolbarItem` on iOS 14. Now that the app requires iOS 15, those macros are no longer needed so I deleted them.

In addition to adding the `.destructive` role, I also added a confirmation prompt to all delete actions. To do this, I used the new `confirmationDialog` which requires iOS 15.

Example of the `confirmationDialog` and `.destructive`  in use:
```
.confirmationDialog("Permanently delete all?",
                    isPresented: $isPresentingDelete,
                    titleVisibility: .visible
) {
    Button("Delete", role: .destructive) {
        storage.purgeDeletedLists()
        getArchivedLists()
    }
} message: {
    Text("This action cannot be undone")
}
```
_Note: I don't know why it's required to pass `titleVisibility: .visible` but the title will not be displayed without it. Seems like it should be displayed by default to me_ 🤔

And the resulting dialog as seen on the iPhone 13 simulator:

![destructive button screenshot](/assets/confirmationdialog-destructive.jpg)

This might be the last release of Snaplists developed with Xcode 13. I'm hoping to start work soon on features using the new APIs available in iOS 16 and Xcode 14. Hopefully, these red delete buttons and confirmation prompts leave the app in a better state for anyone not upgrading from iOS 15 right away. 

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/snaplists-simple-lists-app/id1527429580)
